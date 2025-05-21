# Single executable apps の調査

## はじめに

node.js で作るアプリケーションを単一の exe にビルドしたい。

## 必要なインストール

このリポジトリを利用する前に、以下のツールやパッケージをインストールしてください。

### 前提

- [Node.js](https://nodejs.org/) v20 以上
- npm

### グローバルに必要なもの

- `postject`
  ```
  npm install -g postject
  ```

### プロジェクト内でインストールするもの

- TypeScript 関連
  ```
  npm install typescript ts-node @types/node tsx --save-dev
  ```

## Single executable apps(sea)

Single executable apps(sea)

[Single executable applications | Node.js v20.19.1 Documentation](https://nodejs.org/dist/latest-v20.x/docs/api/single-executable-applications.html#single-executable-applications)

## 手順

typescript で作った簡単なアプリケーションを exe 化していく。

### TypeScript コードをコンパイル

1. TypeScript をインストール

   > npm install typescript --save-dev

   > npm install ts-node --save-dev

   > npm install @types/node --save-dev

   > npm install tsx --save-dev

2. TypeScript のコンパイル設定ファイルを作成

> npx tsc --init

これにより、 tsconfig.json が生成される。コマンドで生成した tsconfig.json を下記の内容に変更する。

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "outDir": "./dist",
    "strict": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
```

3. src/index.ts の作成
   src/index.ts を作成して、console.log()を記載する。

4. TypeScript コードをコンパイル
   > npx tsc

これにより、src/ ディレクトリ内のコードが dist/ ディレクトリに JavaScript として出力される。

5. JavaScript ファイルの実行

> node dist/index.js

console.log の内容が出力される。

TypeScript ファイルを直接実行する場合は、

> npx ts-node src/index.ts

### SEA ファイルの作成

以下の手順で SEA ファイル(sea-prep.blob)を作成する。これは exe にデータを流し込むための中間ファイルである。

1. SEA ファイルのテンプレートを作成
   **sea-config.json** を作成し、下記内容で保存する。

```
{
  "main": "./dist/index.js",
  "output": "sea-prep.blob"
}
```

2. SEA ファイルを生成
   Node.js のバイナリを使用して SEA ファイルを生成する。

> node --experimental-sea-config sea-config.json

これで、sea-prep.blob が生成される。

### exe の生成

1. node 本体の実行ファイルをコピーする
   生 node をアプリケーション名にリネームしてコピーする。
   **hello.exe** の部分を自分がつけたい exe 名に変えるおまじないコマンドと思ってもらっていい。

> node -e "require('fs').copyFileSync(process.execPath, 'hello.exe')"

この段階で生成された exe は、node.exe と同じ挙動であるので注意。

2. バイナリ(hello.exe)に blob を注入する
   下記コマンドで、hello.exe に blob ファイルの内容を注入して、exe が実行されるようにする。

> npx postject hello.exe NODE_SEA_BLOB sea-prep.blob --sentinel-fuse NODE_SEA_FUSE_fce680ab2cc467b6e072b8b5df1996b2

3. バイナリの実行

> ./hello.exe
