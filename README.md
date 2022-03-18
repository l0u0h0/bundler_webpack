# WebPack  
- [WebPack](https://webpack.js.org/)
- `bundler`와는 달리 루트 경로에 `webpack.config.js` 파일을 생성해 직접 구성을 해줘야한다.
- 좀 더 세세하게 설정을 해줄 수 있기에 작은 프로젝트보다는 보다 큰 프로젝트에 주로 사용된다.
### webpack.config.js
- 브라우저가 아닌 `node.js` 환경에서 동작
- 그렇기에 `export` 할 때 `module.exports` 를 통해 객체 데이터를 내보낼 수 있음
- 속성  
`entry` - 파일을 읽어들이기 시작하는 진입점 설정  
`parcel`에서는 `parcel index.html`로 사용하지만 `webpack` 에서는 기본적으로  
`js` 파일을 진입점으로 잡음.  
`output` - 결과물(번들)을 반환하는 설정  
`parcel` 의 `dist` 디렉토리와 비슷한 개념  
`node.js` 에서 원하는 절대적인 경로를 설정해주어야한다.  
```js
const path = require('path')
output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js',
    clean: true
  }
```  
`__dirname` 은 해당 파일이 존재하는 경로를 의미한다.  
`clean: true` 는 구성이 바뀌었을 때 기존의 구성을 제거하는 의미이다.  
기본적으로 `path` 와 `filename` 은 기본값이다.
---
index.html을 이용해 개발 서버를 열기 위해서는  
- `html-webpack-plugin` 모듈을 설치 후  
- `webpack.config.js` 파일에 import 시켜준 뒤  
하위 속성에  
```js
plugin: [
    new HtmlPlugin({
      template: './index.html'
    })
  ]
```
부여해준다.