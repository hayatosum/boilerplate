---
title: React + TypeScript でWebアプリを開発する
date: 2020-07-16
---

# 環境構築

## Node.jsをインストールする

下記コマンドでNode.jsがインストール済みが確認できる。

```
$ node -v
v12.18.2
```

## yarnをインストールする

下記コマンドでyarnがインストール済みが確認できる。

```
$ yarn -v
1.22.4
```


# プロジェクトの操作

## プロジェクトを作成する

下記のコマンドでプロジェクトを作成できる。
`{プロジェクト名}`には、作成したい任意のプロジェクト名を設定する。

```
$ npx create-react-app {プロジェクト名} --typescript
```

## プロジェクトを起動する

作成したプロジェクトのディレクトリに移動し、`{yarn start}`コマンドで起動できる。

```
$ cd {プロジェクト名}
$ yarn start
```

起動コマンドを実行すると、http://localhost:3000 がブラウザで開かれる。

## プロジェクトを終了する

`Ctrl + C`で起動しているプロジェクトを終了（停止）できる。


# ディレクトリ構成

`create-react-app`コマンドで作成されるプロジェクトのディレクトリ構成は下記のようになっている。

```
.git
 L ...
node_modules
 L ...
public
 L favicon.ico
 L index.html
 L logo192.png
 L logo512.png
 L manifest.json
 L robots.txt
src
 L App.css
 L App.test.tsx
 L App.tsx
 L index.css
 L index.tsx
 L logo.svg
 L react-app-env.d.ts
 L serviceWorker.ts
 L setupTests.ts
.gitignore
package.json
README.md
tsconfig.json
yarn.lock
```


# Gitで管理

## リポジトリにプッシュする

`create-react-app`コマンドで作成したプロジェクトには`.git`フォルダも自動で作成されるため、
ローカルリポジトリをリモートリポジトリにプッシュするという流れになる。

1. GitHub上で新規でリポジトリを作成する（READMEは作成しない）。
2. ロカールリポジトリを1で作成したリポジトリにプッシュする（下記コマンドを参照）。

```
$ git init
$ git remote add origin git@github.com:YOUR_USER_NAME/YOUR_REPOSITORY_NAME.git
$ git push -u origin master
```

## リポジトリからクローンする

下記コマンドでクローンできる。

```
$ git clone git@github.com:YOUR_USER_NAME/YOUR_REPOSITORY_NAME.git
```

gitからクローンしたままの状態のプロジェクトには、nodeのモジュールかインストールが含まれていないため、
プロジェクトのディレクトリで`npm install`を実行する必要がある。


# 開発環境の設定

## コミット時にコードを自動整形する

下記のライブラリをインストールすることで、コミット時にコードを自動整形するようにできる。

- husky
- lint-staged
- pretter

下記のコマンドでインストールができる。
開発時のみに必要なライブラリのため、`-D`を付与し`devDependencies`としてインストールする。

```
$ yarn add -D husky lint-staged prettier
```

インストールが完了したら、`package.json`を以下のように編集する。

### lint-staged

拡張子が`ts`または`tsx`の場合に、`pretter`を走らせてから`git add`するようにする。

```json:package.json
{
  ...,
  "lint-staged": {
    "*.{ts,tsx}": [
      "prettier --write",
      "git add"
    ]
  }
}
```

### husky

`precommit`時にlint-stagedが実行されるようにする。

```json:package.json
{
  ...,
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  }
}
```

### pretter

コードフォーマット関連の設定を追加する。
- `semi`: 末尾にセミコロンをつけるか
- `singleQuote`: シングルクォートに変換するか
- `tabWidth`: インデントのスペースの数
- `trailingComma`: 複数行の場合に末尾にカンマをつけるか

```json:package.json
{
  ...,
  "prettier": {
    "semi": true,
    "singleQuote": true,
    "tabWidth": 2,
    "trailingComma": "all"
  }
}
```

上記以外のオプションについては、pretterの[公式ドキュメント](https://prettier.io/docs/en/options.html)で確認できる。

## 型チェックによる安全性を最大化する

`tsconfig`にオプションを設定することで、型チェックに関する下記の機能を適用させる。
- `strictNullChecks`: nullやundefinedが「一人前の型」になる
- `noImplicitAny`: 暗黙のany型を禁止する
- `noImplicitThis`: thisに型を指定していない場合にエラーを発生させる
- `noImplicitReturns`: 関数内のすべての経路で返り値の型があっているかをチェックする
- `noUnusedLocals`:  宣言して一度も使われないローカル変数があるとコンパイルエラーになる
- `noUnusedParameters`：一度も使われないパラメータがあるとコンパイルエラーになる

```json:tsconfig.json
{
  "compilerOptions": {
    ...,
    "strictNullChecks": true,
    "noImplicitAny": true,
    "noImplicitThis": true,
    "noImplicitReturns": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
  },
  ...
}
```

上記以外のオプションについては、TypeScriptの[公式ドキュメント](https://www.typescriptlang.org/docs/handbook/compiler-options.html)で確認できる。

## importを絶対パスにする

相対パスでimportするのは色々と面倒なため、絶対パスで指定できるようにする。

```json:tsconfig.json
{
  "compilerOptions": {
    ...,
    "baseUrl": "src"
  },
  ...
}
```

例えば、`src/components/Button.tsx`にあるモジュールをインポートする場合、下記のように記載することでインポートできる。

```typescript
import Button from 'components/Button'
```

詳しい使用方法は[公式ドキュメント](https://create-react-app.dev/docs/importing-a-component/#absolute-imports)で確認できる。


# 備考

- ELECTRONというフレームワークを利用するとWebアプリをデスクトップアプリケーション化できる（[参考](https://qiita.com/umamichi/items/6ce4f46c1458e89c4cfc)）。


# 参考資料

- create-react-appで React + Typescript な環境を構築する: https://qiita.com/sunnyG/items/05c2e9381d6ba2d9fccf
- package.jsonにおけるdependenciesとdevDependenciesの違い(超シンプルに): https://qiita.com/fj_yohei/items/0b7fc2bc826df23935db

