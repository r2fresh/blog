# Vue-cli를 사용한 프로젝트에서 prettier관련 에러 해결

## 오류내용
``` bash
Module build failed: Error: No parser and no file path given, couldn't infer a parser.
    at normalize (/Users/r2fresh/workspace/kt/gis/gistool/node_modules/prettier/index.js:7051:13)
    at formatWithCursor (/Users/r2fresh/workspace/kt/gis/gistool/node_modules/prettier/index.js:10370:12)
    at /Users/r2fresh/workspace/kt/gis/gistool/node_modules/prettier/index.js:31115:15
    at Object.format (/Users/r2fresh/workspace/kt/gis/gistool/node_modules/prettier/index.js:31134:12)
    at Object.module.exports (/Users/r2fresh/workspace/kt/gis/gistool/node_modules/vue-loader/lib/template-compiler/index.js:80:23)
```

## 1. 간단한 해결 방법

먼저 `node_modules\vue-loader\lib\template-compiler\index.js` 파일을 수정하기 위해 에디터에서 연다.

아래의 소스코드를 찾는다.
``` javascript
// prettify render fn
    if (!isProduction) {
      code = prettier.format(code, { semi: false})
    }
```
소스코드를 아래와 같이 수정한다.
``` javascript
// prettify render fn
if (!isProduction) {
  code = prettier.format(code, { semi: false, parser: 'babylon' })
}
```

수정 후 다시 서버를 실행

## 2. Prettier Downgrade

vue-loader에서 사용하는 prettier의 버전이 v1.12.0 이하에 최적화 되어 있기 때문에 package.json에 있는 dependencies의 prettier의 버전이 v1.13.0일경우 에러가 발생한다. 아래와 같이 이전 버전을 삭제하고 Downgrade된 버전을 설치해야 한다.

```
// module 삭제 및 package.json내용도 삭제
$> npm uninstall --save prettier

// prettier v1.12.0 버전 설치
$> npm install --save prettier@1.12.0
```

완료 후 다시 서버를 실행