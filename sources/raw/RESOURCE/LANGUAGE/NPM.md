#NPM #Nodejs #PackageManager 
## 개요


Node.js 를 설치하면 함께 설치되는 패키지 매니져

- NPM을 통해 package를 가져오는 경우 package.json에 사용한 package에 대한 정보가 기록되어 
버전과 패키지 관리가 수월해짐
- cdn을 사용하지 않고 install 명령어를 통해 로컬 개발환경으로 라이브러리를 바로 들고와서 사용할 수있다는 장점

## COMMAND
#### Install 

`npm install ['package-name'] ['option']` 

- 명령어를 통해 해당 패키지를 설치 가능함 
- `--global` , `-g` 옵션을 통해 전역 위치에 패키지를 설치 할 수 있다.

###### 지역 설치 옵션 

`npm i vue -D` : `devDependencies` 
`npm i vue --save--dev`

- 하위 항목으로 생성됨. 개발용 라이브러리들이며, 배포시에는 포함되지 않는 라이브러리이다.
---
#### UnInstall 
`npm uninstall ['package-name']`  
- 해당 명령어를 통해 패키지와 함께 종속되어 있는 모듈을 삭제할 수 있다.
---

