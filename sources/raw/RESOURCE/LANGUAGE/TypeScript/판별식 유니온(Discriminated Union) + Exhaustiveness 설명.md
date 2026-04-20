[1 tool called]

[1 tool called]

## 판별식 유니온(Discriminated Union)이란?

**공통된 리터럴 타입 필드**를 가진 유니온 타입입니다. 이 공통 필드를 **판별자(discriminant)**라고 합니다.

## 핵심 개념

### 1. **판별자(Discriminant)**
```typescript
type LoadingState = {
  status: 'loading';  // ← 판별자: 리터럴 타입
  progress: number;
};

type SuccessState = {
  status: 'success';  // ← 판별자: 리터럴 타입
  data: string;
};
```

### 2. **타입 좁히기(Type Narrowing)**
```typescript
function handleState(state: AppState): string {
  switch (state.status) {
    case 'loading':
      // 여기서 state는 LoadingState로 좁혀짐
      return `Loading... ${state.progress}%`;
    case 'success':
      // 여기서 state는 SuccessState로 좁혀짐
      return `Success: ${state.data}`;
    case 'error':
      // 여기서 state는 ErrorState로 좁혀짐
      return `Error: ${state.message}`;
  }
}
```

## Exhaustiveness(완전성) 체크

**모든 가능한 케이스를 처리했는지 컴파일 타임에 확인**하는 기능입니다.

### 1. **never 타입 활용**
```typescript
function assertNever(x: never): never {
  throw new Error("Unexpected object: " + x);
}
```

### 2. **완전성 체크 동작**
```typescript
function handleState(state: AppState): string {
  switch (state.status) {
    case 'loading':
      return `Loading... ${state.progress}%`;
    case 'success':
      return `Success: ${state.data}`;
    // case 'error': // 이 케이스를 주석 처리하면
      // return `Error: ${state.message}`;
    default:
      return assertNever(state); // 컴파일 에러 발생!
  }
}
```


## 판별식 유니온(Discriminated Union) + Exhaustiveness 설명

### 1. **판별식 유니온이란?**

**공통된 리터럴 타입 속성**을 가진 유니온 타입입니다.

```typescript
// 공통 판별자: 'status' 필드
type LoadingState = { status: 'loading'; progress: number; };
type SuccessState = { status: 'success'; data: string; };
type ErrorState = { status: 'error'; message: string; };

// 판별식 유니온
type AppState = LoadingState | SuccessState | ErrorState;
```

### 2. **판별식 유니온의 장점**

#### **타입 안전성**
```typescript
function handleState(state: AppState): string {
  switch (state.status) {
    case 'loading':
      // TypeScript가 state를 LoadingState로 인식
      return `로딩 중... ${state.progress}%`;
    case 'success':
      // TypeScript가 state를 SuccessState로 인식
      return `성공: ${state.data}`;
    case 'error':
      // TypeScript가 state를 ErrorState로 인식
      return `에러: ${state.message}`;
  }
}
```

### 3. **Exhaustiveness (완전성) 체크**

**모든 가능한 케이스를 처리했는지 컴파일 타임에 확인**하는 기능입니다.

#### **never 타입 활용**
```typescript
function assertNever(x: never): never {
  throw new Error("처리되지 않은 케이스: " + x);
}
```

#### **완전성 체크 동작**
```typescript
function handleState(state: AppState): string {
  switch (state.status) {
    case 'loading':
      return `로딩 중... ${state.progress}%`;
    case 'success':
      return `성공: ${state.data}`;
    // case 'error':  // 만약 이 케이스를 빼먹으면?
    //   return `에러: ${state.message}`;
    default:
      return assertNever(state); // ← 컴파일 에러 발생!
  }
}
```

### 4. **실제 활용 사례**

#### **API 응답 처리**
```typescript
type ApiResponse = 
  | { type: 'success'; data: any }
  | { type: 'error'; code: number; message: string }
  | { type: 'loading' };

function handleApiResponse(response: ApiResponse) {
  switch (response.type) {
    case 'success':
      return response.data;
    case 'error':
      throw new Error(`${response.code}: ${response.message}`);
    case 'loading':
      return null;
    default:
      return assertNever(response); // 새로운 타입 추가 시 여기서 에러
  }
}
```

#### **React 상태 관리**
```typescript
type FormState = 
  | { status: 'idle' }
  | { status: 'submitting' }
  | { status: 'success'; result: string }
  | { status: 'error'; error: string };
```

### 5. **기존 코드와 비교**

#### **기존 방식 (현재 A, B 타입)**
```typescript
type A = {id: number };
type B = {name : string};

// 판별자가 없어서 'in' 연산자 사용
function f(v: A | B) {
  if("id" in v) {
    v.id;
  } else {
    v.name;
  }
}
```

#### **판별식 유니온 방식**
```typescript
type A = { type: 'A'; id: number };
type B = { type: 'B'; name: string };

function f(v: A | B) {
  switch (v.type) {  // 판별자로 구분
    case 'A':
      return v.id;
    case 'B':
      return v.name;
    default:
      return assertNever(v); // 완전성 체크
  }
}
```

### 6. **핵심 포인트**

1. **판별자**: 공통된 리터럴 타입 속성 (`status`, `type` 등)
2. **타입 좁히기**: switch/if문으로 자동 타입 추론
3. **완전성 체크**: `never` 타입으로 모든 케이스 처리 확인
4. **확장성**: 새로운 케이스 추가 시 컴파일러가 누락 체크

이 패턴은 **상태 관리, API 응답 처리, 에러 핸들링** 등에서 매우 유용합니다!