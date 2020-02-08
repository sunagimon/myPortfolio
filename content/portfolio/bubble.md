---
title: "Bubble-Pixels"
type: portfolio
weight: 11
description : "Bubble-Pixels (2020/01/31)"
caption: 気泡を操作可能なインタラクティブ水中ディスプレイ
image: images/portfolio/bubble.png
creator: 宇川拓人, 小川剛史
release: January 31, 2020
media: Master Studies
---
# Movie
<iframe width = "100%" height = "315" src="https://www.youtube.com/embed/ZhwvcLrFlck" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---
# Tech
水中にある超撥水素材の表面に気泡が定着する原理を利用している. <br>
超撥水面の傾斜と空気の吹き込み量を調整することで気泡を水平方向に移動させる. <br>
気泡を配置し, 気泡を画素としたイメージを描画する. <br>

超撥水板の四隅にユニバーサルジョイントを介して上下方向に動くラックギアを接続している. <br>
ギヤードモータの回転を制御することでギアの位置が決まるラックピニオン機構を採用し, 板の傾斜を調整している. <br>
気泡の吹き込み/吸い込みは蠕動ポンプを使用している. <br>
ArduinoによりギヤードモータはPID制御, 蠕動ポンプはPWM制御をして動かしている. <br>

---
# Comment
水平方向に移動する気泡を画素としたディスプレイ. <br>
修士の研究テーマである. <br>
「水ディスプレイの特性」と「気泡の特性」を同時に発揮可能な新たなインタフェースの創出を目指して提案・設計した. <br>

研究室配属前から「新しいディスプレイを創りたい」という意気込みを語っていたが, それ通りの研究を行うことができた. <br>
先生に感謝です. <br>

---
# Link

<a href= https://www.ogawa-lab.org/ target=”_blank”>小川剛史</a>