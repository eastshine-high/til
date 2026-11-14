# pydantic-settings로 설정 관리하기

## 왜 pydantic-settings인가

앱이 동작하려면 DB 주소, API 키, 타임아웃 같은 값들이 필요합니다.
이 값들은 환경마다 다르기 때문에 코드에 직접 쓰지 않고 **환경변수**로 주입하는 것이 관례입니다.

가장 단순한 방법은 `os.getenv`로 직접 읽는 것입니다.

```python
import os

DB_URL = os.getenv("DATABASE_URL")
TIMEOUT = float(os.getenv("TIMEOUT_S", "30"))  # 타입 변환 직접
MAX_RETRY = int(os.getenv("MAX_RETRY", "3"))    # 반복되는 패턴
```

이 방법의 문제:

- 타입 변환(`int`, `float`, `bool`)을 매번 직접 해야 합니다
- 필수값이 빠져도 그 코드가 실행되는 순간까지 모릅니다
- 로컬 개발용 `.env` 파일 로딩, 환경변수 우선순위를 직접 구현해야 합니다
- 설정이 코드 여기저기 흩어집니다

pydantic-settings는 이 문제를 선언적으로 해결합니다.

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str              # 필수 — 없으면 앱 시작 직후 에러
    timeout_s: float = 30.0        # 타입 변환 자동
    max_retry: int = 3

settings = Settings()
```

`DATABASE_URL`, `TIMEOUT_S`, `MAX_RETRY` 환경변수를 읽어서 선언한 타입으로 자동 변환합니다.
타입이 맞지 않으면 ValidationError가 발생합니다.

---

## 환경변수 ↔ 필드 매핑

pydantic-settings는 **필드명을 대문자로 바꿔서** 환경변수를 찾습니다.

```
필드명          →   env var
database_url   →   DATABASE_URL
timeout_s      →   TIMEOUT_S
max_retry      →   MAX_RETRY
```

환경변수 이름을 대문자로 쓰는 것은 유닉스 관례입니다.
pydantic-settings가 이 변환을 자동으로 해주기 때문에 필드명은 파이썬 컨벤션(소문자+밑줄)을 따르면 됩니다.

---

## .env 파일

매번 환경변수를 직접 export하기 번거롭기 때문에 로컬 개발 시에는 `.env` 파일을 쓰는 것이 관례입니다.

```bash
# .env
DATABASE_URL=postgresql://localhost/mydb
SECRET_KEY=dev-secret
DEBUG=true
```

`SettingsConfigDict`로 읽어올 파일을 지정합니다.

```python
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
    )

    database_url: str
    secret_key: str
    debug: bool = False
```

우선순위: **실제 환경변수 > .env 파일 > 필드 기본값**

`.env`는 보통 `.gitignore`에 추가해서 저장소에 올리지 않습니다. 대신 `.env.example`을 올려서 어떤 변수가 필요한지 알려주는 것이 관례입니다.

---

## model_config 주요 옵션

```python
model_config = SettingsConfigDict(
    env_file=".env",
    env_file_encoding="utf-8",
    env_ignore_empty=True,   # ENV_VAR= (빈 값) → 없는 것으로 취급, 기본값 사용
    extra="ignore",          # 선언하지 않은 환경변수는 무시
)
```

**`env_ignore_empty=True`**
`DB_PASSWORD=` 처럼 빈 값으로 설정된 환경변수를 없는 것으로 취급합니다.
없으면 빈 문자열 `""`이 그대로 들어가는데, `str | None = None` 필드에 전달되면 ValidationError가 납니다.

**`extra="ignore"`**
시스템 환경변수(`PATH`, `HOME` 등) 중 선언하지 않은 것들을 조용히 무시합니다.
`extra="forbid"`로 바꾸면 미선언 환경변수가 있을 때 에러를 냅니다.

---

## 타입별 환경변수 형식

```python
class Settings(BaseSettings):
    # bool: "true"/"false", "1"/"0", "yes"/"no", "on"/"off" 모두 인식
    debug: bool = False

    # list: JSON 배열 문자열로 넘깁니다
    allowed_hosts: list[str] = ["localhost"]
    # ALLOWED_HOSTS='["localhost", "example.com"]'

    # Optional: 없으면 None
    smtp_host: str | None = None
```

---

## validator — 환경변수를 읽은 뒤 처리

환경변수를 읽어온 값을 그대로 쓰기 어려운 경우 validator로 가공합니다.

### `@field_validator` — 단일 필드 변환

```python
from pydantic import field_validator
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str

    # "postgres://" → "postgresql+psycopg://" 로 드라이버 접두사 교체
    @field_validator("database_url", mode="before")
    @classmethod
    def _fix_db_scheme(cls, v: str) -> str:
        return v.replace("postgres://", "postgresql+psycopg://", 1)
```

`mode="before"` — pydantic 타입 변환 전에 먼저 실행됩니다. 원시 문자열을 받아서 고칩니다.

### `@model_validator(mode="after")` — 여러 필드를 조합

한 필드의 기본값이 다른 필드에 의존할 때 사용합니다.
모든 필드가 로드된 뒤 실행되기 때문에 `self`로 다른 필드에 접근할 수 있습니다.

```python
from pydantic import model_validator
from typing import Self

class Settings(BaseSettings):
    project_name: str = "MyApp"
    emails_from_name: str = ""

    @model_validator(mode="after")
    def _set_email_sender_name(self) -> Self:
        if not self.emails_from_name:
            self.emails_from_name = self.project_name  # 다른 필드 참조
        return self
```

### `@computed_field` — 저장하지 않는 파생값

환경변수로 직접 설정하는 것이 아니라 다른 필드들로부터 유도되는 읽기 전용 값에 씁니다.
`model_dump()`에도 포함됩니다.

```python
from pydantic import computed_field

class Settings(BaseSettings):
    smtp_host: str | None = None
    emails_from_email: str | None = None

    @computed_field
    @property
    def emails_enabled(self) -> bool:
        return bool(self.smtp_host and self.emails_from_email)
```

---

## 싱글톤 패턴

Settings는 앱 전체에서 하나만 만드는 것이 보통입니다.

**방법 1 — 모듈 최상위에서 직접 인스턴스화**

```python
settings = Settings()  # 모듈 임포트 시 1회 생성
```

간단합니다. 단점은 테스트에서 환경변수를 바꾼 뒤 새 Settings를 만들기 어렵습니다.

**방법 2 — `lru_cache`**

```python
from functools import lru_cache

@lru_cache(maxsize=1)
def get_settings() -> Settings:
    return Settings()

# 테스트에서 환경변수를 바꾸고 싶을 때
get_settings.cache_clear()
```

테스트에서 환경변수를 조작하고 `cache_clear()`를 호출하면 다음 호출 시 새 Settings가 만들어집니다.
FastAPI의 `Depends(get_settings)`와도 자연스럽게 연결됩니다.

---

## 정리

| 역할              | 방법                                        |
| --------------- | ----------------------------------------- |
| 환경변수 → 필드 자동 매핑 | `BaseSettings` 상속                         |
| 타입 변환           | 필드 타입 선언 (`int`, `bool`, `str \| None` 등) |
| `.env` 파일 로드    | `SettingsConfigDict(env_file=...)`        |
| 단일 필드 가공        | `@field_validator`                        |
| 필드 간 의존 기본값     | `@model_validator(mode="after")`          |
| 파생 읽기 전용값       | `@computed_field`                         |
| 필수값 강제          | 기본값 없이 선언                                 |
| 싱글톤             | 모듈 직접 인스턴스 or `lru_cache`                 |

참고: [FastAPI 공식 템플릿 config.py](https://github.com/fastapi/full-stack-fastapi-template/blob/master/backend/app/core/config.py)
