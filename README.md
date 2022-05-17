# forked WebGLInput

WebGLInputからの変更点
- input領域はデフォルトで非表示
- input領域は画面外上部に存在はするが隠している
  - 選択時にinput領域が隠れるとUnityのViewサイズが変わるため
- モバイルの判定にiPad向けも含める
- 入力のフォーカスを外れた際にInputFieldの入力をクリアできるようにした

# How to use
WebGLInput/Assets/WebGLSupport/をコピーしてプロジェクトに追加してください

# WebGLInputの仕組みについて
1. InputFieldの入力を検知すると、WebGLInputのプラグインを通してブラウザのinput要素を生成する
    1. htmlファイルにinput要素がUnityのView（unity-container）とは独立して動的に追加される
1. 以降はinputの内容が変更されるとUnityに変更内容を通知する

# 変更点の詳細
## UnityのViewサイズ変更について
input要素の位置がキーボードにより隠れてしまうとView全体を上に移動するようになっている。そのためinput要素が生成される位置を常にトップとすることでこの問題を回避した。https://github.com/tetsujp84/WebGLInput/blob/master/Assets/WebGLSupport/WebGLInput/WebGLInput.jslib#L40-L44

InputFieldが画面下部にあり、入力中の文字が視認できない場合はUnity側でInputFieldと連動した表示欄を作っておくか、WebGLInput.jslibを改造して位置やサイズを個別で調整すること。

index.htmlのconfig.devicePixelRatio = 1;がコメントアウトされていること。これが有効になっているとAndroidでズームされてしまい、Viewサイズが崩れる。

## iPadの対応について
iPadはApplication.isMobilePlatformがfalseになる。また、WebGLの場合はApplication.platformがWebGLPlayerになるなどiPadの判定が正確に取れないため、強制的にモバイル判定をさせるフラグを別で追加している。
https://github.com/tetsujp84/WebGLInput/blob/master/Assets/WebGLSupport/WebGLInput/WebGLInput.cs#L127

## 入力のフォーカスを外れた際にInputFieldの入力もクリアする
InputField内に直前まで入力していた文字が残ってしまうため、入力完了時に消すフラグを追加した。
この方法以外にもInputFieldのonEndEditで明示的に消してもよい。
https://github.com/tetsujp84/WebGLInput/blob/master/Assets/WebGLSupport/WebGLInput/Wrapper/WrappedInputField.cs#L98

## InputFieldのonEndEditについて
Unity2020でのonEndEditは確定時以外にもキーボードからフォーカスを外した際に呼ばれるようになっている。
Unity2021からはonSubmitが用意されており、そちらは確定した場合のみ呼ばれるようになっている。
