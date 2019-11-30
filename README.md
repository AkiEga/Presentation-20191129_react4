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

- 背景
	- 理由1: クライアントに送信するファイルサイズを縮小化させる
	- 理由2: javascriptの仕様差分吸収
		- ES6
		- ES2015
		- ES2018
		=> いちいち古い規格に合わせて相互変換するのが大変
			=> さらにtypescriptで書きたい場合も自動で変換してくれる
		
### webpackの動作の流れ
- webpackを使用する場合の典型的なファイル構成は下記
	```	shell
	webpack.config.js
	```
	- ワークフロー

- watch機能
	さらにwatch機能というのもあります。
	一度起動するとバックグラウンドでソース更新がある度に自動変換してくれるので大変便利です

### webpackでの変換結果
- 変換後のソースコード
	- 
	- ReactだとDOMをjsxで指定しますが、変換後のソースコードでは下記の様に変換されます
		- Before)
			```js
			```
		- After)
			```js
			```

- ソースマップ
	変換前と変換後のソースコードの対応関係を表したソースマップファイル(.map)が一緒に生成されます。
	IDEやエディターによってはソースマップを指定するとブレークポイントが止められるようになります。

- 参考) visual studio code(vscode)でのソースマップ設定例
	.vscode/launch.json  
	```json
	```

### React向けのWebpack設定方法
- 前提環境
	Webpack:
	Node.js
	React: 

- webpack.config.jsの設定例


