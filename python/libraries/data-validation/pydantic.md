# Pydantic

- Pydantic은 **타입 힌트를 이용해 데이터를 검증하고 변환하는** 파이썬 라이브러리이다.
- 클래스에 필드와 타입을 선언해 두면, 들어온 데이터가 그 형식에 맞는지 **런타임에 자동으로 검사**한다.
- 한 마디로 **믿을 수 없는 외부 데이터를 믿을 수 있는 파이썬 객체로 바꿔주는 도구**이다.

[https://docs.pydantic.dev/latest/](https://docs.pydantic.dev/latest/)

## 기본 사용법

- `BaseModel`을 상속한 클래스에 타입 힌트로 필드를 선언한다.
- 변환할 수 있는 값은 선언된 타입으로 자동 변환된다. (예: 문자열 `"25"` → 정수 `25`)
- 타입이 맞지 않으면 어떤 필드가 왜 틀렸는지 알려주는 `ValidationError`가 발생한다.

```python
from pydantic import BaseModel, EmailStr

class User(BaseModel):
    name: str
    age: int
    email: EmailStr

user = User(name="철수", age="25", email="cs@example.com")
print(user.age)  # 25  ← 문자열 "25"를 int로 자동 변환

User(name="영희", age="스물", email="잘못된이메일")
# ValidationError: age는 정수가 아니고, email 형식도 틀렸다고 알려준다
```

> `EmailStr`을 사용하려면 `pip install "pydantic[email]"`로 추가 의존성을 설치해야 한다.

## 직렬화(Serialization)

- `model_dump()`는 모델을 dict로, `model_dump_json()`은 JSON 문자열로 변환한다.

```python
user.model_dump()
# {'name': '철수', 'age': 25, 'email': 'cs@example.com'}

user.model_dump_json()
# '{"name":"철수","age":25,"email":"cs@example.com"}'
```

## 제약 조건과 커스텀 검증

- `Field()`로 값의 범위, 길이 같은 제약 조건을 걸 수 있다.
- `@field_validator`로 직접 만든 검증 로직을 붙일 수 있다.

```python
from pydantic import BaseModel, Field, field_validator

class Product(BaseModel):
    name: str = Field(min_length=1)
    price: int = Field(gt=0)  # 0보다 커야 한다

    @field_validator("name")
    @classmethod
    def strip_name(cls, v: str) -> str:
        return v.strip()
```

## 특징

- **속도**: v2부터는 핵심 로직이 Rust로 작성된 `pydantic-core` 위에서 동작해서 v1보다 훨씬 빠르다.
- **dataclasses와의 차이**: 표준 라이브러리의 `dataclasses`는 타입을 선언만 할 뿐 검증하지 않는다. Pydantic은 선언한 타입대로 실제로 검증하고 변환한다.

## 어디에 쓰이나

- **FastAPI**: 요청과 응답 데이터 검증에 Pydantic을 기본으로 사용한다.
- **설정 관리**: `pydantic-settings`로 환경변수나 `.env` 파일을 타입 안전하게 읽어올 수 있다.
- **외부 데이터 파싱**: 외부 API 응답이나 JSON 파일처럼 형식을 보장할 수 없는 데이터를 안전한 객체로 바꿀 때 사용한다.
- **LLM 출력 구조화**: 모델이 정해진 형식의 JSON을 내도록 스키마를 정의할 때 자주 사용한다.
