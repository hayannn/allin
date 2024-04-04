# camp2022_allin
스마일게이트 개발 캠프 2022 - 윈터 개발 캠프 2기 - All-IN

The project has been initialized.


<br>
<br>

> # 🚨 전체 Team README는 원본 저장소를 참고해주세요! 🚨
> [Team README.md](https://github.com/sgdevcamp2022/allin)

<br>
<br>

## 인증/인가 서버 소개
1. 제공 서비스 및 기능

| 서비스         | Method |       URL               | 인증 | 기능                                                                                                  |
| -------------- | ------ | ----------------------- | ---- | ---------------------------------------------------------------------------------------------------- |
| 회원가입       |  POST  | /api/v1/auth/signup     |   X  | Email, Password, Username, Nickname 정보를 받아 회원가입을 진행한다.                                  |
| 로그인         |  POST  | /api/v1/auth/login      |   X  | Email, Password을 통해 로그인을 진행한다. <br> 로그인 성공 시, AccessToken과 RefreshToken을 발급한다.  |
| 로그아웃       |  GET   | /api/v1/auth/user/logout|   O  | 유효한 AccessToken을 제시하면, 로그아웃을 진행한다.                                                    |
| 토큰 재발급    |  POST  |/api/v1/auth/reissue     |   O  | 유효한 AccessToken과 RefreshToken을 제시하면, 일치 시 새로운 토큰을 발급한다.                          |
| 회원정보 조회  |  GET   | /api/v1/auth/user/me    |   O  | 유효한 AccessToken을 제시하면, Email, username, nickname 정보를 제공한다.                              |
| 회원정보 수정  |  PUT   |/api/v1/auth/user/update |   O  | 유효한 AccessToken + 수정할 nickname  or password을 제시하면, <br> 일치 시 회원 정보를 수정하고, 회원 정보를 제공한다. |
| 회원탈퇴       | DELETE | /api/v1/auth/user/me    |  O   | 유효한 AccessToken을 제시하면, 일치 시 회원 탈퇴 기능을 제공한다.                                       |

```
* 회원 기준 *
- 일반 가입 회원은 ROLE_USER 권한을 부여받습니다.
- 그 외 관리자 회원은 ROLE_ADMIN 권한을 부여받습니다.
```

#### 회원가입
- 회원가입 대상은 비회원(Role 없음)입니다.
- username은 실명이며, nickname은 활동에 사용할 닉네임입니다.
- Email, Password, username, nickname 정보를 받습니다.

#### 로그인
- 로그인 대상은 회원(ROLE_USER, ROLE_ADMIN)입니다.
- 가입 시 사용한 Email, Password 정보를 받아 로그인을 진행합니다.
- 로그인 성공 시 해당 회원의 AccessToken & RefreshToken을 발급합니다.

#### 로그아웃
- 로그아웃 대상은 '로그인 한' 회원(ROLE_USER, ROLE_ADMIN)입니다.
- 가입 시 발급 받은 유효한 AccessToken을 제시하면 로그아웃에 성공합니다.

#### 토큰 재발급
- 토큰 재발급 대상은 유효한 AccessToken을 보유한 회원(ROLE_USER, ROLE_ADMIN)입니다.
- 가입 시 발급 받은 AccessToken & RefreshToken을 제시하면 회원 정보 일치 시 토큰이 재발급됩니다.

#### 회원정보 조회 & 수정
- 회원 정보 접근 대상은 유효한 AccessToken을 보유한 회원(ROLE_USER, ROLE_ADMIN)입니다.
- 가입 시 발급 받았거나, 재발급 받은 유효한 AccessToken을 제시하면 회원 정보 일치 시, 해당 회원의 정보를 제공하며 nickname과 password는 수정할 수 있습니다.(단, 접근 URL은 다름)

#### 회원탈퇴
- 회원 탈퇴 대상은 가입 및 로그인한 회원(ROLE_USER, ROLE_ADMIN)입니다.
- 가입 시 발급 받았거나, 재발급 받은 AccessToken을 제시하면 해당 회원 정보를 찾아 일치 시 삭제합니다.

---

2. 디렉터리 구조
```
sgdevcamp2022/allin
└── backend
    ├── AuthServer7
        ├── docs
        |
        ├── src
            ├── main
            │   ├── java/me/ver/Authserver7
            │   │               ├── Authserver7Application.java
            |   |               |
            │   │               ├── config
            |   |               |
            │   │               ├── controller
            |   |               |
            │   │               ├── domain
            |   |               |
            │   │               ├── dto
            |   |               |
            |   |               ├── jwt
            |   |               |
            |   |               ├── repository
            |   |               |
            |   |               ├── service
            |   |               |
            |   |               ├── util
            │   │               |
            |   |               └── web
            |   | 
            │   └── resources
            │       └── application.yml

```
#### config
```
├── config
│   ├── JwtSecurityConfig.java
│   ├── SecurityConfig.java
│   └──  initDb.java
```

#### controller
```
├── controller
│   ├── AuthController.java
│   └── UserController.java
```

#### domain
```
├── domain
|   ├── Authority.java
|   ├── AuthorityEnum.java
|   ├── RefreshToken.java
|   └── User.java
```

#### dto
```
├── dto
|   ├── TokenRequestDto.java
|   ├── TokenResponseDto.java
|   ├── UserRequestDto.java
|   ├── UserResponseDto.java
|   └── UserUpdateDto.java
```

#### jwt
```
├── jwt
|   ├── JwtAccessDeniedHandler.java
|   ├── JwtAuthenticationEntryPoint.java
|   ├── JwtFilter.java
|   ├── Subject.java
|   └── TokenProvider.java 
```

#### repository
```
├── repository
|   ├── AuthorityRepository.java
|   ├── RefreshTokenRepository.java
|   └── UserRepository.java
```

#### service
```
├── service
|   ├── exception
|   |   ├── AuthServiceException.java
|   |   ├── AuthServiceValidateException.java
|   |   ├── UserServiceException.java
|   |   └── UsreServiceValidateException.java
|   |
|   ├── AuthService.java
|   ├── CustomUserDetails.java
|   ├── CustimUserDetailsService.java
|   └── UserService.java
```

#### util
```
├── util
|   └── SecurityUtil.java
```

#### web
```
├── web
|   ├── exception
|   |   └── ExControllerAdvice.java
|   |
|   ├── response
|   |   ├── ApiResponse.java
|   |   └── ApiResponseGenerator.java
|   |
|   ├── ApiResponse.java
|   ├── ExceptionResponse.java
|   ├── AuthErrorMessage.java
|   ├── AuthExceptionController.java
|   ├── UserErrorMessage.java
|   └── UserExceptionController.java
```
---

3. 기술 스택
- Java 11
- Springboot 2.6.8.RELEASE
- Spring Data JPA
- Spring Security
- Lombok
- Jwt
- Mysql
- Redis
---

4. 아키텍처

![image](https://user-images.githubusercontent.com/102213509/218680442-e3b03b76-36a7-4e3b-845e-a8b88759ef0c.png)
- 가입 및 로그인 이전을 Auth Server로 명명.
- 가입 및 로그인 이후를 User Server로 명명.
- Token 저장은 MySQL에 진행하나, Refresh Token 관리와 로그아웃 시의 blacklist 처리를 위해 Redis 사용.
---

5. 데이터베이스

![image](https://user-images.githubusercontent.com/102213509/218681125-72f140fe-275f-414c-b792-b3f918c36b09.png)
#### 테이블
- user
  - 회원가입 시 입력된 정보를 바탕으로 유저 정보가 저장된 테이블
- authority
  - 권한
- user_authority
  - 가입한 유저 고유 id 별로 권한을 부여한 정보를 저장한 테이블
- refresh_token
  - rt_key = User ID 값 삽입
  - rt_value = Refresh Token String 값 삽입
- Hibernate_sequence
  - MySQL애서 Auto increment 대신 사용

#### 도메인 간 관계
- 현재 설정
  - user - authority = N:N (1:N, N:1도 무방하나, 다른 많은 정보가 들어가지 않기 때문에 해당 관계로 표현함.)
  - authority - user_authority = authority에 정의한 Role을 user_authority에서 가져다 사용하는 
  - refresh_token = Token 자체에 유저 정보가 있기 때문에 매핑 하지 않음.

