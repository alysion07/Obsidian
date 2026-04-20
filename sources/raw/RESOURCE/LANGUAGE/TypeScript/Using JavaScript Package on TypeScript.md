# Declaration Files

- `.d.ts` 파일을 의미한다.
- `lib` 에 추가한 **JS** 패키지나 모듈에 선언되어 있는 함수의 형태를 **TS**에서 인식할 수 있도록 형태를 정의한 파일. **C**언어의 `.h` 파일과 유사한 기능을 수행함


## `.d.ts`파일 활용

```js title:'JS package'
export function init(config) {  
    return true;  
}  
  
export function exit(code) {  
    return code + 1;  
}
```

```ts title:"'.d.ts' file for 'ts file'"
interface Config {  
    url: string;  
}   
declare module "myPackage" {  
    export function init(config: Config): boolean;  
  
    export function exit(code): number;  
}
```

```ts title:"import 'js package' in 'ts' file"
// d.ts 파일에 선언된 내용을 통해 형태를 인식함
import {init, exit} from "myPackage";  
  
init({  
    url: "https://google.com"  
})
```


## `tsconfig.json`파일 활용

- **JSDoc**와 함께 활용

```json title:tsconfig.json hl:11
{  
  "compilerOptions": {  
    "target": "es2016",  
    "module": "commonjs",  
    "esModuleInterop": true,  
    "forceConsistentCasingInFileNames": true,  
    "strict": true,  
    "skipLibCheck": true,  
    "outDir": "build",  
    "lib": ["ES6", "DOM"],  
    "allowJs": true,  
  },  
  "include": ["src"],  
}
```

```ts title:"ts file"
// js 파일을 직접 불러오게되면 checker가 오류를 발생시키지만 allowJs를 설정해 직접 로드하여 사용할 수 있도록 가능
import {init, exit} from "./myPackage";

init({url: "https://google.com"});
```

```js title::"modifying the comment using 'jsDoc'"   hl:1
// @ts-check  
/**  
 * initialize the project 
 * @param {object} config  
 * @param {boolean} config.debug  
 * @param {string} config.url  
 * @returns boolean  
 */  
export function init(config) {  
    return true;  
}  
  
/**  
 * Exits the program 
 * @param {number} code  
 * @returns number  
 */  
export function exit(code) {  
    return code + 1;  
}
```

**JSDoc**를 활용하여 함수의 형태를 `comment`로 작성하면  *js package*를 별도의 수정 없이 사용할 수 있다.