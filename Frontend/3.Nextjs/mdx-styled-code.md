# MDX 코드 블록 스타일링: rehype-pretty-code와 remark-gfm 가이드

Next.js에서 MDX를 사용할 때, 기본 코드 블록은 밋밋한 텍스트로 표시됩니다. 이를 VS Code처럼 예쁜 하이라이팅과 기능을 갖춘 코드 블록으로 변환하는 방법을 알아봅니다.

---

## 1. 필수 플러그인 설치

코드 하이라이팅을 위한 `rehype-pretty-code`와 GitHub 스타일 문법(표, 체크리스트 등)을 지원하는 `remark-gfm`을 설치합니다.

```bash
npm install next-mdx-remote rehype-pretty-code remark-gfm shiki
```

---

## 2. Next.js 설정 (next.config.mjs)

플러그인을 등록하고 하이라이팅 테마를 설정합니다.

```javascript
import remarkGfm from 'remark-gfm';
import rehypePrettyCode from 'rehype-pretty-code';

/** @type {import('rehype-pretty-code').Options} */
const options = {
  theme: 'one-dark-pro', // 다양한 테마 선택 가능 (github-dark, dracula 등)
  keepBackground: true,
};

const nextConfig = {
  pageExtensions: ['ts', 'tsx', 'js', 'jsx', 'md', 'mdx'],
  // MDX 설정
  experimental: {
    mdxRs: true,
  },
};

// MDX 구성 함수 (버전에 따라 설정 방식이 다를 수 있음)
export default withMDX({
  extension: /\.mdx?$/,
  options: {
    remarkPlugins: [remarkGfm], // 표, 리스트 등 확장 문법 지원
    rehypePlugins: [[rehypePrettyCode, options]], // 코드 하이라이팅
  },
})(nextConfig);
```

---

## 3. 주요 기능 활용법

### **① 라인 하이라이팅**
코드 블록 상단에 `{} `를 사용하여 특정 줄을 강조할 수 있습니다.
```javascript {1,3-4}
// 1번 줄과 3, 4번 줄이 강조됩니다.
const a = 1;
const b = 2;
const c = 3;
```

### **② 파일 이름 표시**
`title="파일명"` 속성을 추가하여 코드 블록 상단에 파일명을 표시할 수 있습니다.
```typescript title="lib/utils.ts"
export const add = (a: number, b: number) => a + b;
```

---

## 4. CSS 스타일링 (Tailwind CSS 기준)

`rehype-pretty-code`가 생성하는 데이터 속성을 이용해 스타일을 입힙니다.

```css
/* 글로벌 CSS 또는 Tailwind 레이어에 추가 */
code[data-theme*=' '] {
  @apply grid min-w-full break-words rounded-none border-l-4 border-l-transparent bg-transparent py-1;
}

/* 하이라이트된 라인 스타일 */
div[data-rehype-pretty-code-fragment] span[data-highlighted-line] {
  @apply border-l-blue-500 bg-blue-500/10;
}

/* 코드 블록 제목 스타일 */
div[data-rehype-pretty-code-title] {
  @apply mt-4 rounded-t-lg bg-slate-800 px-4 py-2 text-sm font-medium text-slate-200;
}
```

---

## 5. 결론

`remark-gfm`으로 문서의 구조를 잡고, `rehype-pretty-code`로 코드 가독성을 높이면 기술 블로그의 품질이 한층 올라갑니다. 특히 **Shiki** 기반의 정적 하이라이팅은 런타임 오버헤드가 없어 성능 면에서도 우수합니다.
