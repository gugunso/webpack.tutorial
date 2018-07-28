# webpack.tutorial
webpackが動く環境を作って、チュートリアルをやってみることにした。

## 参考URL
- webpack installation  
https://webpack.js.org/guides/installation/
- webpack getting-started  
https://webpack.js.org/guides/getting-started/
- nvm インストール  
https://github.com/creationix/nvm#install-script

## 概要
環境構築
- 下調べ
- 必要なミドルのインストール
- webpackのインストール

チュートリアル
- ？
- ？ 

の順番に進める。

# 環境構築
## 1.下調べ
google で検索語 「webpack」「チュートリアル」 などで検索した。  
簡単に調査したところ、本家のドキュメントを読めば十分ということがわかった。  

### 要件
webpackの要件について書いてある箇所
https://webpack.js.org/guides/installation/#pre-requisites
> Before we begin, make sure you have a fresh version of Node.js installed. 
> The current Long Term Support (LTS) release is an ideal starting point. 
> You may run into a variety of issues with the older versions as they may be missing functionality webpack and/or its related packages require. 

訳（超適当）
~~~ text
始める前に、フレッシュ！なバージョンのnode.jsがインストールされていることを確認せえよ。
LTSバージョンなら最高や。
古いバージョン使っとると様々な問題に出くわす可能性があるで。
webpackや関連パッケージに必要な機能が不足している可能性があるからや。
~~~


## 2.必要なミドルのインストール
前述の通り、LTSバージョンのnode.jsとnpmをインストールすれば良い。  
今回は、vagrantでcentos6環境を立て、nvmを入れていくことにした。  
※centos6環境構築の部分は割愛する。  

https://github.com/creationix/nvm#install-script

~~~ sh
# nvmのインストール
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
# インストール可能なバージョンの確認
nvm ls-remote
＃ lts(long term support)バージョンのインストール
nvm install --lts
# 確認 
node -v
npm -v
~~~

## 3.webpackのインストール
改めて説明することは特に無い。
下記URLを参考にやっていく。

https://webpack.js.org/guides/getting-started/

~~~ sh
npm init -y
npm install webpack webpack-cli --save-dev
~~~

## 4.apacheのインストールと設定
webpackとは直接関係ないが、ブラウザで動作確認するのでapacheをインストールして簡単に設定を行った。
 
~~~ sh
yum install httpd
~~~

設定変更箇所は、
- ドキュメントルートのパス
- apache実行ユーザー、グループ（お試し環境なので vagrant ユーザに変更）
の２点のみ。

以上で、環境構築が完了した。


# チュートリアル
## 1. https://webpack.js.org/guides/getting-started/ 

### なんでwebpackを使うのか？
https://webpack.js.org/guides/getting-started/#basic-setup
> It is not immediately apparent that the script depends on an external library.
> If a dependency is missing, or included in the wrong order, the application will not function properly.
> If a dependency is included but not used, the browser will be forced to download unnecessary code.

~~~ text
ぱっと見では外部ライブラリに依存していることが分からない
依存関係がないか、間違った順序で含まれていると、アプリケーションが正しく機能しない
依存関係が含まれているが使用されていない場合、ブラウザは不要なコードをダウンロードすることになる。
~~~ 

### どうやってやるのか？
簡単にまとめると、

1. 必要なパッケージをnpmを使ってインストールする  
1. javascript に、必要としているパッケージをimportするように書く。  
1. 2のjavacriptを元に、依存パッケージをバンドルしたjavascriptファイルを生成する  
1. htmlファイル側には、3でバンドルされたjsのパスを書く  

という流れ。


必要なパッケージのインストールは、npmで行う。
~~~ sh
npm install --save lodash
~~~

バンドルするコマンドは以下。
~~~ sh
npx webpack
~~~

バンドルする時、デフォルトでは webpack.config.js が設定ファイルとして使われる。  
明示的に設定ファイルを指定する場合は、下記オプションで指定すれば良い。  

~~~
npx webpack --config webpack.config.js
~~~

npm のビルドスクリプトにwebpack を指定しておけば、ビルド時にwebpackしてくれる。

~~~ sh
npm run build
~~~ 

