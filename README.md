# じゃんけんゲーム

> ハンズオンで利用するじゃんけんゲームのソースです。デュフフなどにご利用ください

## テンプレート作成コマンド
```bash
# vue-cliをインストール
npm install vue-cli -g
# vue-cliで生成
vue init browserify-simple janken
# 生成されたディレクトリへ移
cd janken
# node_modulesをインストール
npm install
# 付け足しでインストール
npm install vueify --save 
npm install socket.io
# vue-routerをインストール こちらは第２部で使います
npm install vue-router -S
# サーバー立て
npm run dev
```

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For more information see the [docs for vueify](https://github.com/vuejs/vueify).
