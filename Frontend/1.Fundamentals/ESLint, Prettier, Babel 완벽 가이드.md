# 프론트엔드 개발 환경의 핵심: ESLint, Prettier, Babel 이해하기

프론트엔드 프로젝트를 시작할 때 가장 먼저 마주치는 도구들이 있습니다. 바로 **ESLint, Prettier, Babel**입니다. 이 도구들이 각각 어떤 역할을 하는지, 그리고 왜 함께 사용해야 하는지 정리해 보겠습니다.

---

## 1. ESLint: "코드의 논리와 품질을 담당"

**ESLint**는 자바스크립트 코드의 문법 에러를 찾아내고, 잠재적인 버그를 유발할 수 있는 코드를 경고해주는 **린터(Linter)**입니다.

*   **주요 역할:**
    *   선언하고 사용하지 않은 변수 찾기.
    *   정의되지 않은 함수 호출 방지.
    *   특정 코딩 스타일 강제 (예: `var` 대신 `let`, `const` 권장).
*   **핵심 철학:** "코드가 올바르게 동작하는가?"에 집중합니다.

---

## 2. Prettier: "코드의 미적 일관성을 담당"

**Prettier**는 코드를 보기 좋게 정렬해주는 **포맷터(Formatter)**입니다.

*   **주요 역할:**
    *   줄 바꿈, 들여쓰기(Tab vs Space) 자동 교정.
    *   따옴표 스타일(`'` vs `"`) 통일.
    *   세미콜론(`;`) 누락 보정.
*   **핵심 철학:** "코드가 예쁘고 일관되게 보이는가?"에 집중합니다. 
*   **협업 시 이점:** 누가 작성하든 코드가 동일한 형식을 유지하므로 코드 리뷰가 수월해집니다.

---

## 3. Babel: "코드의 호환성을 담당"

**Babel**은 최신 자바스크립트 문법(ES6+)을 구형 브라우저(IE 등)에서도 돌아갈 수 있는 코드로 바꿔주는 **트랜스파일러(Transpiler)**입니다.

*   **주요 역할:**
    *   화살표 함수(`=>`)를 일반 `function`으로 변환.
    *   `class`, `import` 등 최신 문법을 하위 버전 자바스크립트로 번역.
    *   JSX(React) 문법을 일반 자바스크립트로 변환.
*   **2026년 트렌드:** 최근에는 속도가 더 빠른 **SWC**나 **esbuild**가 Babel의 역할을 대체하고 있는 추세입니다.

---

## 4. 이 도구들을 왜 함께 쓰나요? (협업 시너지)

| 구분 | ESLint | Prettier | Babel |
| :--- | :--- | :--- | :--- |
| **관심사** | 코드 에러 및 품질 (Logic) | 스타일링 및 포맷팅 (Style) | 브라우저 호환성 (Compatibility) |
| **작동 시점** | 개발 중 (IDE) / 빌드 시 | 코드 저장 시 (Save) | 빌드 시 (Build) |

### **주의: ESLint와 Prettier의 충돌 방지**
두 도구 모두 스타일 관련 설정을 가질 수 있어 서로 부딪힐 때가 있습니다. (예: ESLint는 작은따옴표를 원하는데, Prettier는 큰따옴표로 자동 수정해버리는 경우)
이를 해결하기 위해 보통 `eslint-config-prettier`를 설치하여 **스타일 교정 권한을 Prettier에게 위임**하는 설정을 추가합니다.

---

## 5. 실무 권장 설정 예시 (Standard Config)

프로젝트 루트에 아래 파일들을 생성하여 바로 사용할 수 있는 표준 설정입니다.

### **① Prettier 설정 (`.prettierrc`)**
```json
{
  "printWidth": 100,          // 한 줄 최대 길이
  "tabWidth": 2,              // 들여쓰기 칸 수
  "useTabs": false,           // 탭 대신 스페이스 사용
  "semi": true,               // 문장 끝 세미콜론 사용
  "singleQuote": true,        // 작은따옴표 사용
  "trailingComma": "all",     // 여러 줄 요소의 마지막 콤마 추가
  "bracketSpacing": true,     // 객체 리터럴 중괄호 사이 공백
  "arrowParens": "always"     // 화살표 함수 인자 괄호 필수
}
```

### **② ESLint 설정 (`eslint.config.js` - v9+ Flat Config 기준)**
최신 ESLint는 `eslint.config.js`라는 이름의 플랫 설정을 사용합니다.

```javascript
import js from "@eslint/js";
import prettier from "eslint-config-prettier";

export default [
  js.configs.recommended, // 기본 자바스크립트 추천 규칙
  prettier,               // Prettier와 충돌하는 규칙 비활성화
  {
    rules: {
      "no-unused-vars": "warn",    // 사용하지 않는 변수는 경고만
      "no-console": "off",         // 콘솔 로그 허용
      "eqeqeq": "error",           // === 사용 강제
      "curly": "error"             // 제어문 중괄호 필수
    }
  }
];
```

---

## 6. 요약

1.  **에러**를 잡고 싶다면? -> **ESLint**
2.  코드를 **깔끔하게** 줄 맞추고 싶다면? -> **Prettier**
3.  **최신 문법**을 어디서든 돌리고 싶다면? -> **Babel**

이 세 가지 도구를 조화롭게 설정하는 것이 생산성 높은 프론트엔드 개발 환경의 첫걸음입니다.
