---
title: "LIFE"
type: portfolio
weight: 13
description : "LIFE (2019/03/17)"
caption: PCをNPCにひっぱりぶつけ, PCへと変えるゲーム
image: images/portfolio/life.png
creator: 宇川拓人
release: March 17, 2019
media: unity1week_20190311

---
# Movie
<iframe width = "100%" height = "315" src="https://www.youtube.com/embed/er3s5WeyrrU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---
# Tech
Unityで制作し, WebGLでビルドしている. <br>

3D空間内でモン◯トのような引っ張り飛ばすアクションを実装するためにRayCastを使用し, Dragベクトルの計算をしている. <br>

```
if(Input.GetMouseButton(0))
{
    Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
    RaycastHit hit;
    if(Physics.Raycast(ray, out hit, Mathf.Infinity) && touch == true)
    {
        //ドラッグベクトル計算
        dragVec = transform.position - hit.point;
    }
}
```

PC → NPCへの切り替えはInstantiate関数を使い, クローンオブジェクトを生み出し, 自身をDestroyしている. <br>
コード上でも自身の死と新たな生の誕生を再現した. <br>

```
GameObject dragPlayer = Instantiate(DragPlayer, transform.position, transform.rotation) as GameObject;
Destroy(this.gameObject);
```
---
# Comment
unityroomの1週間ゲームジャムで制作. <br>
お題は「つながる」<br>

つながる「LIFE」<br>
PCをNPCに引っ張りぶつけるとPCは消え, NPCはPCになる. <br>
ビデオゲームアートという世界ではPlayerの意思は「生」であるらしい. <br> 
生き物が行う「生をつなぐ」という行為をゲームのキャラにやらせたみた. <br>

アリストテレスの目的論になぞらえて恵みを与えるために雨を降らせなるなど, 世界観やコンテクストにこだわった. <br>

直前にビデオゲームアート展(ICC | イン・ア・ゲームスケープ)に行った影響を強く受けている. <br>
哲学的自己満ゲーム. <br> 
レンダリングパスを上手く設定しないとPointLightが消えるなど, Unity → WebGLを闇の深さを感じられる作品でもあった. <br>

---
# Link
<a href= https://unityroom.com/games/life target=”_blank”>unityroom_LIFE</a> 