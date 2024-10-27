# p5wgex
自作webglライブラリ  
code:[https://inaridarkfox4231.github.io/p5wgex/p5wgex.js](https://inaridarkfox4231.github.io/p5wgex/p5wgex.js)  
このアドレスを放り込むとp5wgexでアクセスできる  
内部でfoxIAも定義してるのでfoxIAも使える  
おわり  
画像ってどうやるの？こうするの  
![Altテキスト](https://inaridarkfox4231.github.io/assets/backgrounds/ocean0.JPG)  
すげぇ！  
何書けばいいの？  
えーと、作成動機  
webglの勉強用です。サンプルを作って動かそうと思ってて。でもp5のwebglはできることが少ないし、  
threeは敷居が高かったので自作することにしました。  
参考にしたのはh_doxasさんのありがたいサイトと、pavelさんの流体シミュレーションのコードや、その辺ですね。  
あとp5をハッキングして得た知識などです（webglをどのように扱ってるのか気になったので）。  
p5と名前がついていますがp5の関数を使っていません。なのでp5を読み込まなくても普通に動きます  
（threeの関数も使っていません...というかそんなライブラリ無いと思うけど）。  
せいぜいテクスチャ生成時にp5のグラフィックスオブジェクトを受け取れるくらいですね...これがすごく便利なので。  
しかしその判定をするのにp5の読み込みを前提としていないので、つまりそういうことです。  
h_doxasさんのすごく参考になるサイト：[wgld.org](https://wgld.org/)  
pavelさんの流体シミュレーション：[WebGL Fluid Simulation](https://github.com/PavelDoGreat/WebGL-Fluid-Simulation)  
カメラはこれをちょっとだけ参考にしました：[EasyCam](https://github.com/freshfork/p5.EasyCam)  
ほんとにちょっとだけですね...  
できること？？？
通常のwebgl描画、インスタンシング（p5のおもちゃみたいのじゃなくてちゃんとしたもの）、  
トランスフォームフィードバック、射影テクスチャマッピング、フロートテクスチャ、ポイントスプライト、  
[影などなど](https://openprocessing.org/sketch/2415785)、そこら辺ですね。  
すんなりできるとは言わないです。あんまりインスタントしてしまうと  
柔軟性がなくなるので、きちんと書かないといけないです。  
ただある程度楽にはなっています。あとあれですね、glslが書けない人には扱えないです（ごめんなさい）。  
使い方？使用方法ね。そのうちね。  
サンプルを作ることに終始している感じなので作品とか本格的に作るならThree.jsを使った方がいいです。  
一番遊んでるのはcameraControllerの辺り（p5やThreeでorbitControlと呼ばれているもの）...  
ここではp5の仕様変更で出来なかったことを存分にやってます（たのしい！！）  
あとベクトルは3次元のVec3というのがあります。  
軸周りの回転ができます。3次元なので出来て当たり前なんですがp5が導入してないので...  

## 使用例  
RGB三角形
```javascript
function setup() {
  createCanvas(400, 400, WEBGL);
  const _node = new p5wgex.RenderNode(this._renderer.GL);
  _node.registFigure("triangle", [
    {size:1, name:"aIndex", data:[0, 1, 2]}
  ]);
  _node.registPainter("shader",
  `#version 300 es
   #define TAU 6.28318
   in float aIndex;
   out vec3 vColor;
   void main(){
     vColor = vec3((aIndex==0.0), (aIndex==1.0), (aIndex==2.0));
     gl_Position = vec4(sin(TAU*aIndex/3.0), cos(TAU*aIndex/3.0), 1.0, 1.0);
   }
  `,
  `#version 300 es
   precision highp float;
   in vec3 vColor;
   out vec4 fragColor;
   void main(){
     fragColor = vec4(vColor, 1.0);
   }
  `);
  _node.clear(0)
       .use("shader", "triangle")
       .drawArrays("triangles")
       .unbind()
       .flush();
}
```
webgl2のレンダリングコンテキストでRenderNodeを初期化して、これを使っていろいろやる感じです。  
registFigureでattributeを一通り用意することができます。このレンダリングでは3つのindexを使って、  
いつものRGB三角形を描画していますね。これだと恩恵があんま感じられないですね...p5ならbegin～endの  
shapeで3つ頂点を決めてそれぞれ頂点色を配してどーん、それで終わりですね。  
というわけでp5の方が便利だと思います。このライブラリの紹介は以上です。  
ここまでお読みいただいてありがとうございました。  

[完]

![qdd2d2222df](https://github.com/user-attachments/assets/bc2989f7-b18e-4392-b7ef-a2fb587bfbbd)
