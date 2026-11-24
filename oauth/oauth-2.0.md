# OAuth 2.0 기초 — 비밀번호를 주지 않고 권한을 위임하기

## 어떤 문제를 푸는가

사진 인화 서비스가 내 클라우드 드라이브의 사진을 가져와야 한다고 해 봅시다. OAuth 가 없던 시절에는 방법이 하나뿐이었습니다.
**인화 서비스에 내 드라이브 ID 와 비밀번호를 알려 주는 것**입니다.

| 문제 | 설명 |
|---|---|
| 비밀번호 노출 | 인화 서비스가 내 비밀번호를 저장한다. 그 서비스가 털리면 내 계정이 털린다 |
| 권한이 너무 넓음 | 사진만 읽으면 되는데 메일·파일 삭제까지 모든 권한을 갖는다 |
| 철회가 어려움 | 이 서비스만 끊으려면 비밀번호를 바꿔야 하고, 그러면 다른 서비스도 다 끊긴다 |
| 기간이 없음 | 한 번 알려 주면 비밀번호를 바꿀 때까지 계속 쓸 수 있다 |

OAuth 2.0([RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749), 2012)은 비밀번호 대신 **범위(scope)와 수명이 정해진 토큰**을 주는 방식으로 이 문제를 풉니다.

> OAuth 는 **인가(authorization) 위임** 프레임워크입니다.
> "이 앱이 내 대신 이 API 를 이 범위만큼 호출해도 된다"를 표현하는 것이 목적이고, "이 사람이 누구인가"(인증)는 OAuth 의 목적이 아닙니다. 이 차이는 마지막 절에서 다시 봅니다.

---

## 네 가지 역할

| 역할 | 하는 일 | 예 |
|---|---|---|
| **Resource Owner** | 자원의 주인. 접근을 허락한다 | 나(사용자) |
| **Client** | 사용자를 대신해 자원에 접근하려는 앱 | 사진 인화 서비스 |
| **Authorization Server** | 사용자를 인증하고 동의를 받아 토큰을 발급 | 클라우드 회사의 로그인 서버 |
| **Resource Server** | 토큰을 확인하고 자원을 제공 | 클라우드 드라이브 API |

Authorization Server 와 Resource Server 는 같은 서버일 수도, 다른 서버일 수도 있습니다.
"Client" 는 브라우저가 아니라 **앱**을 가리킨다는 점을 헷갈리지 않아야 합니다. 브라우저는 User-Agent 라고 부릅니다.

### 추상 흐름

```mermaid
sequenceDiagram
    participant C as "Client"
    participant RO as "Resource Owner"
    participant AS as "Authorization Server"
    participant RS as "Resource Server"
    C->>RO: (A) 권한 요청
    RO-->>C: (B) Authorization Grant (허락의 증거)
    C->>AS: (C) Grant 제출 + 클라이언트 인증
    AS-->>C: (D) Access Token
    C->>RS: (E) Access Token 으로 요청
    RS-->>C: (F) 보호된 자원
```

핵심은 (B) 의 **Authorization Grant** 입니다. "사용자가 허락했다"는 증거이고, 이것을 토큰으로 바꾸는 방식에 따라 grant type 이 나뉩니다.

---

## 토큰 두 가지

| 토큰 | 누가 받나 | 용도 | 특징 |
|---|---|---|---|
| **Access Token** | Resource Server | API 호출 권한 증명 | 수명이 짧다. 형식은 정해져 있지 않음(불투명 문자열 또는 JWT) |
| **Refresh Token** | Authorization Server | 만료된 access token 을 새로 받기 | 수명이 길다. **Resource Server 로 보내지 않는다** |

```mermaid
sequenceDiagram
    participant C as "Client"
    participant AS as "Authorization Server"
    participant RS as "Resource Server"
    C->>AS: Grant 제출
    AS-->>C: Access Token + Refresh Token
    C->>RS: Access Token
    RS-->>C: 자원
    C->>RS: Access Token (만료됨)
    RS-->>C: 401 invalid_token
    C->>AS: Refresh Token
    AS-->>C: 새 Access Token (+ 새 Refresh Token)
```

access token 을 짧게 두고 refresh token 으로 갱신하는 이유는, **API 마다 돌아다니는 토큰이 털려도 피해 시간을 짧게** 하기 위해서입니다. 오래 사는 refresh token 은 인가 서버하고만 주고받습니다.

### Bearer 토큰

OAuth 2.0 에서 거의 모든 access token 은 **Bearer 토큰**([RFC 6750](https://datatracker.ietf.org/doc/html/rfc6750))입니다. "가진 사람(bearer)이 곧 주인"이라는 뜻이라, 훔친 사람도 그대로 쓸 수 있습니다. 그래서 TLS 가 필수입니다.

```http
GET /photos HTTP/1.1
Host: api.example.com
Authorization: Bearer mF_9.B5f-4.1JqM
```

---

## Scope

scope 는 **토큰이 할 수 있는 일의 범위**입니다. 공백으로 구분한 문자열이고, 이름과 의미는 각 서비스가 정합니다.

```
scope=photos.read profile
```

- 클라이언트가 요청한 scope 와 실제 발급된 scope 는 다를 수 있습니다. 사용자가 일부만 동의하거나 서버 정책으로 줄일 수 있기 때문에, 서버는 달라진 경우 응답에 실제 scope 를 알려 줍니다.
- 리소스 서버는 요청마다 토큰의 scope 를 보고, 부족하면 403 `insufficient_scope` 로 응답합니다.

---

## 클라이언트 등록과 종류

클라이언트는 미리 인가 서버에 등록해 **client_id** 를 받습니다. 등록할 때 redirect URI 도 함께 적습니다.

| 종류 | 뜻 | 예 |
|---|---|---|
| **Confidential** | 비밀값(client_secret)을 안전하게 보관할 수 있다 | 서버에서 도는 웹 앱 |
| **Public** | 비밀값을 보관할 수 없다. 코드가 사용자 기기에 배포되기 때문 | SPA(자바스크립트), 모바일·데스크톱 앱 |

confidential client 는 토큰 엔드포인트에서 자기를 인증합니다. 가장 흔한 방법은 HTTP Basic 입니다.

```http
Authorization: Basic base64(client_id:client_secret)
```

---

## 엔드포인트 세 개

| 엔드포인트 | 위치 | 누가 부르나 | 용도 |
|---|---|---|---|
| **Authorization endpoint** | 인가 서버 | 브라우저 (리다이렉트) | 사용자 로그인·동의, 인가 코드 발급 |
| **Token endpoint** | 인가 서버 | 클라이언트 (서버 간 직접 호출) | grant → 토큰 교환 |
| **Redirection endpoint** | 클라이언트 | 브라우저 (인가 서버가 보냄) | 인가 결과(코드 또는 오류) 받기 |

"브라우저를 거치는 통신(front channel)"과 "서버끼리 직접 하는 통신(back channel)"을 구분하면 각 grant 의 설계가 보입니다.
front channel 은 URL 이 주소창·기록·로그에 남기 때문에 **중요한 것(토큰)을 싣지 않는 것**이 원칙입니다.

---

## Grant type 네 가지

RFC 6749 는 네 가지 grant 를 정의합니다.

| Grant | 쓰임 | 지금은 |
|---|---|---|
| **Authorization Code** | 사용자가 브라우저로 로그인하는 일반적인 경우 | 표준. PKCE 와 함께 |
| Implicit | 백엔드가 없는 브라우저 앱 | 쓰지 않는다 (OAuth 2.1 에서 삭제) |
| Resource Owner Password Credentials | 클라이언트가 ID·비밀번호를 직접 받음 | 쓰지 않는다 (OAuth 2.1 에서 삭제) |
| **Client Credentials** | 사용자 없이 서비스가 자기 이름으로 호출 | 표준 |

여기에 refresh token grant 와, 확장 grant(Device Code, Token Exchange, JWT Bearer 등)가 더해집니다.

### 1. Authorization Code Grant

가장 중요한 grant 입니다. **브라우저로는 짧게 쓰고 버리는 코드만 보내고, 토큰은 서버끼리 받는다**는 것이 핵심입니다.

```mermaid
sequenceDiagram
    participant U as "사용자 (브라우저)"
    participant C as "Client (서버)"
    participant AS as "Authorization Server"
    U->>C: "로그인" 클릭
    C-->>U: 302 → AS /authorize
    U->>AS: GET /authorize?response_type=code&client_id&redirect_uri&scope&state
    AS-->>U: 로그인 · 동의 화면
    U->>AS: 로그인 · 동의
    AS-->>U: 302 → redirect_uri?code=...&state=...
    U->>C: GET /callback?code=...&state=...
    Note over C: state 가 보낸 값과 같은지 확인
    C->>AS: POST /token (grant_type=authorization_code, code, redirect_uri) + 클라이언트 인증
    AS-->>C: { access_token, refresh_token, expires_in }
```

**인가 요청** (브라우저 → 인가 서버)

```http
GET /authorize?response_type=code
  &client_id=s6BhdRkqt3
  &redirect_uri=https%3A%2F%2Fclient.example.com%2Fcb
  &scope=photos.read
  &state=xyz HTTP/1.1
Host: server.example.com
```

| 파라미터 | 뜻 |
|---|---|
| `response_type=code` | 인가 코드를 달라 |
| `client_id` | 등록된 클라이언트 ID |
| `redirect_uri` | 결과를 돌려받을 주소 (등록된 값이어야 함) |
| `scope` | 요청 권한 |
| `state` | 클라이언트가 만든 임의 값. 응답에서 그대로 돌아온다 (CSRF 방지) |

**인가 응답** (인가 서버 → 브라우저 → 클라이언트)

```http
HTTP/1.1 302 Found
Location: https://client.example.com/cb?code=SplxlOBeZQQYbYS6WxSbIA&state=xyz
```

인가 코드는 **짧게 살고(최대 10분 권장) 한 번만** 쓸 수 있습니다.

**토큰 요청** (클라이언트 서버 → 인가 서버, back channel)

```http
POST /token HTTP/1.1
Host: server.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&code=SplxlOBeZQQYbYS6WxSbIA
&redirect_uri=https%3A%2F%2Fclient.example.com%2Fcb
```

**토큰 응답**

```http
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store

{
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "tGzv3JOkF0XG5Qx2TlKWIA"
}
```

왜 코드를 한 번 거치나?

- 브라우저 URL 에는 **코드**만 남고, **토큰**은 서버끼리 받으니 주소창·기록·Referer 로 새지 않습니다.
- 토큰 교환 때 클라이언트를 인증하므로, 코드를 훔쳐도 client_secret 이 없으면 토큰으로 바꿀 수 없습니다.

남는 문제는 **client_secret 이 없는 public client** 입니다. 이 경우 코드를 가로채면 그대로 토큰을 받을 수 있습니다. 이를 막으려고 나온 것이 **PKCE**([RFC 7636](https://datatracker.ietf.org/doc/html/rfc7636))이고, OAuth 2.1 에서는 모든 클라이언트에 필수가 됐습니다.

### 2. Implicit Grant (쓰지 않음)

백엔드 없는 브라우저 앱을 위해, 코드 교환 단계 없이 **인가 응답에 바로 access token** 을 담아 주는 방식입니다.

```http
HTTP/1.1 302 Found
Location: https://client.example.com/cb#access_token=2YotnFZFEjr1zCsicMWpAA&token_type=Bearer&expires_in=3600
```

토큰이 URL(fragment)에 실려 브라우저 기록·확장 프로그램·악성 스크립트로 새기 쉽고, 누구에게 발급된 토큰인지 확인하기도 어렵습니다. 지금은 브라우저 앱도 **Authorization Code + PKCE** 를 씁니다.

### 3. Resource Owner Password Credentials Grant (쓰지 않음)

클라이언트가 사용자 ID·비밀번호를 직접 받아 토큰 엔드포인트에 보내는 방식입니다.

```http
grant_type=password&username=johndoe&password=A3ddj3w
```

OAuth 가 없애려던 "앱에 비밀번호 주기"를 그대로 하는 셈입니다. 원래도 "클라이언트를 아주 신뢰할 때, 다른 방법이 없을 때만"이라는 조건이 붙어 있었고, 지금은 쓰지 않습니다. 다단계 인증(MFA)이나 외부 로그인(SSO)과도 맞지 않습니다.

### 4. Client Credentials Grant

사용자 없이 **클라이언트 자신이 자원의 주인**인 경우입니다. 배치 작업, 서비스 간 호출에 씁니다.

```mermaid
sequenceDiagram
    participant C as "Client (서비스)"
    participant AS as "Authorization Server"
    C->>AS: POST /token (grant_type=client_credentials, scope) + 클라이언트 인증
    AS-->>C: { access_token, expires_in }
```

- confidential client 만 쓸 수 있습니다.
- 보통 refresh token 을 주지 않습니다. 만료되면 같은 방법으로 다시 받으면 되기 때문입니다.

### Refresh Token Grant

```http
POST /token HTTP/1.1
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&refresh_token=tGzv3JOkF0XG5Qx2TlKWIA
```

- 새로 받는 토큰의 scope 는 처음 받은 scope 를 넘을 수 없습니다.
- 인가 서버는 새 refresh token 을 함께 줄 수 있습니다(회전). OAuth 2.1 에서는 public client 에 회전 또는 발신자 묶기가 필수가 됐습니다.

---

## state 와 CSRF

`state` 가 없으면 이런 공격이 가능합니다.

1. 공격자가 **자기 계정**으로 인가를 시작해 인가 코드를 얻는다.
2. 그 코드가 담긴 콜백 URL(`/cb?code=공격자코드`)을 피해자가 열게 만든다.
3. 피해자의 세션이 **공격자 계정**에 연결된다. 피해자가 올린 사진이 공격자 드라이브로 간다.

클라이언트는 인가 요청 전에 추측할 수 없는 `state` 를 만들어 사용자 세션에 저장하고, 콜백에서 돌아온 값과 비교합니다. 다르면 버립니다.

---

## 오류 응답

### 인가 엔드포인트 오류

redirect URI 로 오류를 돌려줍니다.

```http
HTTP/1.1 302 Found
Location: https://client.example.com/cb?error=access_denied&state=xyz
```

| error | 뜻 |
|---|---|
| `invalid_request` | 필수 파라미터 누락, 잘못된 값 |
| `unauthorized_client` | 이 클라이언트는 이 방식으로 코드를 받을 수 없음 |
| `access_denied` | 사용자나 서버가 거부 |
| `unsupported_response_type` | 지원하지 않는 `response_type` |
| `invalid_scope` | 잘못된 scope |
| `server_error`, `temporarily_unavailable` | 서버 문제 |

단, **client_id 나 redirect_uri 자체가 잘못됐다면 리다이렉트하지 않고** 사용자에게 직접 오류를 보여 줍니다. 공격자가 지정한 주소로 사용자를 보내는 오픈 리다이렉트를 막기 위해서입니다.

### 토큰 엔드포인트 오류

보통 400 과 JSON 으로 돌려줍니다.

| error | 뜻 |
|---|---|
| `invalid_request` | 파라미터 오류 |
| `invalid_client` | 클라이언트 인증 실패 (401 일 수 있음) |
| `invalid_grant` | 코드·refresh token 이 무효, 만료, 이미 사용됨, redirect_uri 불일치 |
| `unauthorized_client` | 이 클라이언트에 허용되지 않은 grant |
| `unsupported_grant_type` | 지원하지 않는 grant |
| `invalid_scope` | 잘못된 scope |

### 리소스 서버 오류 (RFC 6750)

| 상황 | 상태 코드 | 응답 헤더 |
|---|---|---|
| 토큰 없음 | 401 | `WWW-Authenticate: Bearer` |
| 토큰 무효·만료 | 401 | `WWW-Authenticate: Bearer error="invalid_token"` |
| scope 부족 | 403 | `WWW-Authenticate: Bearer error="insufficient_scope"` |

---

## OAuth 2.0 이후 — 확장 문서들

RFC 6749 는 일부러 많은 것을 비워 둔 **프레임워크**입니다. 토큰 형식, 토큰 검증 방법, 서버 정보 공개 방법 등은 이후 별도 RFC 로 채워졌습니다.

| 문서                                       | 채운 빈자리                   |
| ---------------------------------------- | ------------------------ |
| RFC 6750 Bearer Token Usage              | 토큰을 API 에 보내는 방법, 오류 응답  |
| RFC 7009 Token Revocation                | 토큰 폐기 엔드포인트              |
| RFC 7519 JWT / RFC 9068 JWT Access Token | 자체 검증 가능한 토큰 형식          |
| RFC 7636 PKCE                            | public client 의 인가 코드 보호 |
| RFC 7662 Token Introspection             | 리소스 서버가 인가 서버에 토큰 유효성 묻기 |
| RFC 8252 Native Apps                     | 모바일·데스크톱 앱의 모범 사례        |
| RFC 8414 Authorization Server Metadata   | 인가 서버의 엔드포인트 정보 공개       |
| RFC 8628 Device Authorization            | TV·CLI 처럼 입력이 불편한 기기     |
| RFC 8693 Token Exchange                  | 토큰을 다른 대상용 토큰으로 교환       |
| RFC 9700 Security BCP                    | 10년간 밝혀진 공격과 대응          |

이 중 필수로 지켜야 할 것들을 한 권으로 합친 것이 [OAuth 2.1](https://github.com/eastshine-high/til/blob/main/oauth/oauth-2.1.md) 입니다.

---

## OAuth 는 인증이 아니다

"구글로 로그인" 같은 기능 때문에 OAuth 를 로그인 프로토콜로 오해하기 쉽습니다. 하지만 access token 은 **"이 토큰으로 이 API 를 부를 수 있다"** 만 말해 줄 뿐, 다음을 보장하지 않습니다.

- 이 토큰이 **우리 앱을 위해** 발급됐는지 (다른 앱이 받은 토큰을 들고 와도 API 호출은 성공한다)
- 사용자가 **언제, 어떻게** 로그인했는지
- 토큰 형식이 정해져 있지 않으니, 사용자 정보를 **표준 방법으로** 얻을 수 있는지

그래서 access token 만으로 "로그인"을 구현하면, 다른 앱에 발급된 토큰으로 우리 앱에 로그인하는 **토큰 치환 공격**에 취약해집니다.

이 빈자리를 채우는 것이 **OpenID Connect(OIDC)** 입니다. OAuth 2.0 위에 "누가 로그인했는가"를 담은 **ID Token**(대상 앱이 명시된 서명된 JWT)과 표준 사용자 정보 엔드포인트를 더합니다.

| | OAuth 2.0 | OpenID Connect |
|---|---|---|
| 목적 | 인가 (API 접근 권한 위임) | 인증 (사용자 신원 확인) |
| 결과물 | Access Token | ID Token (+ Access Token) |
| 받는 쪽 | Resource Server | Client |
| 시작 표시 | `scope=photos.read` | `scope=openid ...` |

"Google 로 로그인" 이 바로 OIDC 입니다. ID Token 의 구조와 검증 방법은 [OpenID Connect(OIDC) — OAuth 2.0 위에 얹은 로그인](https://github.com/eastshine-high/til/blob/main/oauth/oidc.md) 에서 이어서 정리합니다.

---

## 정리

- OAuth 2.0 은 **비밀번호 대신 범위·수명이 정해진 토큰**으로 권한을 위임하는 프레임워크다.
- 역할은 넷: Resource Owner, Client, Authorization Server, Resource Server.
- 핵심 grant 는 **Authorization Code**: 브라우저로는 코드만, 토큰은 서버끼리.
- Implicit·Password grant 는 쓰지 않는다. public client 는 PKCE 로 보호한다.
- `state` 로 CSRF 를 막고, access token 은 짧게, refresh token 은 인가 서버하고만.
- OAuth 는 인증이 아니다. 로그인에는 OIDC 를 쓴다.

## 참고

- [RFC 6749 — The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749)
- [RFC 6750 — The OAuth 2.0 Authorization Framework: Bearer Token Usage](https://datatracker.ietf.org/doc/html/rfc6750)
- [RFC 6819 — OAuth 2.0 Threat Model and Security Considerations](https://datatracker.ietf.org/doc/html/rfc6819)
- [RFC 9700 — OAuth 2.0 Security Best Current Practice](https://datatracker.ietf.org/doc/html/rfc9700)
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)
- [OAuth 2.0 — oauth.net](https://oauth.net/2/)
