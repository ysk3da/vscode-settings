# VSCodeの設定を再編する

VSCodeを利用してきて、３年以上経ちました。
導入当時はHTML、CSS、JavaScript、JQueryを中心にマークアップをする仕事が大半でしたが、
現在は、React、VueなどのNode.jsを利用したもの、Flutterなど利用シーンに際して使いたい拡張機能が色々と異なる場合が多いです。

まずはユースケースの仕分けを行います。

## ユースケース

- HTML,CSS,JavaScriptの環境: 社員のメンバーほぼ全員に必要
- WordPressの制作環境
- React系の制作環境: Next、Gatsuby、React Appなど
- Flutter: これに関してはAndoroid Studioが優秀なので必須ではない

## HTML,CSS,JavaScriptの環境

欲しい機能

- ESlintによる自動整形
- Stylelintによる自動整形

このあたりは社内ルールの統一につながるので利用したいところ

