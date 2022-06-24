# stylelintを入れてCSSのチェックを自動化しよう　| VScode

## 想定読者

- node.jsのインストールが終わっている
- npmコマンドの触り程度は理解している

## プロジェクトを作成しよう

まずはフォルダを作成して、VScodeで開きましょう。
フォルダ名は`stylelint-training`にしましょうか。

## 拡張機能をインストールしよう

Stylelint公式のVScode拡張をインストールします。

[stylelint - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)

上記リンクからページに行き、「install」をクリックします。

できない場合は左メニューのMarketplaceから検索窓に「stylelint」と入力して、蝶ネクタイのアイコンを目印にインストールしてください。


## npm initをしよう。

プロジェクト`stylelint-training`をVScodeで開き、
上部メニューの「表示」からターミナルを表示します。

ターミナルに

```shell
npm init -y
```

と打ち込んでEnterキーを押しましょう。

プロジェクト内にpackage.jsonが作成されたら成功です。

## stylelintをインストールしよう

次に`stylelint`をインストールします。

下記の公式ページの指示に従って操作しましょう。

[Getting started | Stylelint](https://stylelint.io/user-guide/get-started/)

```shell
npm install --save-dev stylelint stylelint-config-standard
```

`node_modules`というフォルダが作成されたら成功です。

## .vscodeフォルダを作成しよう

次にプロジェクトルート（package.jsonやnode_modulesが作成された場所）に`.vscode`というフォルダを作成します。
この中に、`settings.json`のファイルを作成し、設定を行います。

## .vscode/settings.jsonを作成しよう

`.vscode`フォルダ内に`settings.json`を作成します。

.vscode/settings.json

```json
{
  "css.validate": false,
  "less.validate": false,
  "scss.validate": false
}
```

これはVScode内蔵のlinterを動作させないための設定です。
VScodeはデフォルトでlinterを内蔵しており、そのままにするとStylelintと同時に動いて、エラーが二重に表示されます。

私の環境では`less`は利用しないので、`css`と`scss`のみ入れます。
その他、設定をいれて、完成した`settings.json`は下記です。

```json
{
  "css.validate": false,
  "scss.validate": false,
  "stylelint.validate": ["css", "scss"],
  "[css]": {
    "editor.formatOnSave": false,
    "editor.codeActionsOnSave": {
      "source.fixAll.stylelint": true
    }
  },
  "[scss]": {
    "editor.formatOnSave": false,
    "editor.codeActionsOnSave": {
      "source.fixAll.stylelint": true
    }
  },
  "stylelint.configFile": "./.stylelintrc"
}
```

１つずつ見てきましょう。

```json
  "[css]": {
    "editor.formatOnSave": false,
    "editor.codeActionsOnSave": {
      "source.fixAll.stylelint": true
    }
  },
  "[scss]": {
    "editor.formatOnSave": false,
    "editor.codeActionsOnSave": {
      "source.fixAll.stylelint": true
    }
  },
```

この部分は「`css`と`scss`ファイルに対して、`{...}`　に記述されている設定を有効にしてください。」という意味です。

` "editor.formatOnSave": false`はVScodeがデフォルトで内蔵するコードフォーマッタ（自動整形ツール）が動かないようにするためです。

```json
    "editor.codeActionsOnSave": {
      "source.fixAll.stylelint": true
    },
```

この部分は、「ファイルが保存されたときに自動で修正できる箇所を修正してください。」という意味です。

```json
 "stylelint.configFile": "./.stylelintrc"
```

この部分は、「stylelingの設定はこのファイルを見てください」という意味です。

他にも良いルールがあれば随時追加していくのが良いでしょう。
`settings.json`については一旦ここまで。

## .stylelintrcを作成しよう

次にプロジェクトルートディレクトリに`.stylelintrc`ファイルを作成します。
この中には実際に利用するStylelintのルールを記載します。
Stylelintはこのファイルを読み込み、その内容にしたがって、エラーの出力と自動整形を行ってくれます。

このファイルは複数の形式をサポートしていて、`.stylelintrc`はJSONまたはYAML形式です。
それ以外には`.js`や`.cjs`ファイルも利用することができます。
詳しくは、下記公式を確認してください。

[Configuration | Stylelint](https://stylelint.io/user-guide/configure/)

またファイル名も自由に設定が可能で、独自の命名を行いたい場合は、前述の`settings.json`の"stylelint.configFile"にファイルへのパスを記述しておくと読み込まれるようになります。

一旦書いてみましょう。

```json
{
  "rules": {
    "block-no-empty": true,
    "no-duplicate-selectors": null,
    "number-leading-zero": "always"
  }
}
```

今回は基礎的なものになりますが、3つに限定してルールを設定しておきます。

- 空のブロックを許可しない
- 重複したセレクターを許可しない
- １未満の数値で頭の0を省略しない


ここで、true / false, null, stringと出てきましたが、どのような項目があるか、どのような値が設定できるかは公式で確認が可能です。
少し癖のあるプロパティ名になりますが、いくつか眺めてみるとどのように検索すればよいか分かってくると思います。

[List of rules | Stylelint](https://stylelint.io/user-guide/rules/list/)

## 実際に動かしてみよう

```
npx stylelint "**/*.{css, scss}"
```

おっとエラーに、、、

```shell
When linting something other than CSS, you should install an appropriate syntax, e.g. "postcss-scss", and use the "customSyntax" option
```

なになに、、、css以外の解析を行うには"postcss-scss"と"customSyntax"の設定を行うことが必要、、、と

こうですね。

```diff
{
+  "customSyntax": "postcss-scss",
  "rules": {
    "block-no-empty": true,
    "no-duplicate-selectors": null,
    "number-leading-zero": "always"
  }
}
```

このままでは"postcss-scss"は無い状態なのでインストールします。

```
npm i -D postcss-scss
```

では改めて

```
npx stylelint "**/*.{css,scss}"
```

無事に動きました。