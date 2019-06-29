# canvas
关于canvas的一些知识
[转载一篇canvas的详细介绍](https://blog.csdn.net/u012468376/article/details/73350998)
### 特点
1. 苹果公司推出
2. HTML5新增
3. 由HTML代码配合高度和宽度属性而定义出的可绘制区域
4. JavaScript代码可以访问该区域，类似于其他通用的二维API，通过一套完整的绘图函数来动态生成图形。
5. 可用来制作简单照片和动画，甚至可以实时视频处理和渲染
### canvas元素标签
1. 只有两个可选属性即width和height属性，不设置则默认宽300px，高150px。
2. 建议直接写在HTML中如`<canvas id="tutorial" width="300" height="300"></canvas>`,不建议再CSS中定义宽高
3. 结束标签`</canvas>`不可少
### 一个简单的小例子
```
<html>
<head>
    <title>Canvas tutorial</title>
    <style type="text/css">
        canvas {
            border: 1px solid black;
        }
    </style>
</head>
<canvas id="tutorial" width="300" height="300"></canvas>
</body>
<script type="text/javascript">
    function draw(){
        var canvas = document.getElementById('tutorial');
        if(!canvas.getContext) return;//检测浏览器支持性
        var ctx = canvas.getContext("2d");//使用2D渲染上下文
        ctx.fillStyle = "rgb(200,0,0)";//选取填充颜色
      	//绘制矩形
        ctx.fillRect (10, 10, 55, 50);//确定矩形初始位置和宽高

        ctx.fillStyle = "rgba(0, 0, 200, 0.5)";//选取填充颜色
        ctx.fillRect (30, 30, 55, 50);//确定矩形初始位置和宽高
    }
    draw();
</script>
</html>
```
### 三种方法绘制矩形
1. `fillRect(x, y, width, height)`//绘制一个填充的矩形
2. `strokeRect(x, y, width, height)`//绘制一个矩形的边框
3. `clearRect(x, y, widh, height)`//清除指定的矩形区域，然后这块区域会变的完全透明。
### 绘制路径
1. 新建一条路径`beginPath()`
- 路径一旦创建成功，图形绘制命令被指向到路径上生成路径
2. 创建路径起始点`moveTo(x, y)`
- 把画笔移动到指定的坐标(x, y)。相当于设置路径的起始点坐标。
3. 把路径封闭`closePath()`
- 闭合路径之后，图形绘制命令又重新指向到上下文中
4. 调用绘制方法去绘制出路径`stroke()`
- `stroke()`通过线条来绘制图形轮廓
- `fill()`通过填充路径的内容区域生成实心的图形
5. 一旦路径生成，通过描边或填充路径区域来渲染图形。
### 绘制路径的一些小例子
```
//绘制线段
function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    var ctx = canvas.getContext("2d");
    ctx.beginPath(); //新建一条path
    ctx.moveTo(50, 50); //把画笔移动到指定的坐标
    ctx.lineTo(200, 50);  //绘制一条从当前位置到指定坐标(200, 50)的直线.
    //闭合路径。会拉一条从当前点到path起始点的直线。如果当前点与起始点重合，则什么都不做
    ctx.closePath();
    ctx.stroke(); //绘制路径。
}
draw();
//绘制三角形
function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    var ctx = canvas.getContext("2d");
    ctx.beginPath();
    ctx.moveTo(50, 50);
    ctx.lineTo(200, 50);
    ctx.lineTo(200, 200);
  	ctx.closePath(); //虽然我们只绘制了两条线段，但是closePath会closePath，仍然是一个3角形
    ctx.stroke(); //描边。stroke不会自动closePath()
}
draw();
//填充三角形
function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    var ctx = canvas.getContext("2d");
    ctx.beginPath();
    ctx.moveTo(50, 50);
    ctx.lineTo(200, 50);
    ctx.lineTo(200, 200);
   
    ctx.fill(); //填充闭合区域。如果path没有闭合，则fill()会自动闭合路径。
}
draw();
//除了这些简单的图形，canvas还可以绘制圆弧和贝塞尔曲线
```
### 给图形上色需要的两个重要属性
1. fillStyle 设置图形的填充颜色默认情况下为黑色
2. strokeStyle 设置图形轮廓的颜色默认情况下为黑色
### 其他属性
- Transparency(透明度)`globalAlpha = transparencyValue`
- globalAlpha 属性在需要绘制大量拥有相同透明度的图形时候相当高效。不过，我认为使用rgba()设置透明度更加好一些。
- `lineWidth = value`设置线宽
- `lineCap = type`线条末端样式，共有三个值：butt以方形结束|round以圆形结束|square以方形结束，但是增加一个宽度和线段相同，高度是线段宽度一般的矩形区域
- `lineJoin = type`统一path内，设定线条和线条间接合处的样式，共有三个值：round弧线|bevel末端填充一个三角形|miter默认值
- `setLineDash`和`lineDashOffset`制定虚线样式
### 绘制文本
```
var ctx;
function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    ctx = canvas.getContext("2d");
    ctx.font = "100px sans-serif"
    ctx.fillText("天若有情", 10, 100);
    ctx.strokeText("天若有情", 10, 200)
}
draw();
```
### 绘制图片缩放图片对图片切片
### 变形旋转，变形矩阵的使用
### 动画实例
- 模拟太阳系
```
let sun;
let earth;
let moon;
let ctx;
function init(){
    sun = new Image();
    earth = new Image();
    moon = new Image();
    sun.src = "sun.png";
    earth.src = "earth.png";
    moon.src = "moon.png";

    let canvas = document.querySelector("#solar");
    ctx = canvas.getContext("2d");

    sun.onload = function (){
        draw()
    }

}
init();
function draw(){
    ctx.clearRect(0, 0, 300, 300); //清空所有的内容
    /*绘制 太阳*/
    ctx.drawImage(sun, 0, 0, 300, 300);

    ctx.save();
    ctx.translate(150, 150);

    //绘制earth轨道
    ctx.beginPath();
    ctx.strokeStyle = "rgba(255,255,0,0.5)";
    ctx.arc(0, 0, 100, 0, 2 * Math.PI)
    ctx.stroke()

    let time = new Date();
    //绘制地球
    ctx.rotate(2 * Math.PI / 60 * time.getSeconds() + 2 * Math.PI / 60000 * time.getMilliseconds())
    ctx.translate(100, 0);
    ctx.drawImage(earth, -12, -12)

    //绘制月球轨道
    ctx.beginPath();
    ctx.strokeStyle = "rgba(255,255,255,.3)";
    ctx.arc(0, 0, 40, 0, 2 * Math.PI);
    ctx.stroke();

    //绘制月球
    ctx.rotate(2 * Math.PI / 6 * time.getSeconds() + 2 * Math.PI / 6000 * time.getMilliseconds());
    ctx.translate(40, 0);
    ctx.drawImage(moon, -3.5, -3.5);
    ctx.restore();

    requestAnimationFrame(draw);
}
```
- 模拟钟表
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body {
            padding: 0;
            margin: 0;
            background-color: rgba(0, 0, 0, 0.1)
        }
        
        canvas {
            display: block;
            margin: 200px auto;
        }
    </style>
</head>
<body>
<canvas id="solar" width="300" height="300"></canvas>
<script>
    init();

    function init(){
        let canvas = document.querySelector("#solar");
        let ctx = canvas.getContext("2d");
        draw(ctx);
    }

    function draw(ctx){
        requestAnimationFrame(function step(){
            drawDial(ctx); //绘制表盘
            drawAllHands(ctx); //绘制时分秒针
            requestAnimationFrame(step);
        });
    }
    /*绘制时分秒针*/
    function drawAllHands(ctx){
        let time = new Date();

        let s = time.getSeconds();
        let m = time.getMinutes();
        let h = time.getHours();
        
        let pi = Math.PI;
        let secondAngle = pi / 180 * 6 * s;  //计算出来s针的弧度
        let minuteAngle = pi / 180 * 6 * m + secondAngle / 60;  //计算出来分针的弧度
        let hourAngle = pi / 180 * 30 * h + minuteAngle / 12;  //计算出来时针的弧度

        drawHand(hourAngle, 60, 6, "red", ctx);  //绘制时针
        drawHand(minuteAngle, 106, 4, "green", ctx);  //绘制分针
        drawHand(secondAngle, 129, 2, "blue", ctx);  //绘制秒针
    }
    /*绘制时针、或分针、或秒针
     * 参数1：要绘制的针的角度
     * 参数2：要绘制的针的长度
     * 参数3：要绘制的针的宽度
     * 参数4：要绘制的针的颜色
     * 参数4：ctx
     * */
    function drawHand(angle, len, width, color, ctx){
        ctx.save();
        ctx.translate(150, 150); //把坐标轴的远点平移到原来的中心
        ctx.rotate(-Math.PI / 2 + angle);  //旋转坐标轴。 x轴就是针的角度
        ctx.beginPath();
        ctx.moveTo(-4, 0);
        ctx.lineTo(len, 0);  // 沿着x轴绘制针
        ctx.lineWidth = width;
        ctx.strokeStyle = color;
        ctx.lineCap = "round";
        ctx.stroke();
        ctx.closePath();
        ctx.restore();
    }
    
    /*绘制表盘*/
    function drawDial(ctx){
        let pi = Math.PI;
        
        ctx.clearRect(0, 0, 300, 300); //清除所有内容
        ctx.save();

        ctx.translate(150, 150); //一定坐标原点到原来的中心
        ctx.beginPath();
        ctx.arc(0, 0, 148, 0, 2 * pi); //绘制圆周
        ctx.stroke();
        ctx.closePath();

        for (let i = 0; i < 60; i++){//绘制刻度。
            ctx.save();
            ctx.rotate(-pi / 2 + i * pi / 30);  //旋转坐标轴。坐标轴x的正方形从 向上开始算起
            ctx.beginPath();
            ctx.moveTo(110, 0);
            ctx.lineTo(140, 0);
            ctx.lineWidth = i % 5 ? 2 : 4;
            ctx.strokeStyle = i % 5 ? "blue" : "red";
            ctx.stroke();
            ctx.closePath();
            ctx.restore();
        }
        ctx.restore();
    }
</script>
</body>
</html>
```
