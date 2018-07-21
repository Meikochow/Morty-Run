var myGamePiece;
var myObstacle = [];
var myBackground;
var myDeaths;
var deaths=0;

function startGame(){
myGamePiece = new component(50, 60, "https://s20.postimg.cc/el22tbvel/morty_running.png", 100, 140, "image");
myBackground = new component(2000, 270, "https://s20.postimg.cc/ssrrhpkvh/background2.png", 0, 0,"background");
  // myBackground = new component(2000, 270, "grey", 0, 0);
myDeaths = new component("30px","monospace","black",10,40,"text");
myGameArea.start();

}
function restart(){
$("#startOver").css("display","none");
myObstacle = [];
myBackground = new component(2000, 270, "https://s20.postimg.cc/ssrrhpkvh/background2.png", 0, 0,"background");
  // myBackground = new component(2000, 270, "grey", 0, 0);
myGameArea.start();
}
var myGameArea = {
canvas : document.createElement("canvas"),
  
start : function(){
if(window.innerWidth < 400){this.canvas.width  = 300;}
  else
this.canvas.width = 600;
this.canvas.height = 200;
this.context = this.canvas.getContext("2d");
document.body.insertBefore(this.canvas,document.body.childNodes[0]);
this.frameNo = 0;
  //the the interval which runs the updateGameArea function (of updating the canvas every 20 miliseconds)
this.interval = setInterval(updateGameArea, 20);
window.addEventListener('keydown', function (e) {myGameArea.key = e.keyCode;})
window.addEventListener('keyup', function (e) {myGameArea.key = false;})
window.addEventListener('touchstart', function (e) {jump();})
},
clear : function(){this.context.clearRect(0, 0, this.canvas.width, this.canvas.height)},
stop:function(){clearInterval(this.interval)}
}

function component(width, height, color, x, y, type){
  this.type = type;
if (type == "image" || type == "background") {
        this.image = new Image();
        this.image.src = color;
  }
  this.width = width;
  this.height = height;
  this.speedX = 0;
  this.speedY = 0;
  this.x = x;
  this.y = y;
  this.update = function(){
  ctx = myGameArea.context;

if (type == "image"|| type == "background") {
  ctx.drawImage(this.image, this.x, this.y, this.width, this.height);
    if(type == "background"){ctx.drawImage(this.image, this.x + this.width, this.y, this.width, this.height);
    }
} else if(this.type == "text"){
   ctx.font = this.width + " " + this.height;
   ctx.fillStyle = color;
   ctx.fillText(this.text, this.x, this.y);
   }else {
  ctx.fillStyle = color;
  ctx.fillRect(this.x, this.y,this.width, this.height)
         } 
  }
this.newPos = function(){
  this.x += this.speedX;
  this.y += this.speedY;
if (this.type == "background") {
    if (this.x == -(this.width)) { this.x = 0;}
        }
  }
this.crashWith = function(otherobj){
  var myleft = this.x+30;
  var myright = this.x + (this.width)-10;
  var mytop = this.y;
  var mybottom = this.y + (this.height);
  var otherleft = otherobj.x;
  var otherright = otherobj.x + (otherobj.width);
  var othertop = otherobj.y+5;
  var otherbottom = otherobj.y + (otherobj.height)-20;
  var crash = true;
    if ((mybottom < othertop)||(mytop > otherbottom)||(myright<otherleft)||(myleft > otherright)){crash = false;}
    return crash;
}
}

function updateGameArea(){
var x,y;
for (var i = 0; i < myObstacle.length; i+=1){
  if(myGamePiece.crashWith(myObstacle[i])){   
    myGameArea.frameNo=0;
    this.x=this.x;
    this.y=this.y
    $("#startOver").css("display","inline");
    deaths++;
    myGameArea.stop();
    return;
  }
}
myGameArea.clear();
myBackground.speedX = -1;
myBackground.newPos();
myBackground.update();
myGameArea.frameNo+=1;
  //stage 1 - 3 mr.popy
    if(myGameArea.frameNo % 50 == 0 && myGameArea.frameNo<200){
      x = myGameArea.canvas.width;
      y = myGameArea.canvas.height-50;
      myObstacle.push(new component(20,50,"https://s20.postimg.cc/yw05a71jh/Mr_poopy_butthole-240x700.png",x, y,"image"))
    } 
  //stage 2 - jerry and popy X2
      if(myGameArea.frameNo>201&&myGameArea.frameNo % 100 == 0 && myGameArea.frameNo<500){
      x = myGameArea.canvas.width+100;
      y = myGameArea.canvas.height-80;
      myObstacle.push(new component(40,80,"https://s20.postimg.cc/sh597q37x/jerry_object_2_30.png",x, y,"image"))
      x = myGameArea.canvas.width;
      y = myGameArea.canvas.height-50;
      myObstacle.push(new component(20,50,"https://s20.postimg.cc/yw05a71jh/Mr_poopy_butthole-240x700.png",x, y,"image"))
    } 
  //stage 3
        if(myGameArea.frameNo>501 && myGameArea.frameNo % 100 == 0 && myGameArea.frameNo<800){
      x = myGameArea.canvas.width;
      y = myGameArea.canvas.height-80;
      myObstacle.push(new component(40,80,"https://s20.postimg.cc/sh597q37x/jerry_object_2_30.png",x, y,"image"))
      x = myGameArea.canvas.width+150;
      y = myGameArea.canvas.height-80;
      myObstacle.push(new component(40, 80, "https://s20.postimg.cc/9l8aw41rx/Morty_Pickle_Front.png", x, y, "image"))
    } 
  if(myGameArea.frameNo == 764){
      x = myGameArea.canvas.width;
      y = myGameArea.canvas.height-50;
      myObstacle.push(new component(20,50,"https://s20.postimg.cc/yw05a71jh/Mr_poopy_butthole-240x700.png",x, y,"image"))
    }
  //stage 4
        if(myGameArea.frameNo>800 && myGameArea.frameNo % 50 == 0 && myGameArea.frameNo<1050){
      x = myGameArea.canvas.width;
      y = myGameArea.canvas.height-80;
      myObstacle.push(new component(40, 80, "https://s20.postimg.cc/fytpom7il/plumbus.png", x, y, "image"))
    } 
  //stage 5 a pikle and a sun X3
    if(myGameArea.frameNo>1080&&myGameArea.frameNo % 110 == 0 && myGameArea.frameNo<1490){
      x = myGameArea.canvas.width;
      y = myGameArea.canvas.height-80;
      myObstacle.push(new component(40, 80, "https://s20.postimg.cc/9l8aw41rx/Morty_Pickle_Front.png", x, y, "image"))
      x = myGameArea.canvas.width+110;
      y = myGameArea.canvas.height-230;
      myObstacle.push(new component(100, 100, "https://s20.postimg.cc/rz1djnkx9/i_ANXs_Kh.png", x, y, "image"))
    } 
  //stage 6
      if(myGameArea.frameNo>1500 && myGameArea.frameNo % 100 == 0 && myGameArea.frameNo<2000){
      x = myGameArea.canvas.width;
      y = myGameArea.canvas.height-60;
      myObstacle.push(new component(50,60,"https://fanart.tv/detailpreview/fanart/tv/275274/characterart/rick-and-morty-589716f33bcf7.png",x, y,"image"))
      x = myGameArea.canvas.width+130;
      y = myGameArea.canvas.height-240;
      myObstacle.push(new component(100, 180, "https://s20.postimg.cc/48b6t4udp/rick.png", x, y, "image"))
    }
  //END
  if(myGameArea.frameNo == 2100){
      x = myGameArea.canvas.width;
      y = myGameArea.canvas.height-220;
      myObstacle.push(new component(180, 220, "https://s20.postimg.cc/6sbf11rwt/Untitled.png", x, y, "image"))
    } 
 //updating the obstacles positions
    for (var i = 0; i < myObstacle.length; i +=1){
      myObstacle[i].x -= 3;
      myObstacle[i].update();
     } 
//updating the death score
myDeaths.text = "Deaths:" + deaths;
myDeaths.update();
myGamePiece.newPos();
//helper condition maintaining the phisics of the GamePiece
if(myGamePiece.y>=141){ myGamePiece.speedY=0;myGamePiece.y=140;}
myGamePiece.update(); 
if(myGameArea.key && myGameArea.key == 32) {jump();} 
}
//the physics of the jump
function jump(){
if(myGamePiece.speedY == 0){ myGamePiece.speedY=-16; setTimeout(()=>myGamePiece.speedY=7 ,220)}
}
