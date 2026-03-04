# TypeScript 기초: 왜 자바스크립트 대신 타입을 써야 할까?

현대 프론트엔드 개발에서 **TypeScript(타입스크립트)**는 선택이 아닌 필수 기술이 되었습니다. 자바스크립트의 유연함은 유지하면서, '타입'이라는 안전장치를 더해 런타임 에러를 획기적으로 줄여주는 타입스크립트의 핵심을 정리합니다.

---

## 1. TypeScript란 무엇인가?

타입스크립트는 자바스크립트의 **상위 집합(Superset)**입니다. 즉, 모든 자바스크립트 코드는 타입스크립트 코드이기도 하지만, 타입스크립트는 자바스크립트에 없는 기능을 제공합니다.

*   **정적 타입 시스템:** 코드를 실행하기 전(컴파일 타임)에 에러를 잡아냅니다.
*   **컴파일 과정:** 브라우저는 타입스크립트를 직접 읽지 못하므로, `tsc` 명령어를 통해 일반 자바스크립트로 변환하는 과정이 필요합니다.

---

## 2. 주요 문법 기초

### **① 기본 타입 지정**
변수명 뒤에 `:`을 붙여 타입을 명시합니다.

```typescript
let name: string = "홍길동";
let age: number = 25;
let isStudent: boolean = true;
let scores: number[] = [90, 85, 100];
```

### **② 인터페이스 (Interface)**
객체의 구조를 정의할 때 사용하며, 협업 시 데이터 규격을 맞추는 데 매우 강력합니다.

```typescript
interface User {
    id: number;
    userName: string;
    email?: string; // ?는 선택적 필드 (Optional)
}

const user: User = {
    id: 1,
    userName: "Alice"
};
```

### **③ 제네릭 (Generics)**
함수나 클래스를 다양한 타입에 대해 재사용할 수 있게 해줍니다.

```typescript
function getFirstElement<T>(arr: T[]): T {
    return arr[0];
}

const firstNum = getFirstElement([1, 2, 3]); // number로 추론
const firstStr = getFirstElement(["a", "b"]); // string으로 추론
```

---

## 3. interface vs type: 무엇을 써야 할까?

비슷해 보이지만 용도와 기능에 차이가 있습니다.

### **① interface (인터페이스)**
*   **용도:** 주로 **객체의 구조**를 정의할 때 사용합니다.
*   **특징 (선언적 확장):** 동일한 이름으로 여러 번 선언하면 자동으로 합쳐집니다(Declaration Merging). 외부 라이브러리의 타입을 확장할 때 유리합니다.
*   **상속:** `extends` 키워드를 사용하여 직관적으로 상속 관계를 표현합니다.

### **② type (타입 별칭)**
*   **용도:** 객체뿐만 아니라 원시값, 연산자(`|`, `&`), 튜플 등 **모든 타입**에 이름을 붙일 수 있습니다.
*   **특징:** 한 번 정의하면 확장이 불가능합니다(새로 정의해야 함).
*   **조합:** `&` (Intersection) 연산자를 사용하여 타입을 합칩니다.

---

## 4. interface 상속(extends) 심화 이해하기

`extends`는 기존에 정의된 인터페이스의 모든 속성을 그대로 물려받으면서 새로운 속성을 추가할 때 사용합니다. 이는 코드 중복을 줄이고 논리적인 계층 구조를 만드는 핵심 도구입니다.

### **① 기본 상속 (Single Inheritance)**
가장 기본적인 형태로, 부모의 모든 속성을 자식이 그대로 가집니다.
```typescript
interface BasicUser {
  id: string;
  name: string;
}

interface PremiumUser extends BasicUser {
  membershipLevel: number;
  discountRate: number;
}
// PremiumUser는 id, name, membershipLevel, discountRate 4개의 속성을 모두 가짐
```

### **② 다중 상속 (Multiple Inheritance)**
TypeScript의 `interface`는 여러 개의 부모를 동시에 상속받을 수 있습니다. 이는 조립식(Mix-in) 스타일의 타입 설계를 가능하게 합니다.
```typescript
interface Flyable { fly(): void; }
interface Swimmable { swim(): void; }

// 여러 개의 인터페이스를 쉼표(,)로 구분하여 상속
interface SuperDuck extends Flyable, Swimmable {
  quack(): void;
}
```

### **③ 속성 재정의 (Overriding) 주의사항**
상속받은 속성의 타입을 변경할 수 있지만, 반드시 **부모 타입과 호환**되어야 합니다.
```typescript
interface Product {
  id: string | number;
  name: string;
}

// 부모가 string | number이므로, 자식은 더 구체적인 string으로 좁히는 것만 가능
interface DigitalProduct extends Product {
  id: string; // OK
}

/* 오류 발생 예시
interface FoodProduct extends Product {
  id: boolean; // Error! 'boolean'은 'string | number'에 할당할 수 없습니다.
}
*/
```

---

## 5. 실무에서의 활용 시나리오

실제 개발 시에는 공통 속성을 가진 베이스 인터페이스를 정의하고 이를 확장해 나가는 방식을 많이 사용합니다.

### **시나리오: UI 컴포넌트 데이터 모델**
웹 페이지의 여러 요소들이 공통으로 갖는 속성(id, 스타일, 클릭 이벤트)을 베이스로 정의합니다.
```typescript
interface BaseComponent {
  id: string;
  className?: string;
  onClick: () => void;
}

interface ButtonProps extends BaseComponent {
  label: string;
  color: "blue" | "red";
}

interface InputProps extends BaseComponent {
  value: string;
  placeholder: string;
}
```
이렇게 설계하면 모든 컴포넌트가 `id`와 `onClick`을 가져야 함을 강제할 수 있어 관리가 매우 쉬워집니다.

---

## 6. 실무 선택 가이드 (Best Practice)
...

1.  **객체의 구조를 정의할 때는 `interface`를 우선적으로 사용하세요.** 성능상 이점이 있고 `extends`가 더 직관적입니다.
2.  **Union(`|`)이나 Intersection(`&`) 같은 복잡한 타입 조합이 필요할 때는 `type`을 사용하세요.**
3.  **라이브러리 제작자라면 `interface`를 쓰세요.** 사용자가 동일한 이름으로 타입을 확장(Merging)하기 쉽기 때문입니다.

---

## 6. 왜 사용해야 할까? (장점)
...

1.  **에러 예방:** 함수에 잘못된 인자를 넘기거나 오타를 내면 에러 메시지가 즉시 뜹니다.
2.  **자동 완성 (DX):** IDE가 객체의 속성을 미리 알고 추천해주므로 개발 속도가 매우 빨라집니다.
3.  **가독성:** 코드가 자체적으로 문서 역할을 하여, 다른 개발자가 쓴 코드를 이해하기 쉽습니다.

---

## 4. 결론

타입스크립트를 처음 배울 때는 타입을 정의하는 과정이 번거롭게 느껴질 수 있습니다. 하지만 프로젝트 규모가 커질수록 타입스크립트가 잡아주는 버그 한두 개가 수 시간의 디버깅 시간을 아껴준다는 것을 체감하게 될 것입니다. 지금 바로 `npx tsc --init`으로 시작해 보세요!
