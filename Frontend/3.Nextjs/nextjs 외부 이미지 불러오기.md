# Next.js 외부 이미지 불러오기 및 최적화 완벽 가이드

Next.js의 `next/image`는 이미지를 자동으로 리사이징하고 최적화해주는 강력한 컴포넌트입니다. 하지만 외부 URL의 이미지를 불러오려면 보안을 위한 추가 설정이 필요합니다.

---

## 1. 왜 보안 설정이 필요한가요?

사용자가 입력한 악의적인 URL을 통해 우리 서버의 리소스를 낭비하거나, 이미지 최적화 서버를 공격하는 것을 방지하기 위함입니다. 따라서 허용된 도메인(Hostname)만 이미지를 불러올 수 있도록 화이트리스트를 관리해야 합니다.

---

## 2. remotePatterns 설정 (next.config.js)

가장 유연하고 안전한 방식인 `remotePatterns`를 사용합니다.

```javascript
// next.config.js
module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'images.unsplash.com', // 특정 호스트 허용
        port: '',
        pathname: '/**', // 모든 경로 허용
      },
      {
        protocol: 'https',
        hostname: '**.githubusercontent.com', // 서브도메인 와일드카드 허용
      },
    ],
  },
}
```

---

## 3. 실전 활용 컴포넌트 예시

외부 이미지의 크기를 미리 알 수 없는 경우 `fill` 속성을 활용하는 것이 유용합니다.

### **공통 이미지 컴포넌트 (`RemoteImage.tsx`)**

```tsx
import Image from 'next/image'

interface Props {
  src: string;
  alt: string;
}

export default function RemoteImage({ src, alt }: Props) {
  return (
    <div className="relative aspect-video w-full overflow-hidden rounded-xl">
      <Image
        src={src}
        alt={alt}
        fill // 부모 컨테이너를 가득 채움
        className="object-cover" // CSS의 background-size: cover 역할
        sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
        priority // 중요한 이미지는 우선순위 로드
      />
    </div>
  )
}
```

---

## 4. 성능 최적화 팁

1.  **`sizes` 속성 활용:** 브라우저 환경에 맞는 최적 크기의 이미지를 요청하도록 도와줍니다. 설정하지 않으면 기본적으로 모든 기기에서 큰 이미지를 다운로드할 수 있습니다.
2.  **`placeholder="blur"`:** 이미지가 로드되기 전 부드러운 블러 효과를 보여주어 사용자 경험(UX)을 개선합니다. (외부 이미지는 별도의 `blurDataURL`이 필요합니다.)
3.  **`priority`:** LCP(Largest Contentful Paint)에 해당하는 메인 배너 이미지 등에는 반드시 `priority` 속성을 추가하세요.

---

## 5. 자주 발생하는 에러 해결

*   **Error: Invalid src prop:** `next.config.js`에 도메인을 등록하지 않았을 때 발생합니다. 설정을 수정한 후에는 반드시 **서버를 재시작**해야 적용됩니다.
*   **이미지 깨짐:** 외부 서버에서 `Referer` 체크를 하거나 이미지가 존재하지 않는 경우입니다. 이럴 때는 `onError` 핸들러를 통해 기본(Fallback) 이미지를 보여주도록 구현하세요.

---

## 6. 결론

외부 이미지를 다룰 때는 보안 설정을 철저히 하고, `fill`과 `sizes`를 적절히 조합하여 성능과 디자인을 동시에 잡으세요. Next.js가 제공하는 자동 최적화 기능만 잘 활용해도 웹 성능 점수(Lighthouse)를 획기적으로 올릴 수 있습니다.
