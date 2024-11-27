# 외부 이미지 불러오기


```tsx
import Image from 'next/image'

export default function RemoteImage({ src, alt }) {
  return (
    <Image
      src={src}
      alt={alt}
      width={500}
      height={500}
    />
  )
}
```

[configuration-options](https://nextjs.org/docs/pages/api-reference/components/image#configuration-options)

```js
module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'example.com',
        port: '',
        pathname: '/account123/**',
      },
    ],
  },
}
```
