# DoorPortal

# 概要
どこで◯ドアシステムの動かし方です．

# 動作環境
- CPU Intel(R) Core(TM) i9-9900K CPU @ 3.60GHz
- GPU RTX2080Ti

- Varjo Base 3.7.1.11
- Unity 2021.3.6.f1
- SteamVR 1.24.6

# 簡単な動作方法

care-fsの/archive/2022/Takara_Fujisawa/program_and_dataにUnityのプロジェクトを置いてます．

1. VIVE Tracker（ドアの左上に取り付ける） と VarjoBase起動（これでXR-3，SteamVRは自動で起動する）
1. Unityのプロジェクトを立ち上げて Assets/!Scenes/Demo を再生．（XR-3のカメラでドアのARマーカを捉えるように気をつける．）

# スクリプトと設定
## Marker.cs

物理ドアと3Dモデルの位置合わせを行っているスクリプトです．（XRRigにアタッチしています）

位置合わせとしては，71-81行目辺りでゴリ押しで行っています．ズレていた場合ここの数字を調整してください．

![marker](https://user-images.githubusercontent.com/95071487/223747943-913085b6-90e0-4382-ac78-e3ee62e6211f.png)

基本的には画像の通りに設定してもらえればいいです．Tracked ObjectのIDの部分はマーカのIDです．今回は300のものを使用しています．

詳しくはhttps://developer.varjo.com/docs/unity-xr-sdk/varjo-markers-with-varjo-xr-plugin を参照してください．


## ChangeLayer.cs

ステンシルバッファを使うため，オブジェクトごとにレイヤーを分けています．そのレイヤーの切り替えを行うスクリプトです．（XRRigの孫オブジェクトのMainCameraにアタッチしています）

カメラに当たり判定を付けてポータルに当たるとレイヤーを切り替えるようにしています．

![changelayer](https://user-images.githubusercontent.com/95071487/223747828-d6847f2c-536d-4ec8-8185-87809f775621.png)

こちらも設定はほぼそのままで大丈夫です．

BlackはBIGというビデオシースルーARを行うための黒い箱を割り当ててください．

CGの部分は任意のものに変えてもらうことができます．他のCGのワールドでもいいですし，球に360度画像を貼り付けたものでも使えます．

360度画像を使う際はOctahedron Sphere 5 D1 というオブジェクトがあるのでそちらを有効にして使用してもらえます．また，割り当てているマテリアルを使うと簡単に任意の360度画像に変更できます．

### レイヤーの設定について
レイヤーの設定が少しややこしいです．

主に使用するレイヤーは「3：Stencil 1」，「6：Stencil 2」，「0：Default」の3つです．

「3：Stencil 1」 ： 行き先になるオブジェクト（CGオブジェクトや球）に割り当てます

「6：Stencil 2」 ： ビデオシースルーAR用の黒い箱に割り当てます．

「0：Default」   ： その他（ドアの3Dモデルの木枠等）は全てこのレイヤーで大丈夫です．


## GetRot.cs

トラッカの回転角度を取得してドアに反映するためのスクリプトです．

基本的にはイジらないでいいです．

## VIVEトラッカの設定
https://qiita.com/RyoyaHase/items/015bbb8671833ef8e740 を参考にトラッカの設定を行ってください．

以下の画像を参考に設定をしてください．Input Sourceは設定時の部位にしてください．
![tracker](https://user-images.githubusercontent.com/95071487/223757270-c72ff615-3e2d-43c4-8b0c-dfa53164bf13.png)


# トラブルシューティング
ここからは上手く動かないときの対処法です．

## 急に位置合わせが合わない
以前まで合っていたのに急に合わなくなった場合には次の項目を確認してみてください．
- ベースステーションの配置は変わっていないか
- ベースステーションの電源が切れていたりしないか

上記に問題がない場合
- VarjoBaseの再起動（Supportの欄にRestartのボタンがあります）
- SteamVRの再起動
- Unityプロジェクトの再起動
- PCの再起動

## VIVEトラッカの回転角が反映されていない（ドアがこちら側に90度開いた状態）
以下の2つの理由が考えられます．
- VIVEトラッカの電源が切れている（結構ある）
- VIVEトラッカの設定が反映されていない
  - Steamの再ログインをしてみる（そもそもログインされていないパターン）
    -以下の画像のSteamVR Input から 開いたウィンドウの右下Open Binding UI
 ![image](https://user-images.githubusercontent.com/95071487/223764438-d2abb24c-8148-4748-83df-644f2d3e5d4e.png)
    - 以下の画像が正しい状態
![true](https://user-images.githubusercontent.com/95071487/223773750-bfa38735-873a-4498-912b-91343449bc3f.png)
    - 他の状態（Steamにログインしていないとか，バインディングが設定されていない）は上手く動作しない．
