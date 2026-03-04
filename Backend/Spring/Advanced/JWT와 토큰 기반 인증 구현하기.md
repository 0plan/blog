# JWT와 토큰 기반 인증 구현하기: 세션 없는 보안 설계

전통적인 세션 방식은 서버의 메모리를 사용하므로 서버 확장 시 관리가 어렵다는 단점이 있습니다. **JWT(JSON Web Token)**는 인증 정보를 토큰 자체에 담아 클라이언트가 보관하게 함으로써, **Stateless(상태 없음)**한 확장이 용이한 구조를 제공합니다.

---

## 1. JWT의 구조

JWT는 점(`.`)으로 구분된 세 부분으로 구성됩니다.
*   **Header:** 토큰의 타입과 사용 중인 암호화 알고리즘 (예: HS256).
*   **Payload:** 토큰에 담을 정보(Claim). 유저 ID, 권한, 만료 시간 등.
*   **Signature:** 서버만 알고 있는 비밀키로 생성한 서명. 토큰의 위변조 여부를 확인합니다.

---

## 2. JWT 인증 워크플로우

1.  **로그인:** 유저가 아이디/비번으로 로그인 요청.
2.  **토큰 생성:** 서버가 유효성을 확인하고 JWT를 생성하여 응답.
3.  **요청:** 클라이언트는 이후 모든 요청의 `Authorization` 헤더에 `Bearer <Token>`을 넣어 보냄.
4.  **검증:** 서버는 서명을 확인하고, 유효하면 요청을 처리.

---

## 3. Spring Boot에서 JWT 구현 (핵심 코드)

`jjwt` 라이브러리를 사용하여 토큰을 생성하고 검증하는 로직의 핵심입니다.

```java
public String createToken(String userPk, List<String> roles) {
    Claims claims = Jwts.claims().setSubject(userPk);
    claims.put("roles", roles);
    Date now = new Date();
    
    return Jwts.builder()
            .setClaims(claims)
            .setIssuedAt(now)
            .setExpiration(new Date(now.getTime() + tokenValidTime)) // 만료 시간 설정
            .signWith(SignatureAlgorithm.HS256, secretKey) // 암호화
            .compact();
}
```

---

## 4. JWT 사용 시 주의사항

1.  **민감 정보 금지:** 페이로드는 누구나 디코딩할 수 있으므로 비밀번호 같은 정보는 절대 넣지 마세요.
2.  **만료 시간 설정:** 토큰이 탈취될 경우를 대비해 Access Token의 수명은 짧게(예: 30분) 설정하고, Refresh Token을 병행 사용하세요.
3.  **Secret Key 보안:** 비밀키가 유출되면 전체 보안이 무너집니다. 반드시 환경 변수나 Secret Manager로 관리하세요.

---

## 5. 결론

JWT는 현대적인 마이크로서비스 아키텍처(MSA)와 모바일 앱 연동에 가장 적합한 인증 방식입니다. 서버의 부담을 줄이고 유연한 확장을 원하신다면 JWT 도입을 적극 고려해 보세요!
