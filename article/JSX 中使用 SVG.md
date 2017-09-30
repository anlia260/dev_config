# JSX 中使用 SVG

1. webpack 增加 `svg-inline-loader`

  ```
  npm i svg-inline-loader --save-dev

  {
  test: /\.svg$/,
  loader: 'svg-inline-loader'
  }
  ```

2. JSX使用 svg

  ```
  const newad = require('../../public/newad.svg')
  <svg style={{fill: "#fff"}} dangerouslySetInnerHTML={{__html: newad }} />
  ```
