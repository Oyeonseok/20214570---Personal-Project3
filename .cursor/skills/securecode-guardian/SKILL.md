---
name: securecode-guardian
description: 시큐어코딩 자동화 워크플로우. 코드를 생성하거나 라이브러리를 사용할 때 반드시 호출. CVE 검색(search_cve)과 의존성 검사(check_dependency)를 포함한 전체 보안 워크플로우를 수행. 만들어줘, 구현해줘, 작성해줘, 코딩해줘, 로그인, 게시판, 회원가입, API, 서버, 페이지, 웹, 검색, 댓글, 업로드 요청 시 적용.
---

# SecureCode Guardian 워크플로우

## 코드 생성 필수 순서

사용자가 코드를 생성/구현/작성 요청하면, 반드시 아래 순서대로 MCP 도구를 호출하세요.

### Step 1: 보안 위협 분석

```
도구: secure_develop
파라미터: { feature: "로그인" }  ← 기능명 입력
```

보안 위협, 방어 체크리스트, 필수 보안 패키지를 확인합니다.

### Step 2: CVE 검색 (라이브러리별)

```
도구: search_cve
파라미터: { query: "express" }  ← 사용할 주요 라이브러리명
```

사용할 라이브러리의 알려진 CVE를 조회합니다. 주요 라이브러리마다 1회 호출하세요.

- express, jsonwebtoken, lodash, bcrypt, axios 등 사용 예정인 패키지
- CVE가 발견되면 패치 버전을 사용하거나 대안을 적용

### Step 3: 보안 코드 템플릿 참고

```
도구: generate_secure_code
파라미터: { feature: "login", language: "typescript" }
```

보안이 적용된 코드 템플릿을 참고하여 코드를 작성합니다.

### Step 4: 코드 작성 후 시큐어코딩 적용

```
도구: secure_code
파라미터: { code: "작성한 전체 코드", show_comparison: true }
```

- 취약점 자동 탐지 + 31개 핸들러 자동 수정
- CVE 패턴 자동 검사 (import된 라이브러리 기반)
- 수정 전/후 비교 리포트

**반드시 secure_code가 반환한 수정된 코드를 사용자에게 제공하세요.**

### Step 5: 의존성 취약점 검사

```
도구: check_dependency
파라미터: { manifest: "package.json 내용", ecosystem: "npm" }
```

package.json이 있으면 의존성 전체에 대한 CVE를 검사합니다.

## 코드 리뷰/검사 요청 시

```
도구: scan_code 또는 review_code
```

기존 코드의 보안 취약점을 분석합니다.

## 취약점 질문 시

```
도구: explain_vulnerability
파라미터: { identifier: "CWE-79" }  ← CWE ID 또는 룰 ID
```

## 설정 파일 감사 시

```
도구: audit_config
파라미터: { content: "설정 파일 내용", filename: ".env" }
```

## 금지사항

- secure_code 도구 호출 없이 코드를 사용자에게 보여주는 것
- CVE 검색 단계를 생략하는 것
- 보안 체크리스트를 무시하는 것
- 원본 코드(수정 전)를 최종 결과로 보여주는 것
