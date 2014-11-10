
## 用pixi写的简单的小游戏,做为pixi入门。
=======================================================

pixi 使用非常简单，只需<script src="./js/pixi.js"></script>引入即可. 

pixi 需两个父级元素stage 和 renderer，stage设置舞台背景属性，renderer则是负责渲染的。

`document.body.appendChild(renderer.view);`必不可少。renderer.view即canvas 

了解pixi大概使用情况，那么就开始做游戏吧 


首先加载图片资源，pixi中有加载图片的loader，加载图片完成后再做操作，可避免一些图片还没加载完成游戏已开始的一些问题
在图片加载时我们可以把一些文本信息添加到舞台上，比如积分和时间，设置好他们的位置 

      //添加积分
       var scoreBoard = new PIXI.DisplayObjectContainer();
       var scoreText = new PIXI.Text(score,{font: "18px Arial bold", align: "center",fill:"#fff"});
      scoreText.position.x = 10;
      scoreText.position.y = 5;
      scoreBoard.addChild(scoreText);
      //添加计时
      var timeText = new PIXI.Text(time,{font: "18px Arial bold", align: "center",fill:"#fff"});
      timeText.position.x = 80;
      timeText.position.y = 5;
      scoreBoard.addChild(timeText);
      stage.addChild(scoreBoard); 


图片加载完成后开始图片渲染，渲染前先清空之前图片，按照难度即每个方向上图片个数，图片越多，则单个图片越小，并随机产生一个真图片的坐标，在真的图片上设置监听事件，在用户点击了真的图片之后，进行积分累加以及难度升级  

      function initimg(){
        //随机生成第几个是真图
            var m=parseInt(Math.random()*count[0]*count[1]);
            if(container.children.length){
              container.removeChildren();
            }      
            var n=0;
            for (var i = 0; i < count[0]; i++) {
                for (var j = 0; j < count[1]; j++) {
                //Sprite 是渲染图片的一个类，也可对Sprite进行位置大小设置,也可对其进行事件监听
                    var sprite=null;
                    if(m==n){   
                      sprite=new PIXI.Sprite(fantrue);
                      sprite.interactive = true;
                      sprite.mousedown=sprite.touchstart=function(){
                        change();
                      };
                    }else{
                      sprite=new PIXI.Sprite(fanfalse);
                    }
                    sprite.width=radius/count[0];
                    sprite.height=radius/count[0];
                    sprite.anchor.x=0;//anchor是sprite的计算点  0,0是图片左上角，1,1是图片右下角，0.5,0.5是图片中心点
                    sprite.anchor.y=0;
                    sprite.position.x = j*sprite.width;
                    sprite.position.y = i*sprite.height;
                    container.addChild(sprite);
                    n++;
                };
            };
      } 

难度升级主要是增加图片数量 

最后最主要的方法 

      function animate(){
      if(gameOver){
        return;
      }
        renderer.render(stage);
        requestAnimFrame(animate);
      }
      requestAnimFrame(animate); 
  
大家可以看到这是一个递归，而且当gameOver是true的时候是无限递归，requestAnimFrame是当当前帧绘制完成调用，也就是不断的绘制这个页面，使这个游戏页面动起来，另外需要注意的是pixi加载图片需要在有服务器的环境下，即你需要搭一个服务器，把它放进去
