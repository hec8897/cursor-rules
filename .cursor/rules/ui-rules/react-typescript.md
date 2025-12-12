# React TypeScript 코딩 규칙

<!--
  React TypeScript 전용 코딩 규칙
  이 파일은 React + TypeScript 프로젝트에서 Cursor AI가 따라야 할 규칙을 정의합니다
-->

## 📋 메타데이터

- 버전: 1.0.0
- 마지막 업데이트: 2024
- 목적: React TypeScript 프로젝트에서 일관된 타입 안전성과 개발 패턴 유지
- 참고:
  - [React 공식 TypeScript 가이드](https://react.dev/learn/typescript)
  - [TypeScript 핸드북](https://www.typescriptlang.org/docs/handbook/intro.html)
  - [토스 프론트엔드 펀더멘털](https://frontend-fundamentals.com/code-quality/code/)

<!-- ============================================ -->
<!-- 타입 안전성 기본 원칙 -->
<!-- ============================================ -->

## 🔒 타입 안전성 기본 원칙

### 타입 명시성

- 모든 변수, 함수, 컴포넌트의 타입을 명시적으로 정의합니다
- `any` 타입 사용을 최소화하고, 필요한 경우 `unknown`을 우선 고려합니다
- 타입 추론이 명확한 경우에만 타입 생략을 허용합니다

### 타입 정의 위치

- 컴포넌트 Props 타입은 컴포넌트 파일 상단에 정의합니다
- 공통 타입은 별도의 `types.ts` 또는 `types/` 디렉토리에 관리합니다
- 한 파일에서만 사용되는 타입은 해당 파일 내부에 정의합니다

<!-- ============================================ -->
<!-- 컴포넌트 타입 정의 -->
<!-- ============================================ -->

## 🧩 컴포넌트 타입 정의

### Props 타입 정의

#### 기본 Props 타입

```typescript
// 좋은 예: 명시적인 Props 타입 정의
interface ButtonProps {
  label: string;
  onClick: () => void;
  variant?: "primary" | "secondary";
  disabled?: boolean;
}

const Button: React.FC<ButtonProps> = ({
  label,
  onClick,
  variant = "primary",
  disabled = false,
}) => {
  return (
    <button className={`btn-${variant}`} onClick={onClick} disabled={disabled}>
      {label}
    </button>
  );
};
```

#### 함수형 컴포넌트 타입

```typescript
// 방법 1: React.FC 사용 (권장하지 않음 - children이 자동 포함됨)
const Component: React.FC<Props> = ({ prop1, prop2 }) => { ... };

// 방법 2: 직접 함수 타입 정의 (권장)
const Component = ({ prop1, prop2 }: Props): JSX.Element => { ... };

// 방법 3: React.JSX.Element 반환 타입 명시
function Component({ prop1, prop2 }: Props): React.JSX.Element {
  return <div>...</div>;
}
```

#### children Props 타입

```typescript
// 명시적으로 children 타입 정의
interface LayoutProps {
  children: React.ReactNode;
  title?: string;
}

const Layout = ({ children, title }: LayoutProps) => {
  return (
    <div>
      {title && <h1>{title}</h1>}
      {children}
    </div>
  );
};

// React.FC는 children을 자동으로 포함하므로 사용 시 주의
```

### 이벤트 핸들러 타입

```typescript
// 마우스 이벤트
const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
  event.preventDefault();
  // ...
};

// 폼 이벤트
const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
  event.preventDefault();
  // ...
};

// 입력 이벤트
const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
  const value = event.target.value;
  // ...
};

// 키보드 이벤트
const handleKeyDown = (event: React.KeyboardEvent<HTMLInputElement>) => {
  if (event.key === "Enter") {
    // ...
  }
};
```

### ref 타입

```typescript
// useRef 타입 정의
const inputRef = useRef<HTMLInputElement>(null);

const handleFocus = () => {
  inputRef.current?.focus();
};

// forwardRef와 함께 사용
interface InputProps {
  label: string;
}

const Input = forwardRef<HTMLInputElement, InputProps>(({ label }, ref) => {
  return (
    <div>
      <label>{label}</label>
      <input ref={ref} />
    </div>
  );
});
```

<!-- ============================================ -->
<!-- Hooks 타입 정의 -->
<!-- ============================================ -->

## 🪝 Hooks 타입 정의

### useState 타입

```typescript
// 명시적 타입 지정
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<User | null>(null);
const [items, setItems] = useState<Item[]>([]);

// 타입 추론이 명확한 경우 생략 가능
const [name, setName] = useState(""); // string으로 추론됨

// null 초기값 처리
const [data, setData] = useState<ApiResponse | null>(null);
```

### useEffect 타입

```typescript
// useEffect는 타입 추론이 자동으로 이루어지므로 명시적 타입 불필요
useEffect(() => {
  // 비동기 작업
  const fetchData = async () => {
    const response = await api.getData();
    setData(response);
  };

  fetchData();
}, []);
```

### useRef 타입

```typescript
// DOM 요소 참조
const inputRef = useRef<HTMLInputElement>(null);
const divRef = useRef<HTMLDivElement>(null);

// 값 저장용 (렌더링 트리거 없음)
const timerRef = useRef<number | null>(null);
const previousValueRef = useRef<string>("");

// 사용 시
if (inputRef.current) {
  inputRef.current.focus();
}
```

### useMemo & useCallback 타입

```typescript
// useMemo
const expensiveValue = useMemo<number>(() => {
  return computeExpensiveValue(a, b);
}, [a, b]);

// useCallback
const handleClick = useCallback<(event: MouseEvent) => void>((event) => {
  console.log("clicked", event);
}, []);

// 제네릭 타입 생략 가능 (타입 추론)
const memoizedValue = useMemo(() => computeValue(a, b), [a, b]);
const memoizedCallback = useCallback(() => doSomething(), []);
```

### 커스텀 Hooks 타입

```typescript
// 반환 타입 명시
interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: Error | null;
}

function useFetch<T>(url: string): UseFetchResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    // fetch 로직
  }, [url]);

  return { data, loading, error };
}

// 사용 예시
const { data, loading, error } = useFetch<User>("/api/user");
```

<!-- ============================================ -->
<!-- 제네릭 사용 -->
<!-- ============================================ -->

## 🔄 제네릭 사용

### 제네릭 컴포넌트

```typescript
// 제네릭을 사용한 재사용 가능한 컴포넌트
interface ListProps<T> {
  items: T[];
  renderItem: (item: T) => React.ReactNode;
  keyExtractor: (item: T) => string | number;
}

function List<T>({ items, renderItem, keyExtractor }: ListProps<T>) {
  return (
    <ul>
      {items.map((item) => (
        <li key={keyExtractor(item)}>{renderItem(item)}</li>
      ))}
    </ul>
  );
}

// 사용 예시
interface User {
  id: number;
  name: string;
}

<List<User>
  items={users}
  renderItem={(user) => <span>{user.name}</span>}
  keyExtractor={(user) => user.id}
/>;
```

### 제네릭 Hooks

```typescript
// 제네릭을 사용한 커스텀 훅
function useLocalStorage<T>(
  key: string,
  initialValue: T
): [T, (value: T) => void] {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });

  const setValue = (value: T) => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(error);
    }
  };

  return [storedValue, setValue];
}

// 사용 예시
const [user, setUser] = useLocalStorage<User | null>("user", null);
```

<!-- ============================================ -->
<!-- 타입 유틸리티 활용 -->
<!-- ============================================ -->

## 🛠️ 타입 유틸리티 활용

### 기본 유틸리티 타입

```typescript
// Partial: 모든 속성을 선택적으로
interface User {
  id: number;
  name: string;
  email: string;
}

type PartialUser = Partial<User>;
// { id?: number; name?: string; email?: string; }

// Required: 모든 속성을 필수로
type RequiredUser = Required<PartialUser>;

// Pick: 특정 속성만 선택
type UserBasic = Pick<User, "id" | "name">;
// { id: number; name: string; }

// Omit: 특정 속성 제외
type UserWithoutEmail = Omit<User, "email">;
// { id: number; name: string; }

// Record: 키-값 타입 정의
type UserRoles = Record<string, "admin" | "user" | "guest">;

// Readonly: 읽기 전용
type ReadonlyUser = Readonly<User>;
```

### 조건부 타입 활용

**조건부 타입(Conditional Types)**이란 타입의 조건에 따라 다른 타입을 반환하는 TypeScript의 고급 기능입니다. `T extends U ? X : Y` 형식으로 작성하며, "T가 U를 확장하면 X, 아니면 Y"를 의미합니다.

#### 기본 개념

```typescript
// 기본 문법: T extends U ? X : Y
// "T가 U의 서브타입이면 X, 아니면 Y"

type IsString<T> = T extends string ? true : false;

type A = IsString<string>; // true
type B = IsString<number>; // false
```

#### 실제 활용 예시

```typescript
// 1. null/undefined 제거
type NonNullable<T> = T extends null | undefined ? never : T;

type Example1 = NonNullable<string | null>; // string
type Example2 = NonNullable<number | undefined>; // number

// 2. 함수 반환 타입 추출
type ReturnType<T extends (...args: any) => any> = T extends (
  ...args: any
) => infer R
  ? R
  : any;

function getUser() {
  return { id: 1, name: "John" };
}
type UserReturn = ReturnType<typeof getUser>;
// { id: number; name: string }

// 3. 함수 매개변수 타입 추출
type Parameters<T extends (...args: any) => any> = T extends (
  ...args: infer P
) => any
  ? P
  : never;

function greet(name: string, age: number) {}
type GreetParams = Parameters<typeof greet>;
// [string, number]

// 4. 배열 요소 타입 추출
type ArrayElement<T> = T extends (infer U)[] ? U : never;

type Numbers = ArrayElement<number[]>; // number
type Mixed = ArrayElement<(string | number)[]>; // string | number

// 5. Promise 타입 추출
type Awaited<T> = T extends Promise<infer U> ? U : T;

type PromiseString = Awaited<Promise<string>>; // string
type NotPromise = Awaited<string>; // string

// 6. React 컴포넌트 Props 타입 추출
type ComponentProps<T> = T extends React.ComponentType<infer P> ? P : never;

const Button: React.FC<{ label: string }> = () => null;
type ButtonProps = ComponentProps<typeof Button>; // { label: string }
```

#### infer 키워드

`infer`는 조건부 타입에서 타입을 추론할 때 사용하는 키워드입니다. "이 위치의 타입을 추론해서 U라는 이름으로 사용하겠다"는 의미입니다.

```typescript
// infer를 사용하여 타입 추론
type ExtractReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

function getValue() {
  return 42;
}
type Value = ExtractReturnType<typeof getValue>; // number
```

#### 중첩 조건부 타입

조건부 타입을 중첩하여 더 복잡한 타입을 만들 수 있습니다.

```typescript
// 중첩 조건부 타입
type Flatten<T> = T extends (infer U)[]
  ? U extends any[]
    ? Flatten<U>
    : U
  : T;

type Nested = Flatten<number[][]>; // number
type Simple = Flatten<number>; // number
```

### 타입 가드

**타입 가드(Type Guard)**는 런타임에 값의 타입을 확인하여 TypeScript 컴파일러에게 타입 정보를 제공하는 함수입니다. `value is Type` 형식의 반환 타입을 사용하여 타입을 좁혀줍니다.

#### 기본 개념

타입 가드는 조건문과 함께 사용하여 TypeScript가 타입을 좁혀주도록 도와줍니다. 이를 통해 `unknown`이나 유니온 타입을 안전하게 처리할 수 있습니다.

```typescript
// 타입 가드 함수의 특징
// 1. 반환 타입이 "value is Type" 형식
// 2. boolean 값을 반환
// 3. true를 반환하면 해당 블록에서 타입이 좁혀짐

function isUser(value: unknown): value is User {
  return (
    typeof value === "object" &&
    value !== null &&
    "id" in value &&
    "name" in value &&
    "email" in value
  );
}

// 사용 예시
const data: unknown = fetchUserData();
if (isUser(data)) {
  // 이 블록에서 data는 User 타입으로 추론됨
  // TypeScript가 자동으로 타입을 좁혀줌
  console.log(data.name); // ✅ 타입 안전
  console.log(data.email); // ✅ 타입 안전
} else {
  // 이 블록에서는 data가 User가 아님
  // console.log(data.name);  // ❌ 에러
}
```

#### 다양한 타입 가드 패턴

```typescript
// 1. 기본 타입 체크
function isString(value: unknown): value is string {
  return typeof value === "string";
}

function isNumber(value: unknown): value is number {
  return typeof value === "number";
}

// 2. 객체 타입 체크
interface User {
  id: number;
  name: string;
  email: string;
}

function isUser(value: unknown): value is User {
  return (
    typeof value === "object" &&
    value !== null &&
    "id" in value &&
    "name" in value &&
    "email" in value &&
    typeof (value as any).id === "number" &&
    typeof (value as any).name === "string" &&
    typeof (value as any).email === "string"
  );
}

// 3. 배열 타입 체크
function isStringArray(value: unknown): value is string[] {
  return (
    Array.isArray(value) && value.every((item) => typeof item === "string")
  );
}

// 4. Discriminated Union 타입 가드
interface Dog {
  type: "dog";
  bark: () => void;
}

interface Cat {
  type: "cat";
  meow: () => void;
}

type Animal = Dog | Cat;

function isDog(animal: Animal): animal is Dog {
  return animal.type === "dog";
}

function makeSound(animal: Animal) {
  if (isDog(animal)) {
    animal.bark(); // ✅ animal은 Dog 타입
  } else {
    animal.meow(); // ✅ animal은 Cat 타입
  }
}

// 5. 클래스 인스턴스 체크
class ApiError extends Error {
  statusCode: number;
  constructor(message: string, statusCode: number) {
    super(message);
    this.statusCode = statusCode;
  }
}

function isApiError(error: unknown): error is ApiError {
  return error instanceof ApiError;
}

// 6. null 체크
function isNotNull<T>(value: T | null): value is T {
  return value !== null;
}

const values: (string | null)[] = ["hello", null, "world"];
const nonNullValues = values.filter(isNotNull);
// string[] 타입으로 추론됨
```

#### React에서의 활용

```typescript
// API 응답 처리
async function fetchUserData(): Promise<unknown> {
  const response = await fetch("/api/user");
  return response.json();
}

function isUserResponse(data: unknown): data is User {
  return (
    typeof data === "object" &&
    data !== null &&
    "id" in data &&
    "name" in data &&
    "email" in data
  );
}

const UserComponent = () => {
  const [user, setUser] = useState<User | null>(null);

  useEffect(() => {
    fetchUserData().then((data) => {
      if (isUserResponse(data)) {
        // data는 User 타입으로 좁혀짐
        setUser(data);
      } else {
        console.error("Invalid user data");
      }
    });
  }, []);

  if (!user) return <div>Loading...</div>;

  // user는 User 타입으로 안전하게 사용 가능
  return <div>{user.name}</div>;
};

// 이벤트 타입 가드
function isKeyboardEvent(event: Event): event is KeyboardEvent {
  return "key" in event;
}

const handleEvent = (event: Event) => {
  if (isKeyboardEvent(event)) {
    // event는 KeyboardEvent 타입
    console.log(event.key);
  }
};
```

#### 타입 가드의 장점

1. **타입 안전성**: 런타임 체크와 컴파일 타임 타입 체크를 동시에 수행
2. **코드 가독성**: 타입 체크 로직을 재사용 가능한 함수로 분리
3. **타입 좁히기**: TypeScript가 자동으로 타입을 좁혀주어 안전한 코드 작성 가능
4. **에러 방지**: 타입 관련 런타임 에러를 사전에 방지

<!-- ============================================ -->
<!-- 타입 정의 패턴 -->
<!-- ============================================ -->

## 📐 타입 정의 패턴

### 인터페이스 vs 타입 별칭

```typescript
// 인터페이스: 객체 형태의 타입 정의에 적합 (확장 가능)
interface User {
  id: number;
  name: string;
}

interface AdminUser extends User {
  role: "admin";
  permissions: string[];
}

// 타입 별칭: 유니온, 인터섹션, 제네릭 등에 적합
type Status = "pending" | "approved" | "rejected";
type ID = string | number;
type UserWithStatus = User & { status: Status };

// 일반적인 가이드라인
// - 객체 타입: interface 사용
// - 유니온/인터섹션: type 사용
// - 확장 가능성: interface 사용
```

### 타입 확장

```typescript
// 인터페이스 확장
interface BaseComponentProps {
  className?: string;
  id?: string;
}

interface ButtonProps extends BaseComponentProps {
  label: string;
  onClick: () => void;
}

// 타입 인터섹션
type ButtonProps = BaseComponentProps & {
  label: string;
  onClick: () => void;
};
```

### 인덱스 시그니처

**인덱스 시그니처(Index Signature)**는 객체의 속성 이름을 동적으로 정의할 수 있게 해주는 TypeScript 기능입니다. 객체에 어떤 키가 올지 미리 알 수 없을 때 사용합니다.

#### 기본 개념

인덱스 시그니처는 `[key: KeyType]: ValueType` 형식으로 작성하며, 객체가 특정 타입의 키와 값을 가질 수 있음을 명시합니다.

```typescript
// 기본 인덱스 시그니처
interface DynamicObject {
  [key: string]: string | number;
}

// 사용 예시
const obj: DynamicObject = {
  name: "John", // ✅ string
  age: 30, // ✅ number
  city: "Seoul", // ✅ string
  // isActive: true,  // ❌ boolean은 허용되지 않음
};

// 동적으로 속성 추가 가능
obj["newKey"] = "value"; // ✅
obj.anotherKey = 42; // ✅
```

#### 인덱스 시그니처의 특징

```typescript
// 1. 모든 속성이 인덱스 시그니처의 타입을 따라야 함
interface Config {
  [key: string]: string | number;
  name: string; // ✅ string | number에 포함됨
  age: number; // ✅ string | number에 포함됨
  // isActive: boolean;  // ❌ 에러: boolean은 허용되지 않음
}

// 2. 숫자 인덱스 시그니처
interface NumberIndex {
  [index: number]: string;
}

const arr: NumberIndex = {
  0: "first",
  1: "second",
  2: "third",
};

// 3. 문자열과 숫자 인덱스 시그니처 동시 사용
interface MixedIndex {
  [key: string]: string | number;
  [index: number]: string; // 숫자 인덱스는 문자열 인덱스의 서브타입이어야 함
}
```

#### 제한된 키 타입 (Mapped Types)

`[K in KeyType]` 문법을 사용하여 특정 키 집합에 대해서만 타입을 정의할 수 있습니다. 이를 **Mapped Type**이라고 합니다.

```typescript
// 제한된 키 타입 (Mapped Type)
type Theme = "light" | "dark";
type ThemeColors = {
  [K in Theme]: string;
};
// 결과: { light: string; dark: string; }

const colors: ThemeColors = {
  light: "#ffffff",
  dark: "#000000",
  // blue: '#0000ff',  // ❌ 에러: Theme에 'blue'가 없음
};

// 여러 키 타입 조합
type Status = "pending" | "approved" | "rejected";
type StatusMessages = {
  [K in Status]: string;
};
// { pending: string; approved: string; rejected: string; }

// 옵셔널 속성으로 만들기
type OptionalThemeColors = {
  [K in Theme]?: string;
};
// { light?: string; dark?: string; }

// readonly로 만들기
type ReadonlyThemeColors = {
  readonly [K in Theme]: string;
};
// { readonly light: string; readonly dark: string; }
```

#### 실전 활용 예시

```typescript
// 1. API 응답 타입 (동적 속성)
interface ApiResponse {
  status: number;
  message: string;
  [key: string]: any; // 추가 속성 허용
}

const response: ApiResponse = {
  status: 200,
  message: "Success",
  data: { id: 1, name: "John" }, // ✅ 동적 속성
  timestamp: Date.now(), // ✅ 동적 속성
};

// 2. 설정 객체 타입
interface AppConfig {
  apiUrl: string;
  timeout: number;
  [feature: string]: string | number | boolean; // 기능별 설정
}

const config: AppConfig = {
  apiUrl: "https://api.example.com",
  timeout: 5000,
  enableCache: true, // ✅ 동적 속성
  maxRetries: 3, // ✅ 동적 속성
};

// 3. 테마 색상 정의
type ColorName = "primary" | "secondary" | "success" | "error";
type ColorPalette = {
  [K in ColorName]: {
    light: string;
    main: string;
    dark: string;
  };
};

const palette: ColorPalette = {
  primary: { light: "#e3f2fd", main: "#2196f3", dark: "#1976d2" },
  secondary: { light: "#f3e5f5", main: "#9c27b0", dark: "#7b1fa2" },
  success: { light: "#e8f5e9", main: "#4caf50", dark: "#388e3c" },
  error: { light: "#ffebee", main: "#f44336", dark: "#d32f2f" },
};

// 4. 컴포넌트 Props 확장
interface BaseProps {
  className?: string;
  id?: string;
}

interface DynamicProps extends BaseProps {
  [key: `data-${string}`]: string; // Template Literal Types
}

const Component = (props: DynamicProps) => {
  // data-* 속성들을 동적으로 사용 가능
  return <div {...props} />;
};

// 사용
<Component className="my-class" data-testid="button" data-cy="submit-btn" />;
```

#### 인덱스 시그니처 주의사항

```typescript
// 1. 모든 속성이 인덱스 시그니처를 따라야 함
interface Problematic {
  name: string;
  [key: string]: number; // ❌ 에러: name은 string인데 인덱스 시그니처는 number
}

// 해결 방법: 유니온 타입 사용
interface Fixed {
  [key: string]: string | number;
  name: string; // ✅ string | number에 포함됨
}

// 2. 타입 안전성 제한
interface Loose {
  [key: string]: any; // ⚠️ any 사용은 타입 안전성을 해침
}

// 더 안전한 방법
interface Safer {
  [key: string]: string | number | boolean; // ✅ 구체적인 타입 지정
}

// 3. Record 유틸리티 타입과의 비교
// 인덱스 시그니처
interface IndexSignature {
  [key: string]: number;
}

// Record 타입 (더 명확함)
type RecordType = Record<string, number>;

// 둘 다 동일하지만 Record가 더 명확하고 간결함
```

<!-- ============================================ -->
<!-- 타입 안전성 패턴 -->
<!-- ============================================ -->

## 🛡️ 타입 안전성 패턴

### null/undefined 처리

```typescript
// 옵셔널 체이닝
const userName = user?.profile?.name ?? "Unknown";

// nullish coalescing
const count = value ?? 0; // null 또는 undefined일 때만 기본값

// 타입 가드
if (user !== null && user !== undefined) {
  // 이 블록에서 user는 non-null
  console.log(user.name);
}
```

### 타입 단언 (Type Assertion)

```typescript
// 타입 단언은 최후의 수단으로만 사용
// 방법 1: as 키워드
const element = document.getElementById("root") as HTMLDivElement;

// 방법 2: <> 문법 (JSX와 충돌 가능)
const element = <HTMLDivElement>document.getElementById("root");

// 타입 가드를 사용하는 것이 더 안전
function isHTMLElement(element: Element | null): element is HTMLElement {
  return element !== null;
}

if (isHTMLElement(element)) {
  // 타입 안전하게 사용
}
```

### any vs unknown

```typescript
// any: 타입 체크를 완전히 우회 (사용 지양)
function processData(data: any) {
  return data.someProperty; // 타입 체크 없음
}

// unknown: 타입 체크 필요 (권장)
function processData(data: unknown) {
  if (typeof data === "object" && data !== null && "someProperty" in data) {
    return (data as { someProperty: string }).someProperty;
  }
  throw new Error("Invalid data");
}
```

<!-- ============================================ -->
<!-- Context API 타입 정의 -->
<!-- ============================================ -->

## 📦 Context API 타입 정의

### Context 타입 정의

```typescript
// Context 값 타입 정의
interface ThemeContextType {
  theme: "light" | "dark";
  toggleTheme: () => void;
}

// Context 생성
const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

// Provider 컴포넌트
export const ThemeProvider: React.FC<{ children: React.ReactNode }> = ({
  children,
}) => {
  const [theme, setTheme] = useState<"light" | "dark">("light");

  const toggleTheme = useCallback(() => {
    setTheme((prev) => (prev === "light" ? "dark" : "light"));
  }, []);

  const value = useMemo<ThemeContextType>(
    () => ({ theme, toggleTheme }),
    [theme, toggleTheme]
  );

  return (
    <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>
  );
};

// 커스텀 Hook으로 Context 사용
export const useTheme = (): ThemeContextType => {
  const context = useContext(ThemeContext);
  if (context === undefined) {
    throw new Error("useTheme must be used within ThemeProvider");
  }
  return context;
};
```

<!-- ============================================ -->
<!-- 이벤트 처리 타입 -->
<!-- ============================================ -->

## 🎯 이벤트 처리 타입

### 이벤트 핸들러 타입 정의

```typescript
// Props에 이벤트 핸들러 타입 정의
interface FormProps {
  onSubmit: (data: FormData) => void;
  onCancel: () => void;
  onChange?: (field: string, value: string) => void;
}

// 이벤트 핸들러 함수 타입
type ButtonClickHandler = (event: React.MouseEvent<HTMLButtonElement>) => void;
type InputChangeHandler = (event: React.ChangeEvent<HTMLInputElement>) => void;
type FormSubmitHandler = (event: React.FormEvent<HTMLFormElement>) => void;
```

### 커스텀 이벤트 타입

```typescript
// 커스텀 이벤트 타입 정의
interface CustomEventDetail {
  action: "add" | "remove" | "update";
  itemId: string;
}

// 이벤트 핸들러
const handleCustomEvent = (event: CustomEvent<CustomEventDetail>) => {
  const { action, itemId } = event.detail;
  // ...
};
```

<!-- ============================================ -->
<!-- 폼 처리 타입 -->
<!-- ============================================ -->

## 📝 폼 처리 타입

### 폼 데이터 타입

```typescript
// 폼 데이터 타입 정의
interface LoginFormData {
  email: string;
  password: string;
  rememberMe?: boolean;
}

// 폼 컴포넌트
const LoginForm = () => {
  const [formData, setFormData] = useState<LoginFormData>({
    email: "",
    password: "",
    rememberMe: false,
  });

  const handleChange =
    (field: keyof LoginFormData) =>
    (event: React.ChangeEvent<HTMLInputElement>) => {
      const value =
        field === "rememberMe" ? event.target.checked : event.target.value;

      setFormData((prev) => ({
        ...prev,
        [field]: value,
      }));
    };

  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    // 제출 로직
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={formData.email}
        onChange={handleChange("email")}
      />
      <input
        type="password"
        value={formData.password}
        onChange={handleChange("password")}
      />
      <input
        type="checkbox"
        checked={formData.rememberMe}
        onChange={handleChange("rememberMe")}
      />
      <button type="submit">로그인</button>
    </form>
  );
};
```

### react-hook-form 타입

```typescript
import { useForm } from "react-hook-form";

interface FormInputs {
  email: string;
  password: string;
  age: number;
}

const Form = () => {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<FormInputs>();

  const onSubmit = (data: FormInputs) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("email", { required: true })} />
      {errors.email && <span>이메일을 입력하세요</span>}

      <input {...register("password", { required: true })} />
      {errors.password && <span>비밀번호를 입력하세요</span>}

      <input type="number" {...register("age", { valueAsNumber: true })} />
      {errors.age && <span>나이를 입력하세요</span>}

      <button type="submit">제출</button>
    </form>
  );
};
```

<!-- ============================================ -->
<!-- API 및 비동기 처리 타입 -->
<!-- ============================================ -->

## 🌐 API 및 비동기 처리 타입

### API 응답 타입

```typescript
// API 응답 타입 정의
interface ApiResponse<T> {
  data: T;
  status: number;
  message?: string;
}

interface User {
  id: number;
  name: string;
  email: string;
}

// API 함수 타입
async function fetchUser(id: number): Promise<ApiResponse<User>> {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}

// 사용 예시
const { data: user } = await fetchUser(1);
```

### 에러 처리 타입

```typescript
// 에러 타입 정의
interface ApiError {
  message: string;
  code: string;
  statusCode: number;
}

// Result 타입 패턴
type Result<T, E = Error> =
  | { success: true; data: T }
  | { success: false; error: E };

async function fetchUserSafe(id: number): Promise<Result<User, ApiError>> {
  try {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) {
      return {
        success: false,
        error: {
          message: "Failed to fetch user",
          code: "FETCH_ERROR",
          statusCode: response.status,
        },
      };
    }
    const data = await response.json();
    return { success: true, data };
  } catch (error) {
    return {
      success: false,
      error: {
        message: error instanceof Error ? error.message : "Unknown error",
        code: "UNKNOWN_ERROR",
        statusCode: 500,
      },
    };
  }
}
```

<!-- ============================================ -->
<!-- 타입 가드 및 타입 좁히기 -->
<!-- ============================================ -->

## 🔍 타입 가드 및 타입 좁히기

### 타입 가드 함수

```typescript
// 타입 가드 함수 정의
function isString(value: unknown): value is string {
  return typeof value === "string";
}

function isUser(value: unknown): value is User {
  return (
    typeof value === "object" &&
    value !== null &&
    "id" in value &&
    "name" in value &&
    "email" in value
  );
}

// 사용 예시
function processValue(value: unknown) {
  if (isString(value)) {
    // value는 string 타입으로 좁혀짐
    console.log(value.toUpperCase());
  }

  if (isUser(value)) {
    // value는 User 타입으로 좁혀짐
    console.log(value.name);
  }
}
```

### 타입 좁히기 패턴

```typescript
// typeof를 사용한 타입 좁히기
function processValue(value: string | number) {
  if (typeof value === "string") {
    // value는 string
    return value.toUpperCase();
  } else {
    // value는 number
    return value.toFixed(2);
  }
}

// in 연산자를 사용한 타입 좁히기
interface Dog {
  type: "dog";
  bark: () => void;
}

interface Cat {
  type: "cat";
  meow: () => void;
}

function makeSound(animal: Dog | Cat) {
  if (animal.type === "dog") {
    animal.bark();
  } else {
    animal.meow();
  }
}

// discriminated union 패턴
type Animal = Dog | Cat;
```

<!-- ============================================ -->
<!-- 성능 최적화 타입 -->
<!-- ============================================ -->

## ⚡ 성능 최적화 타입

### 메모이제이션 타입

```typescript
// useMemo 타입
const expensiveValue = useMemo<number>(
  () => computeExpensiveValue(a, b),
  [a, b]
);

// useCallback 타입
const handleClick = useCallback<(event: MouseEvent) => void>((event) => {
  console.log("clicked");
}, []);

// React.memo 타입
interface MemoizedComponentProps {
  value: string;
}

const MemoizedComponent = React.memo<MemoizedComponentProps>(
  ({ value }) => {
    return <div>{value}</div>;
  },
  (prevProps, nextProps) => {
    return prevProps.value === nextProps.value;
  }
);
```

### 지연 로딩 타입

```typescript
// React.lazy 타입
const LazyComponent = React.lazy<React.ComponentType<ComponentProps>>(
  () => import("./Component")
);

// Suspense와 함께 사용
<Suspense fallback={<Loading />}>
  <LazyComponent prop1="value" />
</Suspense>;
```

<!-- ============================================ -->
<!-- 테스트 타입 -->
<!-- ============================================ -->

## 🧪 테스트 타입

### 테스트 유틸리티 타입

```typescript
// Partial을 사용한 테스트 데이터 생성
function createMockUser(overrides?: Partial<User>): User {
  return {
    id: 1,
    name: "Test User",
    email: "test@example.com",
    ...overrides,
  };
}

// 테스트에서 사용
const mockUser = createMockUser({ name: "Custom Name" });
```

### 테스트 헬퍼 타입

```typescript
// 테스트용 타입 유틸리티
type TestProps<T> = Partial<T> & {
  [K in keyof T]?: T[K] extends Function ? T[K] : T[K] | jest.Mock;
};

// 사용 예시
const testProps: TestProps<ButtonProps> = {
  label: "Test",
  onClick: jest.fn(),
};
```

<!-- ============================================ -->
<!-- 타입 에러 처리 -->
<!-- ============================================ -->

## 🚨 타입 에러 처리

### 타입 에러 해결 패턴

```typescript
// 1. 타입 단언 (최후의 수단)
const element = document.getElementById("root") as HTMLDivElement;

// 2. 타입 가드 사용 (권장)
function isHTMLElement(el: Element | null): el is HTMLElement {
  return el !== null;
}

// 3. 옵셔널 체이닝
const value = obj?.nested?.property;

// 4. 기본값 제공
const count = value ?? 0;

// 5. 타입 좁히기
if (typeof value === "string") {
  // value는 string 타입
}
```

### 타입 에러 방지

```typescript
// 타입 안전한 코드 작성
// 나쁜 예: any 사용
function processData(data: any) {
  return data.property;
}

// 좋은 예: 타입 정의
interface Data {
  property: string;
}

function processData(data: Data) {
  return data.property;
}

// 제네릭 사용
function processData<T extends { property: string }>(data: T) {
  return data.property;
}
```

<!-- ============================================ -->
<!-- 타입 정의 모범 사례 -->
<!-- ============================================ -->

## 📚 타입 정의 모범 사례

### 파일 구조

```
components/
  Button/
    Button.tsx          # 컴포넌트
    Button.test.tsx     # 테스트
    Button.types.ts     # 타입 정의 (선택적)
    index.ts            # export
```

### 타입 파일 예시

```typescript
// Button.types.ts
export interface ButtonProps {
  label: string;
  onClick: () => void;
  variant?: "primary" | "secondary";
  disabled?: boolean;
}

export type ButtonVariant = ButtonProps["variant"];
```

### 타입 재사용

```typescript
// types/common.ts
export interface BaseEntity {
  id: number;
  createdAt: Date;
  updatedAt: Date;
}

// types/user.ts
import { BaseEntity } from "./common";

export interface User extends BaseEntity {
  name: string;
  email: string;
}
```

<!-- ============================================ -->
<!-- AI 어시스턴트 지침 -->
<!-- ============================================ -->

## 🤖 AI 어시스턴트 지침

### React TypeScript 코드 생성 시

- 모든 컴포넌트 Props에 타입을 명시적으로 정의합니다
- `any` 타입 사용을 피하고, `unknown`을 우선 고려합니다
- 이벤트 핸들러의 정확한 타입을 지정합니다 (React.MouseEvent, React.ChangeEvent 등)
- 제네릭을 활용하여 재사용 가능한 컴포넌트와 훅을 만듭니다
- 타입 가드를 사용하여 타입 안전성을 보장합니다

### 타입 정의 시

- 인터페이스는 객체 타입에, 타입 별칭은 유니온/인터섹션에 사용합니다
- 공통 타입은 별도 파일로 분리하여 재사용합니다
- 타입 이름은 명확하고 의미있게 작성합니다
- 제네릭 타입 매개변수는 대문자로 시작합니다 (예: `T`, `K`, `V`)

### 리팩토링 시

- 기존 타입 정의를 확인하고 일관성을 유지합니다
- 타입 에러를 해결할 때 타입 단언보다 타입 가드를 우선 사용합니다
- 불필요한 타입 단언을 제거합니다
- 타입 안전성을 해치지 않도록 주의합니다

### 질문 응답 시

- TypeScript 공식 문서를 참고하여 정확한 타입 정보를 제공합니다
- 타입 에러 해결 방법을 단계별로 설명합니다
- 여러 타입 정의 옵션이 있는 경우 장단점을 설명합니다
- 타입 안전성과 개발 생산성의 균형을 고려합니다

<!-- ============================================ -->
<!-- 참고 자료 -->
<!-- ============================================ -->

## 📚 참고 자료

- [React TypeScript 공식 가이드](https://react.dev/learn/typescript)
- [TypeScript 핸드북](https://www.typescriptlang.org/docs/handbook/intro.html)
- [TypeScript 유틸리티 타입](https://www.typescriptlang.org/docs/handbook/utility-types.html)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
