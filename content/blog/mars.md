---
title: "仮想実環境ARシミュレーションしてみた話"
date: 2020-12-19T00:00:15+09:00
description : "2020,12,19"
type: post
---

この記事はアドベントカレンダー <a href="https://adventar.org/calendars/5825" target="_blank">ほぼ厚木の民</a> の19日目記事です。

#### 概要  
iPhone 12 Pro (LiDAR) を使い空間スキャンし得られた現実環境3DモデルとUnity MARSでお手軽に仮想実環境ARシミュレーションをしてみた. <br>

#### ARのシミュレーションはめんどう  
現在, シンプルなARアプリを開発するのはARKitやARCoreなどのフレームワークが充実しているため容易と言えるだろう. <br>

一方, 少しめんどくさい工程もある. <br>
それはARのシミュレーションだ. <br>
ARアプリ開発では実機にビルドし, 現実世界へカメラを向けて所望な動作(映像の重畳, マーカー認識etc...)確認を行う. <br>

仮にUnityでiOS向けのARアプリを開発するのであればデバッグのたびにUnityからXcodeプロジェクトを書き出し, XcodeからiOS端末へアプリをビルドしカメラを使い動作確認する. <br>
さらにアプリをとある会場で使うとなれば現地に赴き, そこでの動作確認が必要である. <br>
これはめんどうだ. <br>

#### Unity MARSってなに  
<a href="https://unity.com/products/unity-mars" target="_blank">Unity MARS</a>は2020年6月にリリースされた新しいAR開発フレームワークだ. <br>
ARKitやARCoreなどをさらに抽象化し誰でも使えるようにしている. <br>

<img src="https://sunagimon.github.io/images/blog/mars/mars_system.jpg" width="50%" height="50%"><br>

Unity MARS最大の特徴(私が思う)は仮想環境でのシミュレーションが可能という点である. <br>
現実環境の代わりにUnityエディタ内で再現された仮想環境にて動作確認できる. <br>
室内, 公園, 工場などスケールやシチュエーションが異なる20種類ほどの環境が用意されている. <br>


#### LiDARが身近に  
LiDAR : Light Detection and Ranging はレーダー光を利用し対象との距離を測るセンシング技術である. <br>
最近ではハイエンドなスマホに採用されていることが多い. <br>
先日発売されたiPhone 12 Proにもdirect Time of Flight (dToF) 型のLiDARセンサーが搭載されている. <br>
反射した光が戻ってくるまでの時間から距離計算を行う方式であり, 外光に強く長距離での計測も可能である. <br>

これによりセンシングで得られた情報からメッシュ生成, テクスチャ生成し3Dモデルを作り出す. <br>
スマホ1台で手軽に現実環境を3Dモデル化することができるのだ. <br>


#### 目的  
導入が長くなったが, やりたいことはスマホで現実環境をスキャンして3Dモデル化し, それを仮想実環境としてUnity MARSでARシミュレーションするだ. <br>
これにより任意の現実環境でのARシミュレーションがいつでもどこでも簡単に行える. <br>

#### 仮想実環境ARシミュレーション準備編  
まずは現実環境の3Dモデル化をする. <br>
iPhone 12 Proで<a href="https://apps.apple.com/jp/app/3d-scanner-app/id1419913995" target="_blank">3D Scanner App</a>を使用しスキャン, 3Dモデルの生成を行った. <br>
意外なことにApple純正のLiDARアプリはない(ないですよね？). <br>
今回は評判が良い3rd party製の無料アプリを使ってみた. <br>

自室のスキャン中の様子である. <br>
スキャンされた箇所は紫に塗り潰されていくのでわかりやすい. <br>
スキャン終了後はColorizeを選択することでテクスチャが生成される. <br>
メッシュ, テクスチャ生成を含めても3分ほどで3Dモデルが完成した. <br>
Photogrammetryではこう簡単にはいかない. <br>
さすがはLiDARである. <br>

<img src="https://sunagimon.github.io/images/blog/mars/scan.jpg" width="40%" height="40%"><br>

生成した3Dモデルを下図に示す. <br>
自室をグリグリ動かし見て回れるのは面白い. <br>
クオリティに関してだがPhotogrammetryと比較するとやはり粗い. <br>
dToFではなく, 受信光の位相差を測定するiToF方式であればもっと精度は上がるのか気になるところである. <br>
これは私の問題であるが, 家具のほとんどが光を反射しにくい黒を基調しているのでテーブルの足などはうまくスキャンされなかった...<br>
エクスポートはShareを押し, Texturedオプションを忘れないようにOBJ形式を選択する. <br>

<img src="https://sunagimon.github.io/images/blog/mars/scan_room1.jpg" width="30%" height="30%"><br>

<img src="https://sunagimon.github.io/images/blog/mars/scan_room2.jpg" width="30%" height="30%"><br>

続いてUnity MARS側の準備へ移る. <br>
詳しい導入手順や説明は<a href="https://youtu.be/9h5TUiIf-Ks" target="_blank">Unity Japan公式のセミナー</a>参照. <br>
あと, <a href="https://docs.unity3d.com/Packages/com.unity.mars@1.2/manual/index.html" target="_blank">公式ドキュメント</a>が充実しているのでここを見ればだいたいのことが書いてあります. ( ~~私のこの記事いらない~~ )<br>

Unity 2019.3.15f1 (2019.3.xならOK) で新規プロジェクトを作成し, Unity MARSを導入する. <br>
今回実装するARアプリは平面を検出し3Dオブジェクトを出現させるシンプルなものである. <br>
Unity MARSでは驚くほど簡単な手順で実装できる. <br>

まず, MARS PanelからHorizontal Planeを選択する. <br>
HierarchyにHorizontal PlaneとMARS Sessionが追加されるので, Horizontal Planeの子オブジェクトとして出現させたい3Dオブジェクトを追加する. <br>
ここではユニティちゃんを追加した. <br>

<img src="https://sunagimon.github.io/images/blog/mars/mars_panel.jpg" width="50%" height="50%"><br>

<img src="https://sunagimon.github.io/images/blog/mars/unity_chan.jpg" width="50%" height="50%"><br>

以上で完成である. <br>
これをビルドすれば平面を検出しユニティちゃんを出現させるARアプリが実機で動作する. <br>
簡単すぎる. すごいぞMARS. <br>


今回は仮想実環境でのシミュレーションがメインなのでまだ続く. <br>

3D Scanner Appから出力されたファイルを追加する. <br>
おそらく以下のような構成になっている. <br>

<img src="https://sunagimon.github.io/images/blog/mars/texture_object.jpg" width="60%" height="60%"><br>

textured_outputをシーンへ追加する. <br>
このままではせっかくつけた色が見えないのでShaderをUnlit/Textureへ変える. <br>
これでスキャンした3DモデルがしっかりUnity上で確認できる. <br>

<img src="https://sunagimon.github.io/images/blog/mars/import_myroom.jpg" width="80%" height="80%"><br>

追加した3DモデルでUnity MARSのARシミュレーションが行えるよう準備を進める. <br>
Project viewを右クリックし, Create -> MARS -> Simulated Environment Prefabを選択する. <br>
Simulated Environment Prefabをシーンに追加し3Dモデル(textured_object)を子オブジェクトに加える. <br>
3Dモデルにシミュレーションに必要なScriptだけを追加するのでも問題ないが, このやり方が一番手っ取り早いと思われる. <br>

<img src="https://sunagimon.github.io/images/blog/mars/simulated_object.jpg" width="30%" height="30%"><br>

Simulated Environment Prefabの名前を適当に変更し(MyRoomとした), 再びPrefab化する. <br>
シーン上のMyRoomは必要ないので消しても問題ない. <br>
MyRoom~Prefabをinspectorで開きlabelを「Environment」とする. <br>
これによりUnity MARSのシミュレーション用の仮想環境一覧へMyRoomが登録される. <br>

<img src="https://sunagimon.github.io/images/blog/mars/prefab_label.jpg" width="50%" height="50%"><br>

続いて3Dモデル(MyRoom)へ床と壁などの平面を追加する作業を行う. <br>

新規でシーンを作成する(この作業でのみ使用). <br>
WindowからSimulation Viewという見慣れないものを追加する. <br>
これはUnity MARS特有のもので仮想環境でのシミュレーションを確認する画面となる. <br>
仮想環境内の仮想デバイスの画面を表示するDevice Viewも追加する. <br>

Simulation Viewを開く. <br>
デフォルトでは予め用意されている仮想環境が表示されていると思われる. <br>
仮想環境が選択できるタブをクリックし, 先ほど追加したMyRoomを選ぶ. <br>
これでSimulation ViewにMyRoomが表示される. <br>

<img src="https://sunagimon.github.io/images/blog/mars/select_myroom.jpg" width="50%" height="50%"><br>

MARS PanelからPlane Visualizerを追加し, MARS Panel内のHierarchyから矢印をクリックする. <br>
Device Viewを開き, Device View内の再生ボタンをクリックする. <br>

<img src="https://sunagimon.github.io/images/blog/mars/plane_setting.jpg" width="50%" height="50%"><br>

仮想環境内を動くことができるので動き, 見渡すことで平面を検出していく. <br>
現実環境で平面検出をするのと同じ要領でできる. <br>
見つけた平面はマークされるのでわかりやすい. <br>
MARS Environment SettingコンポーネントのSave Planes From Simulationをクリックする. <br>
これで準備は整った. <br>

<img src="https://sunagimon.github.io/images/blog/mars/plane_set.jpg" width="80%" height="80%"><br>


#### 仮想実環境ARシミュレーション実践編  
元のシーンへ戻る. <br>
現在このような状態となっているはずだ. <br>
SceneやGame Viewからは3Dモデルを確認することはできないが問題ない. <br>
UnityとUnity MARSは別扱いらしい. <br>
これらを繋げているのがMARS Panelから何かを追加すると自動でついてくるMARS Sessionである. <br>

<img src="https://sunagimon.github.io/images/blog/mars/pre_complete.jpg" width="80%" height="80%"><br>

実行してみる. <br>
しっかりできていた. <br>
スキャンし得られた仮想実環境の自室を動き周り, 平面を見つけてユニティちゃんを出現させることができた. <br>

<img src="https://sunagimon.github.io/images/blog/mars/complete.jpg" width="90%" height="90%"><br>

ちなみに実機にビルドして現実環境でアプリの動作確認をしてみた様子がこちらである. <br>
シミュレーションと同じ場所でユニティちゃんを出してみた. <br>

<img src="https://sunagimon.github.io/images/blog/mars/build_ver.jpg" width="70%" height="70%"><br>

うーん, シミュレーション通り! <br>


#### 後書き  
Unity MARSが便利すぎた. <br>
ただのMARS紹介になってしまった. <br>
こんなに便利で面白いのに使ってる人あまり見ないな...<br>

久しぶりにブログ書いたら面白かったのでもっと書いていこうかな...<br>
