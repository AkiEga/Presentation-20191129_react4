# Presentation-20191129_react4
## Agenda 
- Webpack導入方法 (18:30~19:00)
	- webpackとは
	- React向けwebpack導入方法
- もくもく会 (19:30~21:00)
	- 参考) 自習ネタ  
		- [AkiEga/Presentation-20190824_react1](https://github.com/AkiEga/Presentation-20190824_react1)
			- [新しい React アプリを作る – React](https://ja.reactjs.org/docs/create-a-new-react-app.html)
			- [チュートリアル：React の導入 – React](https://ja.reactjs.org/tutorial/tutorial.html)
			
## Webpack導入方法
### webpackとは
- 説明: 
	ローカル環境で開発したjavascript(以降js)やtypescript(以降ts)を  
	クライアント環境上で動作する様にソースコード変換してくれるツール  
	[webpack](https://webpack.js.org/)

- 背景:  
	- 理由1: クライアントに送信するファイルサイズを縮小化させる  
	- 理由2: javascriptの仕様差分吸収  
		- ES6
		- ES2015
		- ES2018
		=> いちいち古い規格に合わせて相互変換するのが大変  
	- 理由3: typescript→javascriptへの変換
	- 理由4: jsx等のコード内のhtml的記述の変換  
		参考) [JSX を深く理解する – React](https://ja.reactjs.org/docs/jsx-in-depth.html)
		
### webpackを使うには
- ファイル構成
	```	shell
	<workspaceRoot>/
		.git/
		dist/ 	# 変換後ソース置き場
			bundle.js
			bundle.js.map
		src/ 	# 変換前ソース置き場
			index.(jsx|tsx)
			...
		package.json	# npm設定やscript項ではwebpackのbuildコマンドが書いてあります
		index.html		# htmlエントリーポイント(dist内ファイルを呼び出してます)
		tsconfig.json	# typescript設定
		webpack.config.js # webpack設定
		.gitignore
		Readme.md
	```
#### webpack.config.js
```js
module.exports = {
	mode: 'development',
	entry: __dirname + '/src/index.tsx',
	output: {
	  path: __dirname + '/dist',
	  filename: 'bundle.js'
	},
  
	devtool: 'hidden-source-map',
  
	resolve: {
	  extensions: ['.ts', '.tsx', '.js']
	},
  
	module: {
	  rules: [
		{
		  test: /\.tsx?$/,
		  use: [
			{loader: 'ts-loader'} // ts-loaderにてtypescriptの変換を実施、詳細はtsconfig.jsonにて
		  ]
		}
	  ]
	}
};
```
- tsconfig
```json
{
	"compilerOptions": {
		"strictNullChecks": true,
		"noUnusedLocals" : true,
		"noImplicitThis": true,
		"alwaysStrict": true,
		"outDir": "./dist/",
		"sourceMap": true,
		"noImplicitAny": true,
		"lib": ["dom", "ES2017"],
		"module": "ES2015",
		"target": "es5",
		"jsx": "react" // 実はここでReactを指定して、tsx→js変換を実現してます
	}
}
```
#### webpack のコマンド
- Build: コード変換実施
コマンド例)  
`webpack --colors --config ./webpack.config.js`

- Watch: コード更新監視→差分Build実施
さらにwatch機能というのもあります。  
一度起動するとバックグラウンドでソース更新がある度に自動変換してくれるので大変便利です  
コマンド例)  
`webpack --watch --colors --config ./webpack.config.js`

### webpackでの変換結果
- 変換後のソースコード
	- 
	- ReactだとDOMをjsxで指定しますが、変換後のソースコードでは下記の様に変換されます
		- Before) ./src/hello.tsx  
			```js
			import * as React from 'react';

			export interface Props {
				content: string;
			}

			export default class MyComponent extends React.Component<Props, {}> {
				render() {
					return <div>{this.props.content}</div>
				}
			}
			```
		- After) ./src/hello.tsx  
			```js
			/***/ "./src/hello.tsx":
			/*!***********************!*\
			!*** ./src/hello.tsx ***!
			\***********************/
			/*! exports provided: default */
			/***/ (function(module, __webpack_exports__, __webpack_require__) {

			"use strict";
			__webpack_require__.r(__webpack_exports__);
			/* harmony import */ var react__WEBPACK_IMPORTED_MODULE_0__ = __webpack_require__(/*! react */ "./node_modules/react/index.js");
			/* harmony import */ var react__WEBPACK_IMPORTED_MODULE_0___default = /*#__PURE__*/__webpack_require__.n(react__WEBPACK_IMPORTED_MODULE_0__);
			var __extends = (undefined && undefined.__extends) || (function () {
				var extendStatics = function (d, b) {
					extendStatics = Object.setPrototypeOf ||
						({ __proto__: [] } instanceof Array && function (d, b) { d.__proto__ = b; }) ||
						function (d, b) { for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p]; };
					return extendStatics(d, b);
				};
				return function (d, b) {
					extendStatics(d, b);
					function __() { this.constructor = d; }
					d.prototype = b === null ? Object.create(b) : (__.prototype = b.prototype, new __());
				};
			})();
			```

- ソースマップ
	変換前と変換後のソースコードの対応関係を表したソースマップファイル(.map)が一緒に生成されます。
	IDEやエディターによってはソースマップを指定するとブレークポイントが止められるようになります。

### React向けのWebpack設定方法
- 前提環境
	Webpack:
	Node.js
	React: 

#### React向けのwebpack設定例
[AkiEga/Presentation-20191129_react4_Appendix1_react_with_webpack](https://github.com/AkiEga/Presentation-20191129_react4_Appendix1_react_with_webpack)

### 最後に
上記webpack、webpack.config.js等のファイル設定が面倒だと思いますが、
実は[parcel](https://parceljs.org/getting_started.html)というソフトだとファイル設定ほぼ無しで出来たりします。
  
- parcel コマンド例
	```shell
	# build
	parcel index.html
	# watch
	parcel watch index.html
	```
  
webpackは詳細な設定が出来るのが利点ですが、もし設定が面倒な場合はparcelで対処するのも一つの選択肢です
