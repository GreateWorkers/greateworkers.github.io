---
layout: default
title: webpack
nav_order: 5
parent: django
previous: crontab
next: crontab_user
---
{% include nav_btn.html %}

# webpack
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## webpack 

처음 사용해 봄

15년 전에는 js로 만들어 넣던 줄만 알았는데...

```
pip install django-webpack-loader
```

```
INSTALLED_APPS = {
	...
	'webpack_loader',
}

WEBPACK_LOADER = {
    'DEFAULT': {
        'BUNDLE_DIR_NAME': 'bundles_base/',
        'STATS_FILE': os.path.join(BASE_DIR, 'webpack-stats.json'),
    },
}
```

혹은

```
WEBPACK_LOADER = {
    'DEFAULT': {
        'BUNDLE_DIR_NAME': 'bundles_base/',
        'STATS_FILE': os.path.join(BASE_DIR, 'path/webpack-stats.json'),
    },

    'BASE': {
        'BUNDLE_DIR_NAME': 'bundles_base/',
        'STATS_FILE': os.path.join(BASE_DIR, 'path/webpack-stats.json'),
    },
}
```
[참고](https://github.com/owais/django-webpack-loader#multiple-webpack-projects)

---
## npm 아닌 yarn 으로

npm 보다 yarn 이 깔끔하게 dependency 가 관리되는 것 같음   
react-native 하면서 yarn 이 오류가 더 적었음

## npm yarn 명령어


npm 의 -D 은 --dev  
npm 의 -i --save 은 add  

django root 폴더에서 폴더하나 만들어서  
yarn init -y 으로 node_modules을 관리  
그 폴더에서 아래와 같이 명령어를 실행  

```
yarn init -y
yarn add --dev babel-loader babel-core webpack webpack-cli webpack-bundle-tracker sass-loader node-sass style-loader css-loader webpack-dev-server
yarn add clean-webpack-plugin
```

로컬 js 을 서브할 서버를 실행해서  
수정되는 코드를 즉시 반영해준다  

clean-webpack-plugin 은 프로덕션 레벨에서 빌드하면  
이전 번들 파일 삭제하는 플러그인  

---
### publicPath
publicPath를 output 에 꼭 설정해야함  
djangoServer 에설정해주면 연결되지 않음..(실수)

html 에서 불러오는 방법  
저러면 js을 불러오는 스크립트를 생성함  
{% raw %}
```
{% load render_bundle from webpack_loader %}
{% render_bundle 'main' %}
{% render_bundle 'admin' %}
```
{% endraw %}

---
## start and build
yarn run start  
yarn run build  

package.json
```
"scripts": {
    "start": "webpack-dev-server --config webpack.config.dev.js",
    "build": "webpack --mode production --config webpack.config.prod.js"
  },
```

## css

[참고](https://webpack.js.org/plugins/mini-css-extract-plugin/#options)

css는 js 에서 import 해주고 
전체에서 다 사용할 css는 전체에서 다 사용하는 js에서 
import 함  

그리고 
{% raw %}
```
{% render_bundle 'main' 'css' 'DEFAULT' %}
{% render_bundle 'main' 'js' 'DEFAULT' %}
```
{% endraw %}
등으로 사용


---
## example

나의 설정파일  
[참고](https://webpack.js.org/concepts/entry-points/#multi-page-application)

webpack-dev-config.js

```
var path = require('path');
var webpack = require('webpack');
var BundleTracker = require('webpack-bundle-tracker');
var WebpackDevServer = require('webpack-dev-server');
const publicPath = 'http://localhost:3030';

module.exports = {
  context: __dirname,
  entry:
    {
        menu: './src/app_menu.js',
        post: './src/post.js',
    }
,
  // webpack-dev-server 에서는 의미없음
  output: {
    filename: '[name]-[hash].js',
    publicPath: publicPath + '/static/bundles_base/',

  },
  devServer: {
    contentBase: path.join(__dirname, "../static/user/"),
    inline: true,
    hot: true,
    host: "localhost",
    port: 3030,
    headers: {
      'Access-Control-Allow-Origin': '*'
    },
    disableHostCheck: true, // process.env.NODE_ENV === 'development'
    },


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

  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new BundleTracker({filename: './webpack-stats.dev.json'})
  ]
```

webpack-prod-config.js

```
var path = require('path');
var webpack = require('webpack');
var BundleTracker = require('webpack-bundle-tracker');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');

module.exports = {
  context: __dirname,
  entry: {
        menu: './src/app_menu.js',
        post: './src/post.js',

    },
  output: {
      path: path.resolve('../static/bundles_base/'),
      filename: "[name]-[hash].js"
  },
    module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              // you can specify a publicPath here
              // by default it uses publicPath in webpackOptions.output
              publicPath: '../',
              hmr: process.env.NODE_ENV === 'development',
            },
          },
          'css-loader',
        ]
      }
    ]
  },
  optimization: {
    minimize: true, //Update this to true or false,
    minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
  },
  plugins: [
    new CleanWebpackPlugin(),
    new MiniCssExtractPlugin({
      // Options similar to the same options in webpackOptions.output
      // all options are optional
      filename: '[name]-[hash].css',
      chunkFilename: '[id].css',
      ignoreOrder: false, // Enable to remove warnings about conflicting order
    }),
    new BundleTracker({filename: './webpack-stats.prod.json'})
  ]
}
```
## webpack version 4.41.5

package.json 에 있는 

```
"devDependencies": {
    "babel-core": "^6.26.3",
    "babel-loader": "^8.0.6",
    "css-loader": "^3.4.2",
    "mini-css-extract-plugin": "^0.9.0",
    "node-sass": "^4.13.1",
    "sass-loader": "^8.0.2",
    "style-loader": "^1.1.3",
    "webpack": "^4.41.5",
    "webpack-bundle-tracker": "^0.4.3",
    "webpack-cli": "^3.3.10",
    "webpack-dev-server": "^3.10.1"
  },
```
{% include nav_btn.html %}
