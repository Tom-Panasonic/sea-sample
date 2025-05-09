# Single executable apps の調査

## はじめに

node.js で作るアプリケーションを単一の exe にビルドしたい。

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
