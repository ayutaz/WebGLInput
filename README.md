# forked WebGLInput

WebGLInputからの変更点
- input領域はデフォルトで非表示
- input領域は画面上部に存在はするが隠している
  - 選択時にinput領域が隠れるとUnityのViewサイズが変わるため
- 主なコミット、PR
  - https://github.com/tetsujp84/WebGLInput/pull/1/files

# How to use
クローン後、WebGLInput/Assets/WebGLSupport/をコピーしてプロジェクトに追加してください

# WebGLInputの仕組みについて
1. InputFieldの入力を検知すると、WebGLInputのプラグインを通してブラウザのinput要素を生成する
    1. htmlファイルにinput要素がUnityのView（unity-container）とは独立して動的に追加される
1. 以降はinputの内容が変更されるとUnityに変更内容を通知する

# UnityのViewサイズ変更について
input要素の位置がキーボードにより隠れてしまうとView全体を上に移動するようになっている。そのためinput要素が生成される位置を常にトップとすることでこの問題を回避した。

InputFieldが画面下部にあり、入力中の文字が視認できない場合はUnity側でInputFieldと連動した表示欄を作っておくか、WebGLInput.jslibを改造して位置やサイズを個別で調整すること。
