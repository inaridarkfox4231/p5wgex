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

具体的には「**_orbitControlのダブルクリックによるリセット_**」ですね  
仕様作るの面倒だったんでやめたやつです。カメラのslerpの応用で出来るので、  
いずれ優秀なcontributorが実装するでしょう。健闘を祈ります。  
  
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
やっぱり自作ライブラリなんか作るもんじゃないですね...何が利点なのかさっぱりわかんなくなった  
でも上げないと気づけなかったかも？  
気付けて良かったです。

```js
let _node;
const _timer = new p5wgex.Timer();

function setup() {
  createCanvas(400, 400, WEBGL);
  _node = new p5wgex.RenderNode(this._renderer.GL);
  _node.registFigure("triangle", [
    {size:1, name:"aIndex", data:[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]}
  ]);
  _node.registIBO("triangleIBO", {data:[
    0,1,2, 0,2,3, 0,3,4, 0,4,5, 0,5,6,
    0,6,7, 0,7,8, 0,8,9, 0,9,10, 0,10,1
  ]});
  _node.registPainter("shader",
  `#version 300 es
   #define TAU 6.28318
   in float aIndex;
   uniform float uTime;
   out vec2 vPosition;
   void main(){
     float r = (mod(aIndex, 2.0) == 0.0 ? 0.5 : 1.0);
     vec2 p;
     if(aIndex == 0.0){
       p = vec2(0.0);
     }else{
       p = vec2(r*cos(TAU*aIndex/10.0), r*sin(TAU*aIndex/10.0));
     }
     float t = fract(uTime);
     t = t * t * (3.0-2.0*t);
     float angle = TAU*t/5.0;
     p *= mat2(cos(angle), -sin(angle), sin(angle), cos(angle));
     gl_Position = vec4(p, 1.0, 1.0);
     vPosition = p;
   }
  `,
  `#version 300 es
   precision highp float;
   in vec2 vPosition;
   out vec4 fragColor;
   void main(){
     float l = length(vPosition);
     vec3 color = 1.0-vec3(l*0.4, l*0.8, l*1.2);
     fragColor = vec4(color, 1.0);
   }
  `);
  _timer.initialize("time0");
}
function draw(){
  const t = _timer.getElapsed("time0");
  _node.clear(0)
       .use("shader", "triangle")
       .bindIBO("triangleIBO")
       .setUniform("uTime", t)
       .drawElements("triangles")
       .unbind()
       .flush();
}
```

https://github.com/user-attachments/assets/24acf69e-d1e2-4aa6-8836-d27aa2a59bc2

[インスタンシングの作例](https://openprocessing.org/sketch/2417016)
```javascript
// インスタンシング
let _node;
const mu = p5wgex.meshUtil;
let LS;
let pfc;

const INSTANCE_COUNT = 3000;

function setup() {
  createCanvas(window.innerWidth, window.innerHeight, WEBGL);
  pixelDensity(1);

  _node = new p5wgex.RenderNode(this._renderer.GL);
  const sp = mu.sphere({radius:0.06, detail:{x:24,y:24}});
  const instancePositions = [];
  const instanceColors = [];
  for(let i=0; i<INSTANCE_COUNT; i++){
    const v = getRandom3D().mult(6);
    instancePositions.push(v.x,v.y,v.z);
    instanceColors.push(...p5wgex.coulour("hsv", 0.5+Math.random()*0.15, 0.4+0.6*Math.random(), 1.0));
  }
  mu.regist(_node, sp, "sphere", {otherAttrs:[
    {name:"aInstancePosition", size:3, data:instancePositions, divisor:1},
    {name:"aInstanceColor", size:4, data:instanceColors, divisor:1}
  ]});
  const cam = new p5wgex.PerspectiveCamera({
    w:width, h:height, eye:[20,0,0], top:[0,0,1]
  });
  const CC = new p5wgex.CameraController(this.canvas, {dblclick:true});
  CC.setParam({rotationMode:"free"});
  CC.registCamera("cam", cam);
  LS = new p5wgex.StandardLightingSystem(_node);
  LS.initialize();
  LS.addAttr("vec3", "aInstancePosition");
  LS.addAttr("vec4", "aInstanceColor");
  LS.addVarying("vec4", "vInstanceColor");
  LS.addCode(`
    position += aInstancePosition;
    vInstanceColor = aInstanceColor;
  `, "preProcess", "vs");
  LS.addCode(`
    color = vInstanceColor;
  `, "preProcess", "fs");
  LS.registPainter("spheres");
  LS.setController(CC);

  pfc = new p5wgex.PerformanceChecker(this.canvas);
}

function draw() {
  LS.update();
  _node.clear(0);
  _node.use("spheres", "sphere");
  LS.setLight({
    useSpecular:true
  });
  LS.setDirectionalLight({
    count:1,
    direction:[0,0,-1]
  });
  LS.setLightingUniforms({cameraBase:true});
  LS.setTransform().setMatrixUniforms();

  _node.bindIBO("sphereIBO").drawElementsInstanced("triangles", {instanceCount:INSTANCE_COUNT});
  _node.unbind().flush();

  pfc.update();
}


function rdm(){
  return Math.random();
}

function getRandom3D(){
  const u = rdm();
  const t = Math.acos(1-2*u);
  const s = Math.PI*2*rdm();
  const z = Math.cos(t);
  const x = Math.sin(t)*Math.cos(s);
  const y = Math.sin(t)*Math.sin(s);
  return new p5wgex.Vec3(x,y,z);
}
```

うん  
glsl書くのが楽しい人でないと使えないやこれ  
ごめんなさい  

p5でできることばっかやっても仕方ないので、こっちでないとできないインスタンシングの作例を追加しました。  
やり方は簡単で、attrの指定時にdivisorを用意するだけです。あとドローコールの際にインスタンスカウントを指示するのとそれ用の関数が要ります。あとは内部仕様がよろしくやってくれます。

renderGradation  
```js
function setup() {
  createCanvas(400, 400, WEBGL);

  const _node = new p5wgex.RenderNode(this._renderer.GL);
  _node.renderGradation({
    from:{x:0,y:0}, to:{x:1,y:1},stops:[0,0.49,0.5,0.51,1],
    colors:["black","navy","white","navy","black"]
  });
  _node.renderGradation({
    from:{x:0,y:1}, to:{x:1,y:0},stops:[0,0.49,0.5,0.51,1],
    colors:["black","teal","white","teal","black"]
  },{blend:"screen"});
}

```
実行結果  

![hhh](https://github.com/user-attachments/assets/3c366ab9-d843-44bb-b13b-4eace9a3464a)  

必要になったのでangleToとangleBetweenを導入しました。  
語感からangleBetweenで絶対値、angleToで符号付きとしました。  
まあ符号付きの値もそれなりに役に立つので。  
関数を分けるのは可読性のためという側面が大きいです。  
保守性は問題にならないでしょう。こんな短いんだから。  
```js
  angleBetween(v){
    // vとのなす角（絶対値）
    const crossMag = this.copy().cross(v).mag();
    const dotValue = this.dot(v);
    const theta = Math.atan2(crossMag, dotValue);
    return theta;
  }
  angleTo(v, a=0, b=0, c=1){
    // axisの先っちょから見た場合のvとのなす角（符号付き）
    // PI以内ですね。PIを超えると逆方向です。
    // 便宜上signが負でない場合はすべて1とします。
    const axis = _getValidation(a,b,c);
    const sign = Math.sign(this.copy().cross(v).dot(axis)); // ベクトル三重積
    const theta = this.angleBetween(v);
    return (sign < 0 ? sign : 1) * theta;
  }
```

[VTFとインスタンシングを組み合わせるコード](https://openprocessing.org/sketch/2417567)

