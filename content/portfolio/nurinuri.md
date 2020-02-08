---
title: "ぬりぬりタウン"
type: portfolio
weight: 12
description : "ぬりぬりタウン (2019/05/5)"
caption: 紙に描かれた物に色を塗り, スクリーン内の街へ出現させるシステム
image: images/portfolio/nuri.png
creator: 小沢健悟, 宇川拓人
release: May 5, 2019
media: フレンドリープラザ墨田児童会館「小学生こどもの日スペシャル」

---
# Movie
<iframe width = "100%" height = "315" src="https://www.youtube.com/embed/vM6VySQnM2E" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---
# Tech
Unityで実装している.<br>
オブジェクトはBlenderでモデリングし, 射影のUVマップを塗り絵の台紙としている. <br>
塗られた絵はOpenCV for Unityの矩形検出アルゴリズムとArUcoを用いてマーカーを区別しスキャンされ, オブジェクトへとラッピングしている. <br>

3画面に投影するため, 斜めの投影行列を考慮した3台のカメラをUnity内に配置している. <br>

---
# Comment
紙に描かれた人や動物, 建物, UFOなどのオブジェクトを好きな色に塗り, スクリーン内の街へと映し出す. <br>
映し出されたオブジェクトはアニメーションしながら動き, 一定時間経つとしぼんで消える. <br>

技術的にはよく見るもの. <br>
今回は児童会館の依頼で小学生や幼児向けのため, シンプルかつ子ども向けのデザインにこだわった. <br>

プロジェクターが光るだけで喜ぶ子がいるなど, 微笑ましかった. <br>

---
# Link
<a href= https://fukushi.unchusha.com/sumida/otayori/20190505shogakusei.pdf target=”_blank”>フレンドリープラザ墨田児童会館「小学生こどもの日スペシャル」</a> 

<a href= https://zawazawagh.github.io/zawazawa/ target=”_blank”>小沢健悟</a>