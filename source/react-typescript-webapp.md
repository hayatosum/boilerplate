---
title: React + Typescript でWebアプリを開発する
date: 2020-07-16
---

# 環境構築

## Node.jsをインストールする

下記コマンドでNode.jsがインストール済みが確認できる。

```console
$ node -v
v12.18.2
```

## yarnをインストールする

下記コマンドでyarnがインストール済みが確認できる。

```console
$ yarn -v
1.22.4
```


# プロジェクトの操作

## プロジェクトを作成する

下記のコマンドでプロジェクトを作成できる。
`{プロジェクト名}`には、作成したい任意のプロジェクト名を設定する。

```console
$ npx create-react-app {プロジェクト名} --typescript
```

## プロジェクトを起動する

作成したプロジェクトのディレクトリに移動し、`{yarn start}`コマンドで起動できる。

```console
$ cd {プロジェクト名}
$ yarn start
```

起動コマンドを実行すると、http://localhost:3000 がブラウザで開かれる。

## プロジェクトを終了する

`Ctrl + C`で起動しているプロジェクトを終了（停止）できる。


# プロジェクトをGitで管理

## リポジトリにプッシュする

`create-react-app`コマンドで作成したプロジェクトには`.git`フォルダも自動で作成されるため、
ローカルリポジトリをリモートリポジトリにプッシュするという流れになる。

1. GitHub上で新規でリポジトリを作成する（READMEは作成しない）。
2. ロカールリポジトリを1で作成したリポジトリにプッシュする（下記コマンドを参照）。

```console
$ git init
$ git remote add origin git@github.com:YOUR_USER_NAME/YOUR_REPOSITORY_NAME.git
$ git push -u origin master
```

## リポジトリからクローンする

下記コマンドでクローンできる。

```console
$ git clone git@github.com:YOUR_USER_NAME/YOUR_REPOSITORY_NAME.git
```

gitからクローンしたままの状態のプロジェクトには、nodeのモジュールかインストールが含まれていないため、
プロジェクトのディレクトリで`npm install`を実行する必要がある。
