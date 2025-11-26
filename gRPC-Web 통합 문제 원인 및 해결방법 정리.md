
## 발생한 문제

### 핵심 원인

**Webpack(system_editor) vs Vite(mars_editor) 모듈 시스템 차이**

|항목|system_editor (Webpack)|mars_editor (Vite)|
|---|---|---|
|모듈 시스템|CommonJS + ES Module|ES Module Only|
|`require()` 지원|✅ 네이티브 지원|❌ 브라우저 미지원|
|Node.js polyfill|✅ 자동 제공|⚠️ 플러그인 필요|
|proto 파일 형식|CommonJS (`module.exports`)|ES Module (`export`)|

### 구체적인 에러들

**1단계: CommonJS proto 파일 직접 사용 시도**

```javascript
// proto/task_manager_pb.js
var jspb = require('google-protobuf');  // ❌ require is not defined
module.exports = proto.taskmanager;      // ❌ module is not defined
```

에러: `The requested module does not provide an export named 'TaskArgs'`

 

**2단계: @rollup/plugin-commonjs 플러그인 시도**

```typescript
// vite.config.ts
import commonjs from '@rollup/plugin-commonjs'
plugins: [commonjs()]
```

에러: `Pre-transform error: Cannot read properties of undefined (reading '_container')` 원인: `vite-plugin-node-polyfills`와 충돌

 

**3단계: Dynamic Import 시도**

```typescript
// src/services/grpc/protoLoader.ts
const proto = await import('../../../proto/task_manager_pb.js');
```

에러: 여전히 `require is not defined` - CommonJS 코드 자체가 실행 불가

 

**4단계: 메서드 이름 불일치**

```typescript
// 잘못된 경로
'/taskmanager.TaskManager/StartTask'  // ❌
'/taskmanager.TaskManager/start_task' // ❌ 패키지명 오류
```

에러: `Method not found!`

## 해결 방법

### 최종 솔루션: TypeScript Proto 래퍼 작성

system_editor의 `.proto` 정의를 참고하여 ES Module 형식의 TypeScript 파일로 재작성

 

**1. Proto Message 클래스 ([src/proto/task_manager.ts](vscode-webview://095u75ciqjmekec37atqen3b8u1d08i68ah1e0rotj447ad2an55/src/proto/task_manager.ts))**

```typescript
import * as jspb from 'google-protobuf';

// CommonJS의 jspb.Message를 ES Module에서 사용 가능하도록 래핑
export class TaskArgs extends jspb.Message {
  private args_: string[] = [];
  
  serializeBinary(): Uint8Array {
    const writer = new jspb.BinaryWriter();
    TaskArgs.serializeBinaryToWriter(this, writer);
    return writer.getResultBuffer();
  }
  
  static deserializeBinary(bytes: Uint8Array): TaskArgs {
    const reader = new jspb.BinaryReader(bytes);
    const msg = new TaskArgs();
    return TaskArgs.deserializeBinaryFromReader(msg, reader);
  }
}
```

**2. gRPC Client 클래스 ([src/proto/task_manager_grpc.ts](vscode-webview://095u75ciqjmekec37atqen3b8u1d08i68ah1e0rotj447ad2an55/src/proto/task_manager_grpc.ts))**

```typescript
import * as grpcWeb from 'grpc-web';
import { TaskArgs, TaskId, TaskLog } from './task_manager';

// system_editor 백엔드와 호환되도록 정확한 메서드 경로 사용
const methodDescriptor_TaskManager_StartTask = new grpcWeb.MethodDescriptor(
  '/task.TaskManager/start_task',  // ✅ 패키지: task, 메서드: start_task
  grpcWeb.MethodType.UNARY,
  TaskArgs,
  TaskId,
  (request: TaskArgs) => request.serializeBinary(),
  TaskId.deserializeBinary
);

export class TaskManagerClient {
  start_task(
    request: TaskArgs,
    metadata: grpcWeb.Metadata | undefined,
    callback: (err: grpcWeb.RpcError, response: TaskId) => void
  ): grpcWeb.ClientReadableStream<TaskId> {
    return this.client_.rpcCall(
      this.hostname_ + '/task.TaskManager/start_task',
      request,
      metadata || {},
      methodDescriptor_TaskManager_StartTask,
      callback
    );
  }
}
```

**3. Service 래퍼 ([src/services/grpc/taskManagerService.ts](vscode-webview://095u75ciqjmekec37atqen3b8u1d08i68ah1e0rotj447ad2an55/src/services/grpc/taskManagerService.ts))**

```typescript
import { TaskManagerClient } from '../../proto/task_manager_grpc';
import { TaskArgs, TaskId, TaskLog } from '../../proto/task_manager';

export class TaskManagerService {
  private client: TaskManagerClient;
  
  async startTask(args: string): Promise<string> {
    const taskArgs = new TaskArgs();
    taskArgs.setArgsList(args.split(',').map(a => a.trim()));
    
    return new Promise((resolve, reject) => {
      this.client.start_task(taskArgs, {}, (err, response) => {
        if (err) reject(err);
        else resolve(response.getTaskId());
      });
    });
  }
}
```

## 정리한 내용

### 삭제된 파일들 (불필요)

```
❌ proto/task_manager_pb.js           - CommonJS proto (system_editor 복사본)
❌ proto/task_manager_grpc_web_pb.js  - CommonJS gRPC client
❌ proto/task_manager.proto           - 잘못된 패키지/메서드명
❌ src/services/grpc/protoLoader.ts   - 실패한 dynamic import 시도
❌ src/proto/index.ts                 - 불필요한 래퍼
```

### 제거된 의존성

```json
❌ "@rollup/plugin-commonjs": "^29.0.0"  - vite-plugin-node-polyfills와 충돌
```

### 유지된 설정

```json
✅ "vite-plugin-node-polyfills": "^0.24.0"  - google-protobuf의 Buffer/process 지원
✅ "google-protobuf": "^3.21.2"
✅ "grpc-web": "^1.4.2"
```

## 핵심 교훈

1. **모듈 시스템 호환성**: 브라우저 환경(Vite)에서는 CommonJS를 직접 사용할 수 없음
2. **빌드 도구 차이 이해**: Webpack의 자동 polyfill에 의존하지 말고 명시적으로 ES Module 작성
3. **백엔드 스펙 정확히 파악**: proto 패키지명(`task`)과 메서드명(`start_task`) 정확히 일치 필요
4. **플러그인 충돌 주의**: 여러 polyfill 플러그인은 오히려 문제 발생 가능

## 최종 구조

```
src/
├─ proto/
│  ├─ task_manager.ts         ✅ ES Module proto messages
│  └─ task_manager_grpc.ts    ✅ ES Module gRPC client
└─ services/grpc/
   └─ taskManagerService.ts   ✅ Promise-based service wrapper

proto/                         🗑️ 삭제됨 (CommonJS 파일들)
```

이제 Vite 환경에서도 system_editor 백엔드와 완벽하게 통신 가능합니다.