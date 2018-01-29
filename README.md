#ReactBoilerテンプレート

## webpack.config.jsを編集
```js

var webpack = require('webpack');


module.exports = {
  entry: [
    'script!jquery/dist/jquery.min.js',
    'script!foundation-sites/dist/foundation.min.js',
     './app/app.jsx',
  ],
  externals: {
    jquery: 'jQuery'
  },
  plugins: [
    new webpack.ProvidePlugin({
      '$': 'jquery',
      'jQuery': 'jquery'
    })
  ],
  output: {
    path: __dirname,
    filename: './public/bundle.js'
  },
  resolve: {
    root: __dirname,
    alias: {
      Main: 'app/components/Main.jsx',
      //ここ以外を消す
      applicationStyles: 'app/styles/app.scss'
    },
    extensions: ['', '.js', '.jsx']
  },
  module: {
    loaders: [
      {
        loader: 'babel-loader',
        query: {
          presets: ['react', 'es2015', 'stage-0']
        },
        test: /\.jsx?$/,
        exclude: /(node_modules|bower_components)/
      }
    ]
  },
  devtool: 'cheap-module-eval-source-map'
};

```
## component,Api,app.jsx,styles等を編集
- Main以外は基本削除

```js
var React = require('react');


var Main = (props) => {
  return (
    <div>
      <Nav/>
      <div>
        <div>
          <p>Main.jsx Rendered</p>
          {props.children}
        </div>
      </div>
    </div>
  );
}

module.exports = Main;

```
- ターミナルで`webpack`,`node server.js`を起動
- ポートが塞がっている場合は起動できないので注意！
```js
//今回のappはserver.jsを用意してnodeから起動している
var express = require('express');

//create our app
var app = express();
//ここを変更すれば起動できる
const PORT = process.env.PORT || 3004;



app.use(function (req,res,next){
  if(req.headers['x-forwarded-proto'] === 'https') {
    res.redirect('http://' + req.hostname + req.url);

  }else{
    next();
  }
});

app.use(express.static('public'));

app.listen(PORT,function(){
  console.log('Express server is up on port' + PORT);
});

```

## npmで再起動できるので前のプロジェクのnpmを削除する方法
- `rm -rf node_modules/`を行う
- `rs -a`で削除後を確認
- gitも削除しておくので`rm -rf .git`をしておく
- この準備でプロジェクトの雛形ができる
