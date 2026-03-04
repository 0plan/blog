# Nuxt.js 데이터 페칭 가이드: useFetch와 useAsyncData 완벽 이해

Nuxt 3에서는 서버 사이드 렌더링(SSR) 환경에 최적화된 데이터 페칭 방식이 필요합니다. 단순히 `axios`를 쓰는 대신, Nuxt 전용 Composable을 써야 하는 이유와 차이점을 정리합니다.

---

## 1. 왜 전용 API를 써야 할까?

전통적인 방식(Axios 등)을 사용하면 클라이언트와 서버에서 데이터 요청이 중복으로 발생할 수 있습니다. Nuxt의 페칭 API는 **데이터 하이드레이션(Hydration)**을 지원하여 서버에서 가져온 데이터를 클라이언트에 안전하게 전달하고 중복 호출을 막습니다.

---

## 2. useFetch vs useAsyncData

### **① useFetch (가장 권장됨)**
URL로부터 데이터를 직접 가져올 때 사용하는 가장 단순한 방법입니다.
```javascript
const { data, pending, error, refresh } = await useFetch('/api/users')
```

### **② useAsyncData**
복잡한 로직이 필요하거나 외부 라이브러리(GraphQL, SDK 등)를 호출할 때 사용합니다.
```javascript
const { data } = await useAsyncData('unique-key', () => $fetch('/api/users'))
```

---

## 3. 주요 옵션 활용법

*   **`pick`:** 응답 데이터 중 필요한 필드만 골라서 가져옵니다. (성능 최적화)
*   **`watch`:** 특정 반응형 데이터가 변할 때 자동으로 데이터를 다시 불러옵니다.
*   **`server: false`:** 특정 데이터를 클라이언트 사이드에서만 불러오고 싶을 때 사용합니다.

---

## 4. 요약

1.  대부분의 상황에서는 **`useFetch`** 하나로 충분합니다.
2.  URL 호출 이상의 복잡한 로직이 필요하면 **`useAsyncData`**를 고려하세요.
3.  성능을 위해 항상 **`pick`**을 사용하여 불필요한 데이터를 덜어내세요.
