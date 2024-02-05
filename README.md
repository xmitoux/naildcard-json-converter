# Naildcard用Wildcard JSONコンバーター

https://xmitoux.github.io/naildcard-json-converter/

## 概要

-   Naildcardの旧形式のWildcardデータを新形式に変換するためのツールです。
-   `v1.4.0` からはWildcardのSimple Editorがなくなるため、以前の形式のWildcardデータを取り込めません。  
    旧データを移行したい場合はこのツールで新形式に変換してからv1.4.0にインポートしてください。

## 使い方

1. Naildcard `v1.3.0` 以前の Simple Editor からWildcardデータをコピーし、左のテキストエリアに貼り付けます。
2. Convertボタンを押します。
3. 右のテキストエリアに新形式のデータが出力されます。
4. データをコピーして適当なテキストエディタに貼り付け、`.json`で保存します。
5. Naildcard を`v1.4.0` にアップデートします。
6. Wildcard Manager の`Import Wildcards`ボタンを押し、保存したJSONファイルを取り込めば移行完了です。
