# React 코딩 규칙

<!--
  React 전용 코딩 규칙
  이 파일은 React 프로젝트에서 Cursor AI가 따라야 할 규칙을 정의합니다
-->

## 📋 메타데이터
- 버전: 1.0.0
- 마지막 업데이트: 2024
- 목적: React 프로젝트에서 일관된 코드 품질과 개발 패턴 유지
- 참고: [토스 프론트엔드 펀더멘털 - 좋은 코드를 위한 4가지 기준](https://frontend-fundamentals.com/code-quality/code/)

<!-- ============================================ -->
<!-- 좋은 코드를 위한 4가지 기준 -->
<!-- 토스 프론트엔드 펀더멘털의 코드 품질 기준을 React에 적용 -->
<!-- ============================================ -->

## 🎯 좋은 코드를 위한 4가지 기준

좋은 React 코드는 **변경하기 쉬운** 코드입니다. 새로운 요구사항을 구현할 때 기존 코드를 수정하고 배포하기 수월한 코드가 좋은 코드입니다. 코드가 변경하기 쉬운지는 4가지 기준으로 판단할 수 있습니다.

> 참고: 이 섹션은 [토스 프론트엔드 펀더멘털](https://frontend-fundamentals.com/code-quality/code/)의 코드 품질 기준을 React에 적용한 것입니다.

### 1. 가독성 (Readability)

**가독성**은 코드가 읽기 쉬운 정도를 말합니다. 코드가 변경하기 쉬우려면 먼저 코드가 어떤 동작을 하는지 이해할 수 있어야 합니다.

읽기 좋은 React 코드는 읽는 사람이 한 번에 머릿속에서 고려하는 맥락이 적고, 위에서 아래로 자연스럽게 이어집니다.

#### 맥락 줄이기

- **같이 실행되지 않는 코드 분리하기**
  - 컴포넌트 내에서 서로 관련 없는 로직을 분리합니다
  - UI 렌더링 로직과 비즈니스 로직을 분리합니다
  - 커스텀 훅으로 로직을 추출합니다

```typescript
// 나쁜 예: 모든 로직이 한 곳에
const UserProfile = ({ userId }: { userId: string }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetch(`/api/users/${userId}`).then(res => res.json()).then(setUser);
  }, [userId]);
  
  // UI 로직과 데이터 페칭이 섞여 있음
  return <div>{user?.name}</div>;
};

// 좋은 예: 로직을 커스텀 훅으로 분리
function useUser(userId: string) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => {
        setUser(data);
        setLoading(false);
      });
  }, [userId]);
  
  return { user, loading };
}

const UserProfile = ({ userId }: { userId: string }) => {
  const { user, loading } = useUser(userId);
  return <div>{user?.name}</div>;
};
```

- **구현 상세 추상화하기**
  - 복잡한 로직은 별도 함수나 커스텀 훅으로 추상화합니다
  - 컴포넌트는 "무엇을" 하는지에 집중하고, "어떻게" 하는지는 숨깁니다

```typescript
// 나쁜 예: 구현 상세가 컴포넌트에 노출됨
const ProductList = () => {
  const [products, setProducts] = useState([]);
  const [filter, setFilter] = useState('');
  
  useEffect(() => {
    fetch('/api/products')
      .then(res => res.json())
      .then(data => {
        const filtered = data.filter(p => 
          p.name.toLowerCase().includes(filter.toLowerCase())
        );
        setProducts(filtered);
      });
  }, [filter]);
  
  // ...
};

// 좋은 예: 구현 상세를 추상화
function useFilteredProducts(filter: string) {
  const [products, setProducts] = useState([]);
  
  useEffect(() => {
    fetch('/api/products')
      .then(res => res.json())
      .then(data => {
        const filtered = data.filter(p => 
          p.name.toLowerCase().includes(filter.toLowerCase())
        );
        setProducts(filtered);
      });
  }, [filter]);
  
  return products;
}

const ProductList = () => {
  const [filter, setFilter] = useState('');
  const products = useFilteredProducts(filter);
  // ...
};
```

- **로직 종류에 따라 합쳐진 함수 쪼개기**
  - 하나의 함수나 컴포넌트가 여러 종류의 로직을 처리하지 않도록 합니다
  - 데이터 페칭, 변환, 렌더링 로직을 분리합니다

#### 이름 붙이기

- **복잡한 조건에 이름 붙이기**
  - 복잡한 조건문은 의미 있는 변수명으로 추출합니다
  - 가독성을 높이고 의도를 명확히 합니다

```typescript
// 나쁜 예: 복잡한 조건이 직접 사용됨
{user && user.age >= 18 && user.verified && !user.banned && (
  <PremiumContent />
)}

// 좋은 예: 조건에 의미 있는 이름 부여
const canAccessPremium = user 
  && user.age >= 18 
  && user.verified 
  && !user.banned;

{canAccessPremium && <PremiumContent />}
```

- **매직 넘버에 이름 붙이기**
  - 하드코딩된 숫자나 문자열은 상수로 추출합니다
  - 의미를 명확히 하고 유지보수를 쉽게 합니다

```typescript
// 나쁜 예: 매직 넘버 사용
const MAX_RETRIES = 3;
if (retryCount < 3) { ... }

// 좋은 예: 의미 있는 상수 사용
const MAX_API_RETRY_COUNT = 3;
const DEBOUNCE_DELAY_MS = 300;

if (retryCount < MAX_API_RETRY_COUNT) { ... }
```

#### 위에서 아래로 읽히게 하기

- **시점 이동 줄이기**
  - 조건부 렌더링을 단순하게 유지합니다
  - Early return 패턴을 활용합니다
  - 중첩된 조건문을 피합니다

```typescript
// 나쁜 예: 중첩된 조건부 렌더링
const Component = ({ user, isLoading }) => {
  return (
    <div>
      {isLoading ? (
        <Loading />
      ) : (
        user ? (
          <UserProfile user={user} />
        ) : (
          <LoginPrompt />
        )
      )}
    </div>
  );
};

// 좋은 예: Early return으로 단순화
const Component = ({ user, isLoading }) => {
  if (isLoading) return <Loading />;
  if (!user) return <LoginPrompt />;
  
  return <UserProfile user={user} />;
};
```

- **삼항 연산자 단순하게 하기**
  - 복잡한 삼항 연산자는 if-else로 분리합니다
  - 중첩된 삼항 연산자는 피합니다

```typescript
// 나쁜 예: 중첩된 삼항 연산자
{isLoading ? <Loading /> : user ? <UserProfile /> : <LoginPrompt />}

// 좋은 예: Early return 또는 명확한 조건부 렌더링
{isLoading && <Loading />}
{!isLoading && !user && <LoginPrompt />}
{!isLoading && user && <UserProfile user={user} />}
```

### 2. 예측 가능성 (Predictability)

**예측 가능성**이란, 함께 협업하는 동료들이 함수나 컴포넌트의 동작을 얼마나 예측할 수 있는지를 말합니다. 예측 가능성이 높은 React 코드는 일관된 규칙을 따르고, 컴포넌트의 이름과 props, 반환 값만 보고도 어떤 동작을 하는지 알 수 있습니다.

#### 이름 겹치지 않게 관리하기

- 컴포넌트 이름은 명확하고 고유하게 작성합니다
- Props 이름은 일관된 컨벤션을 따릅니다
- 변수명과 함수명이 충돌하지 않도록 주의합니다

```typescript
// 나쁜 예: 모호한 이름
const Button = ({ onClick }) => { ... };
const handleClick = () => { ... };

// 좋은 예: 명확하고 구체적인 이름
const SubmitButton = ({ onSubmit }: SubmitButtonProps) => { ... };
const handleFormSubmit = () => { ... };
```

#### 같은 종류의 함수는 반환 타입 통일하기

- 같은 목적의 함수나 훅은 일관된 반환 타입을 유지합니다
- 커스텀 훅의 반환값 구조를 통일합니다

```typescript
// 좋은 예: 일관된 반환 타입
function useFetch<T>() {
  return { data: null, loading: true, error: null };
}

function useMutation<T>() {
  return { mutate: () => {}, loading: false, error: null };
}
```

#### 숨은 로직 드러내기

- 컴포넌트의 숨은 의존성을 명시적으로 드러냅니다
- Side effect가 있는 경우 명확히 표시합니다
- Props의 기본값이나 변환 로직을 명시합니다

```typescript
// 나쁜 예: 숨은 로직
const UserCard = ({ user }) => {
  const displayName = user?.name || 'Guest'; // 기본값이 숨어있음
  return <div>{displayName}</div>;
};

// 좋은 예: 명시적인 기본값
const DEFAULT_USER_NAME = 'Guest';

const UserCard = ({ user }: UserCardProps) => {
  const displayName = user?.name ?? DEFAULT_USER_NAME;
  return <div>{displayName}</div>;
};
```

### 3. 응집도 (Cohesion)

**응집도**란, 수정되어야 할 코드가 항상 같이 수정되는지를 말합니다. 응집도가 높은 React 코드는 코드의 한 부분을 수정해도 의도치 않게 다른 부분에서 오류가 발생하지 않습니다.

> **주의**: 가독성과 응집도는 서로 상충할 수 있습니다. 함께 수정되지 않으면 오류가 발생할 수 있는 경우에는 응집도를 우선하여 코드를 공통화/추상화하세요. 위험성이 높지 않은 경우에는 가독성을 우선하여 코드 중복을 허용하세요.

#### 함께 수정되는 파일을 같은 디렉토리에 두기

- 관련된 컴포넌트, 훅, 유틸리티를 같은 디렉토리에 배치합니다
- 기능별로 폴더를 구성합니다 (도메인 기반 구조)

```
components/
  UserProfile/
    UserProfile.tsx
    UserProfile.test.tsx
    useUserProfile.ts
    types.ts
```

#### 매직 넘버 없애기

- 하드코딩된 값은 상수로 추출합니다
- 관련된 상수는 함께 관리합니다

```typescript
// 나쁜 예: 매직 넘버
const PAGINATION_SIZE = 10;
if (items.length > 10) { ... }

// 좋은 예: 의미 있는 상수
const PAGINATION_CONFIG = {
  DEFAULT_PAGE_SIZE: 10,
  MAX_PAGE_SIZE: 100,
} as const;

if (items.length > PAGINATION_CONFIG.DEFAULT_PAGE_SIZE) { ... }
```

#### 폼의 응집도 생각하기

- 폼 관련 상태와 로직을 하나의 컴포넌트나 커스텀 훅으로 묶습니다
- 폼 검증 로직을 폼 컴포넌트와 함께 배치합니다

```typescript
// 좋은 예: 폼 로직을 하나로 묶기
function useLoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [errors, setErrors] = useState({});
  
  const validate = () => {
    const newErrors = {};
    if (!email) newErrors.email = '이메일을 입력하세요';
    if (!password) newErrors.password = '비밀번호를 입력하세요';
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (validate()) {
      // 제출 로직
    }
  };
  
  return { email, setEmail, password, setPassword, errors, handleSubmit };
}
```

### 4. 결합도 (Coupling)

**결합도**란, 코드를 수정했을 때의 영향 범위를 말합니다. 코드를 수정했을 때 영향 범위가 적어서 변경에 따른 범위를 예측할 수 있는 코드가 수정하기 쉬운 코드입니다.

#### 책임을 하나씩 관리하기

- 각 컴포넌트는 하나의 책임만 가집니다
- 단일 책임 원칙(SRP)을 따릅니다

```typescript
// 나쁜 예: 여러 책임을 가진 컴포넌트
const UserDashboard = () => {
  // 데이터 페칭
  const [users, setUsers] = useState([]);
  useEffect(() => { ... }, []);
  
  // 필터링 로직
  const [filter, setFilter] = useState('');
  const filteredUsers = users.filter(...);
  
  // 정렬 로직
  const [sortBy, setSortBy] = useState('name');
  const sortedUsers = filteredUsers.sort(...);
  
  // 렌더링
  return <div>...</div>;
};

// 좋은 예: 책임 분리
function useUsers() { ... } // 데이터 페칭
function useFilteredUsers(users, filter) { ... } // 필터링
function useSortedUsers(users, sortBy) { ... } // 정렬

const UserDashboard = () => {
  const users = useUsers();
  const filteredUsers = useFilteredUsers(users, filter);
  const sortedUsers = useSortedUsers(filteredUsers, sortBy);
  
  return <UserList users={sortedUsers} />;
};
```

#### 중복 코드 허용하기

- 불필요한 결합도를 피하기 위해 중복 코드를 허용하는 경우도 고려합니다
- 작은 중복은 추상화보다 가독성을 우선할 수 있습니다

```typescript
// 가독성을 위해 작은 중복 허용
const PrimaryButton = ({ onClick }) => (
  <button className="btn-primary" onClick={onClick}>...</button>
);

const SecondaryButton = ({ onClick }) => (
  <button className="btn-secondary" onClick={onClick}>...</button>
);

// 과도한 추상화는 오히려 가독성을 해칠 수 있음
```

#### Props Drilling 지우기

- Props Drilling을 최소화합니다
- Context API나 상태 관리 라이브러리를 적절히 활용합니다
- 하지만 과도한 Context 사용은 피합니다

```typescript
// 나쁜 예: Props Drilling
const App = () => {
  const theme = 'dark';
  return <Layout theme={theme} />;
};

const Layout = ({ theme }) => {
  return <Header theme={theme} />;
};

const Header = ({ theme }) => {
  return <Button theme={theme} />;
};

// 좋은 예: Context 사용
const ThemeContext = createContext('light');

const App = () => {
  return (
    <ThemeContext.Provider value="dark">
      <Layout />
    </ThemeContext.Provider>
  );
};

const Button = () => {
  const theme = useContext(ThemeContext);
  return <button className={`btn-${theme}`}>...</button>;
};
```

### 코드 품질 여러 각도로 보기

아쉽게도 이 4가지 기준을 모두 한꺼번에 충족하기는 어려워요.

- **가독성 vs 응집도**: 함수나 변수가 항상 같이 수정되기 위해서 공통화 및 추상화하면 응집도가 높아지지만, 코드가 한 차례 추상화되기 때문에 가독성이 떨어질 수 있습니다.
- **결합도 vs 응집도**: 중복 코드를 허용하면 코드의 영향 범위를 줄여 결합도를 낮출 수 있지만, 한쪽을 수정했을 때 다른 한쪽을 실수로 수정하지 못할 수 있어 응집도가 떨어질 수 있습니다.

React 개발자는 현재 직면한 상황을 바탕으로, 깊이 있게 고민하면서, 장기적으로 코드가 수정하기 쉽게 하기 위해서 어떤 가치를 우선해야 하는지 고민해야 합니다.

<!-- ============================================ -->
<!-- 컴포넌트 설계 원칙 -->
<!-- ============================================ -->

## 🧩 컴포넌트 설계 원칙

### 컴포넌트 구조
- 함수형 컴포넌트를 사용합니다 (클래스 컴포넌트는 레거시 코드에서만 사용)
- 하나의 컴포넌트는 단일 책임 원칙을 따릅니다
- 컴포넌트는 가능한 작고 재사용 가능하게 작성합니다
- 컴포넌트 파일명은 PascalCase를 사용합니다 (예: `UserProfile.tsx`)

### 컴포넌트 구성
- 컴포넌트는 다음과 같은 순서로 구성합니다:
  1. React 및 외부 라이브러리 import
  2. 타입/인터페이스 정의
  3. 컴포넌트 함수
  4. 내부 유틸리티 함수 (필요시)
  5. export

### Props 설계
- Props 타입을 명시적으로 정의합니다 (TypeScript 사용 시)
- Props는 읽기 전용으로 취급합니다 (직접 수정 금지)
- 필수 props와 선택적 props를 명확히 구분합니다
- Props 기본값은 defaultProps보다 함수 매개변수 기본값을 사용합니다

```typescript
// 좋은 예
interface ButtonProps {
  label: string;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
}

const Button: React.FC<ButtonProps> = ({ 
  label, 
  onClick, 
  variant = 'primary',
  disabled = false 
}) => {
  // ...
};
```

### 컴포넌트 분리 기준
- 컴포넌트가 300줄을 넘어가면 분리를 고려합니다
- 동일한 로직이 반복되면 재사용 가능한 컴포넌트로 추출합니다
- UI와 로직이 명확히 분리 가능하면 커스텀 훅으로 분리합니다

<!-- ============================================ -->
<!-- Hooks 사용 가이드 -->
<!-- ============================================ -->

## 🪝 Hooks 사용 가이드

### 기본 Hooks 규칙
- Hooks는 항상 컴포넌트 최상위 레벨에서 호출합니다
- 조건문, 반복문, 중첩 함수 내에서 Hooks를 호출하지 않습니다
- 커스텀 훅의 이름은 `use`로 시작합니다

### useState
- 관련된 상태는 객체로 그룹화합니다
- 상태 업데이트가 이전 상태에 의존하는 경우 함수형 업데이트를 사용합니다
- 초기값이 비용이 큰 계산인 경우 lazy initialization을 사용합니다

```typescript
// 좋은 예
const [user, setUser] = useState<User | null>(null);
const [count, setCount] = useState(() => expensiveCalculation());

// 이전 상태에 의존하는 경우
setCount(prevCount => prevCount + 1);
```

### useEffect
- 의존성 배열을 명확히 지정합니다
- cleanup 함수가 필요한 경우 반환합니다
- 불필요한 effect는 피합니다 (조건부 실행 고려)

```typescript
// 좋은 예
useEffect(() => {
  const subscription = subscribe();
  return () => {
    subscription.unsubscribe();
  };
}, [userId]); // 의존성 명시
```

### useMemo & useCallback
- 성능 최적화가 실제로 필요한 경우에만 사용합니다
- 의존성 배열을 정확히 지정합니다
- 과도한 사용은 오히려 성능을 저하시킬 수 있으므로 주의합니다

```typescript
// 실제로 필요한 경우에만 사용
const memoizedValue = useMemo(() => expensiveCalculation(a, b), [a, b]);
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

### 커스텀 Hooks
- 로직 재사용이 필요한 경우 커스텀 훅으로 추출합니다
- 커스텀 훅은 단일 책임을 가져야 합니다
- 커스텀 훅은 `hooks/` 디렉토리에 위치시킵니다

```typescript
// 좋은 예
function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    // fetch 로직
  }, [url]);

  return { data, loading, error };
}
```

<!-- ============================================ -->
<!-- 상태 관리 패턴 -->
<!-- ============================================ -->

## 📦 상태 관리 패턴

### 로컬 상태 vs 전역 상태
- 컴포넌트 내부에서만 사용되는 상태는 useState로 관리합니다
- 여러 컴포넌트에서 공유되는 상태는 Context API 또는 상태 관리 라이브러리를 사용합니다
- 서버 상태는 React Query, SWR 등의 데이터 페칭 라이브러리를 고려합니다

### Context API
- Context는 적절한 범위로 분리합니다 (과도한 Context 사용 지양)
- Context Provider는 필요한 컴포넌트 트리만 감쌉니다
- Context 값이 자주 변경되는 경우 메모이제이션을 고려합니다

```typescript
// 좋은 예
const UserContext = createContext<UserContextType | undefined>(undefined);

export const UserProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const value = useMemo(() => ({ user, setUser }), [user]);
  
  return <UserContext.Provider value={value}>{children}</UserContext.Provider>;
};
```

### 상태 관리 라이브러리 (Redux, Zustand 등)
- 복잡한 전역 상태가 필요한 경우에만 사용합니다
- 상태 구조를 평탄하게 유지합니다 (정규화)
- 액션과 리듀서는 명확하고 예측 가능하게 작성합니다

<!-- ============================================ -->
<!-- 성능 최적화 -->
<!-- ============================================ -->

## ⚡ 성능 최적화

### 리렌더링 최적화
- React.memo를 적절히 사용하여 불필요한 리렌더링을 방지합니다
- props가 객체나 배열인 경우 참조 동일성을 고려합니다
- 자식 컴포넌트에 함수를 전달할 때 useCallback을 사용합니다

```typescript
// 좋은 예
const MemoizedComponent = React.memo(Component, (prevProps, nextProps) => {
  return prevProps.id === nextProps.id;
});
```

### 리스트 렌더링
- 리스트 아이템에는 고유한 key를 제공합니다
- key는 배열 인덱스보다 고유한 ID를 사용합니다
- 가상화(virtualization)가 필요한 경우 react-window 등을 고려합니다

```typescript
// 좋은 예
{items.map(item => (
  <ListItem key={item.id} item={item} />
))}
```

### 코드 스플리팅
- 라우트별 코드 스플리팅을 적용합니다
- 큰 컴포넌트는 React.lazy로 지연 로딩합니다
- Suspense를 사용하여 로딩 상태를 처리합니다

```typescript
// 좋은 예
const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <LazyComponent />
    </Suspense>
  );
}
```

### 이미지 최적화
- 이미지는 적절한 크기와 포맷을 사용합니다
- lazy loading을 적용합니다
- next/image (Next.js) 또는 유사한 최적화 컴포넌트를 사용합니다

<!-- ============================================ -->
<!-- 이벤트 처리 -->
<!-- ============================================ -->

## 🎯 이벤트 처리

### 이벤트 핸들러 네이밍
- 이벤트 핸들러는 `handle` 또는 `on` 접두사를 사용합니다
- 이벤트 핸들러는 화살표 함수로 정의하거나 useCallback으로 메모이제이션합니다

```typescript
// 좋은 예
const handleClick = useCallback((event: React.MouseEvent) => {
  // 처리 로직
}, [dependencies]);

<button onClick={handleClick}>Click</button>
```

### 이벤트 객체 처리
- 이벤트 객체의 타입을 명시합니다
- 필요시 이벤트의 기본 동작을 방지합니다 (preventDefault)
- 이벤트 전파를 제어합니다 (stopPropagation)

<!-- ============================================ -->
<!-- 폼 처리 -->
<!-- ============================================ -->

## 📝 폼 처리

### 제어 컴포넌트 vs 비제어 컴포넌트
- 대부분의 경우 제어 컴포넌트를 사용합니다
- 복잡한 폼은 react-hook-form, formik 등의 라이브러리를 고려합니다
- 폼 검증은 클라이언트와 서버 양쪽에서 수행합니다

### 폼 상태 관리
- 각 입력 필드는 독립적인 상태로 관리하거나 폼 라이브러리를 사용합니다
- 폼 제출은 비동기로 처리하고 로딩 상태를 표시합니다
- 에러 메시지는 사용자에게 명확하게 표시합니다

<!-- ============================================ -->
<!-- 타입 안전성 -->
<!-- ============================================ -->

## 🔒 타입 안전성 (TypeScript)

### Props 타입 정의
- 모든 컴포넌트의 props 타입을 명시적으로 정의합니다
- 공통 props는 별도 타입으로 추출하여 재사용합니다
- 제네릭을 활용하여 재사용 가능한 컴포넌트를 만듭니다

```typescript
// 좋은 예
interface BaseButtonProps {
  variant?: 'primary' | 'secondary';
  size?: 'small' | 'medium' | 'large';
}

interface ButtonProps extends BaseButtonProps {
  label: string;
  onClick: () => void;
}
```

### 이벤트 타입
- React의 이벤트 타입을 명시적으로 사용합니다
- 커스텀 이벤트의 경우 타입을 정의합니다

```typescript
// 좋은 예
const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
  // ...
};
```

<!-- ============================================ -->
<!-- 접근성 (a11y) -->
<!-- ============================================ -->

## ♿ 접근성

### 기본 원칙
- 시맨틱 HTML을 사용합니다
- 키보드 네비게이션을 지원합니다
- ARIA 속성을 적절히 사용합니다
- 색상 대비를 충분히 확보합니다

### 주요 체크리스트
- 모든 인터랙티브 요소는 키보드로 접근 가능해야 합니다
- 이미지에는 alt 텍스트를 제공합니다
- 폼에는 라벨을 연결합니다
- 포커스 관리가 적절히 이루어집니다

<!-- ============================================ -->
<!-- 에러 처리 -->
<!-- ============================================ -->

## 🚨 에러 처리

### 에러 바운더리
- 예상치 못한 에러를 처리하기 위해 Error Boundary를 구현합니다
- 에러 바운더리는 적절한 레벨에 배치합니다
- 사용자에게 친화적인 에러 메시지를 표시합니다

```typescript
// 좋은 예
class ErrorBoundary extends React.Component<Props, State> {
  // 에러 바운더리 구현
}
```

### 비동기 에러 처리
- 비동기 작업의 에러를 적절히 처리합니다
- 에러 상태를 UI에 반영합니다
- 에러 로깅을 통해 디버깅을 용이하게 합니다

<!-- ============================================ -->
<!-- 테스트 -->
<!-- ============================================ -->

## 🧪 테스트

### 테스트 원칙
- 컴포넌트는 사용자 관점에서 테스트합니다
- 테스트는 격리되어 실행 가능해야 합니다
- 테스트 이름은 무엇을 테스트하는지 명확히 표현합니다

### 테스트 도구
- React Testing Library를 사용하여 컴포넌트를 테스트합니다
- 사용자 인터랙션을 시뮬레이션합니다
- 스냅샷 테스트는 신중하게 사용합니다

<!-- ============================================ -->
<!-- AI 어시스턴트 지침 -->
<!-- ============================================ -->

## 🤖 AI 어시스턴트 지침

### React 코드 생성 시
- 함수형 컴포넌트를 기본으로 사용합니다
- TypeScript를 사용하는 경우 타입을 명시적으로 정의합니다
- Hooks 규칙을 준수합니다
- 성능 최적화는 실제로 필요한 경우에만 적용합니다

### 리팩토링 시
- 기존 동작을 변경하지 않도록 주의합니다
- 컴포넌트 분리는 작은 단계로 진행합니다
- 테스트가 있는 경우 테스트를 먼저 확인합니다

### 질문 응답 시
- React 공식 문서를 참고하여 정확한 정보를 제공합니다
- 최신 React 패턴과 베스트 프랙티스를 반영합니다
- 여러 옵션이 있는 경우 장단점을 설명합니다

