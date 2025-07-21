# NestJS Project 2025

<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="120" alt="Nest Logo" /></a>
</p>

## Project WorkSpace 폴더 생성

```bash
mkdir NestJS-2025-WorkSpace
cd NestJS-2025-WorkSpace
```

### 패키지 생성

- `Vanila JS`

```bash
npx @nestjs/cli new Nest-Hello
```

- TypeScript **strict mode**

```bash
npx @nestjs/cli new --strict Nest-Hello
```

- **Platform** 선택하여 설치

```bash
npm i --save @nestjs/platform-express
npm i --save @nestjs/platform-fastify
```

- Lint and autofix with eslint

```bash
npm run lint
```

- Format with prettier

```bash
npm run format
```

## @nestjs/cli 커멘드

![alt text](image-1.png)

## Project 계속

- **controller** 생성

```sh
npx @nestjs/cli g controller controller/home
```

- **service** 생성

```sh
npx @nestjs/cli g service service/home

```

- 이미 생성된 프로젝트에 **nestJS** 적용

```bash
npm i @nestjs/core @nestjs/common @nestjs/platform-express reflect-metadata typescript
```

### view Template engine 설치

Fastify Framework

```shell
npm i --save @fastify/static @fastify/view handlebars
```

pug engine

```shell
npm install pug
```

main.ts

```ts
const app = await NestFactory.create<NestExpressApplication>(AppModule, {
  bufferLogs: true,
  logger: false,
});
app.useLogger(new ConsoleLogger("NestApplication"));

// static 설정
app.useStaticAssets(join(__dirname, "..", "public"));

// pug 설정
app.setBaseViewsDir(join(__dirname, "..", "views"));
app.setViewEngine("pug");
```

main.js 에 설정

### nestJS 코드 편집 중 **Delete cr** 에러 해결법

#### 문제 원인

- nestjs를 사용하면 **eslint/prettier**를 자동으로 사용한다.
- nestjs를 cli로 생성을 하면 자동으로 설정되는 파일을 수정할 필요는 없지만 그대로 사용했을 때, Editor 화면에서 **Delete cr**이라는 이상한 에러를 볼 수 있다.
- **Delete cr**의 원인은 간단하게 windows에서 발생하는 오류로서, **prettier**와 **windows**의 개행방식에 차이에서 발생하는 것이다.

#### 원인 해결 방법

- **eslint**의 **prettier**의 개행방식을 `endOfLine: 'auto'`로 하면 된다.
  근데 저게 뭔지 궁금해서 더 파보기로한다.
- windows와 Linux는 각각 **CRLF**와 **LF**로 개행을 하는데 이게 문제가 된다.
- **LF**는 **Line Feed**로 줄을 먹인다는 의미이고 CRLF는 Carriage Return Line Feed로서 이것도 결국 **carriage**라는 **기계식 타자기** **고정대**를줄을 먹인다는 의미로서 둘다 결국 개행한다는 뜻인데 다르게 쓰는 것 같다.
  ![alt text](image.png)
- LF가 현대식 개행이고 CRLF가 과거의 개행방식인데 windows는 예전버전의 호환을 위해서 사용하고 있다고....
- 아직 잘 모르겠어서 eslint를 더 파보기로 했다.
- 공식문서에는 한마디로 LF와 CRLF의 메모리 저장에 차이가 있다. 그렇기 때문에 둘은 같은 개행이지만 다른개행인 것이고 이걸 통일할 필요가 있다고 말하고 있다.
- 결국 같은 개행이더라도 차이가 있으니 줄바꿈(endOfLine)을 자동(auto)으로 해놔라 라고 이해하면 되겠다.

#### 실행하기

- nest.js 프로젝트의 .eslintrc.js로 들어간다.

```javascript
rules: {
    '@typescript-eslint/interface-name-prefix': 'off',
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off',
    '@typescript-eslint/no-explicit-any': 'off',
  },
```

- .eslintrc.js의 rules 하단에 코드를 추가한다.
- `'prettier/prettier': ['error', { endOfLine: 'auto' }],`
- 작성이 완료된 코드

```javascript

rules: {
    '@typescript-eslint/interface-name-prefix': 'off',
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off',
    '@typescript-eslint/no-explicit-any': 'off',
    'prettier/prettier': ['error', { endOfLine: 'auto' }],
  },
```

## Jest Test 환경설정

- 테스트 결과 계층적으로 보여주기
- package.json 파일에 `"verbose":true` 항목 추가

```json
"jest": {
    "verbose": true,
    "moduleFileExtensions": [
      "js",
      "json",
      "ts"
    ],
    "rootDir": "src",
    "testRegex": ".*\\.spec\\.ts$",
    "transform": {
      "^.+\\.(t|j)s$": "ts-jest"
    },
    "collectCoverageFrom": [
      "**/*.(t|j)s"
    ],
    "coverageDirectory": "../coverage",
    "testEnvironment": "node"
  }

```
