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
- `localhost`로 개발 서버를 열기 위해 하위 속성에  
```js
devServer: {
    host: 'localhost'
  }
```
---
`static` 폴더를 `dist` 폴더에 복사에 서버에서 사용하기 위해  
`copy-webpack-plugin` 모듈 설치 후 import,  
하위 속성으로  
```js
new CopyPlugin({
      patterns: [
        { from: 'static' }
      ]
    })
```
`plugins` 속성 내부에 작성해준다.
---
### module
- `static` 폴더 안에 `css` 폴더를 만들어 스타일을 적용할 파일을 만들어 줄 수 있지만,  
그냥 루트 경로에 `css` 폴더를 만든 후 모듈 두 개,  
`css-loader, style-loader` 를 설치 후  
```js
module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader',
          'css-loader'
        ]
      }
    ]
  },
```
- `css-loader`  
- `main.js` 에서 import 해서 css 파일을 사용할 때 js에서는 css를 해석할 수 없기에  
그것을 js에서 해석하는 용도로 사용됨.
- `style-loader`
- html의 스타일 태그 부분에 삽입할 수 있도록 하는 역할
- 순서는 `style-loader` 부터
- `scss` 를 사용하기 위해서 모듈 두 개,  
`sass-loader, sass`를 설치 후 모듈 속성에 `css-loader` 다음에 명시해준다.
---
- postcss, autoprefixer
- `postcss, autoprefixer, postcss-loader` 모듈 설치
- `postcss` 라는 스타일의 후처리를 도와주는 모듈 설치 후,  
`autoprefixer` 라는 공급업체접두사를 자동으로 붙여주는 플러그인 설치,  
그 다음 `postcss`를 사용하기 위해 `postcss-loader` 설치
- 이 후 사용은 `parcel` 번들러에서 사용한 것 처럼 브라우저 버전을 명시해준 뒤  
`.postcssrc.js` 파일을 생성한 뒤 모듈 명시해서 사용
---
### babel
- `@babel/core, @babel/preset-env, @babel/plugin-transform-runtime`  
모듈 설치 후 `.babelrc.js` 파일 생성한 뒤 다음 코드 명시
```js
module.exports = {
  presets: ['@babel/preset-env'],
  plugins: [
    ['@babel/plugin-transform-runtime']
  ]
}
```
- `plugins` 는 2차원 배열의 형태로 명시
- `webpack` 이 `babel` 을 읽을 수 있도록 설정 파일에 `rules` 객체에  
```js
{
  test: /\.js$/,
    use: [
      'babel-loader'
    ]
}
```
작성해준다.
- `babel-loader` 모듈 또한 설치해줘야 한다.