### 舞动时钟

### 效果图如下所示
![跳动的时钟](images/跳动的时钟.gif)
### 原理分析

* 时间是由一些小圆点组合而成的,小球是由canvas绘制而成的
* 绘制时间的文字,绘制彩色的小球,每当时间变化时,都会有彩色的小球在x轴,y轴都有一个对应的变化

### 实现代码

```
<!doctype html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="Keywords" content="关键词">
        <meta name="Description" content="描述">
        <title>Document</title>
        <style>
        *{margin:0px;padding:0px;}
        canvas{margin:50px auto;display:block}

        </style>
    </head>
    <body><!--身体-->
        <canvas id="canvas" width="920" height="400">
            您的浏览器不支持canvas标签，请您更换浏览器！！
        </canvas>
        <script>
            var canvas = document.getElementById("canvas");
            var context = canvas.getContext("2d");
            var w = canvas.width;//获取canvas画布的宽度
            var h = canvas.height;//获取canvas画布的高度
            var radius = 7;//设置小球的半径
            var padd = 10;//设置数字之间的间距
            var balls = [];//储存彩色小球
            var u = 0.65;//小球的碰撞能力损失系数
            var currentNums = [];//储存当前屏幕的8个数字
            var digit =
                    [
                        [
                            [0,0,1,1,1,0,0],
                            [0,1,1,0,1,1,0],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [0,1,1,0,1,1,0],
                            [0,0,1,1,1,0,0]
                        ],//0
                        [
                            [0,0,0,1,1,0,0],
                            [0,1,1,1,1,0,0],
                            [0,0,0,1,1,0,0],
                            [0,0,0,1,1,0,0],
                            [0,0,0,1,1,0,0],
                            [0,0,0,1,1,0,0],
                            [0,0,0,1,1,0,0],
                            [0,0,0,1,1,0,0],
                            [0,0,0,1,1,0,0],
                            [1,1,1,1,1,1,1]
                        ],//1
                        [
                            [0,1,1,1,1,1,0],
                            [1,1,0,0,0,1,1],
                            [0,0,0,0,0,1,1],
                            [0,0,0,0,1,1,0],
                            [0,0,0,1,1,0,0],
                            [0,0,1,1,0,0,0],
                            [0,1,1,0,0,0,0],
                            [1,1,0,0,0,0,0],
                            [1,1,0,0,0,1,1],
                            [1,1,1,1,1,1,1]
                        ],//2
                        [
                            [1,1,1,1,1,1,1],
                            [0,0,0,0,0,1,1],
                            [0,0,0,0,1,1,0],
                            [0,0,0,1,1,0,0],
                            [0,0,1,1,1,0,0],
                            [0,0,0,0,1,1,0],
                            [0,0,0,0,0,1,1],
                            [0,0,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [0,1,1,1,1,1,0]
                        ],//3
                        [
                            [0,0,0,0,1,1,0],
                            [0,0,0,1,1,1,0],
                            [0,0,1,1,1,1,0],
                            [0,1,1,0,1,1,0],
                            [1,1,0,0,1,1,0],
                            [1,1,1,1,1,1,1],
                            [0,0,0,0,1,1,0],
                            [0,0,0,0,1,1,0],
                            [0,0,0,0,1,1,0],
                            [0,0,0,1,1,1,1]
                        ],//4
                        [
                            [1,1,1,1,1,1,1],
                            [1,1,0,0,0,0,0],
                            [1,1,0,0,0,0,0],
                            [1,1,1,1,1,1,0],
                            [0,0,0,0,0,1,1],
                            [0,0,0,0,0,1,1],
                            [0,0,0,0,0,1,1],
                            [0,0,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [0,1,1,1,1,1,0]
                        ],//5
                        [
                            [0,0,0,0,1,1,0],
                            [0,0,1,1,0,0,0],
                            [0,1,1,0,0,0,0],
                            [1,1,0,0,0,0,0],
                            [1,1,0,1,1,1,0],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [0,1,1,1,1,1,0]
                        ],//6
                        [
                            [1,1,1,1,1,1,1],
                            [1,1,0,0,0,1,1],
                            [0,0,0,0,1,1,0],
                            [0,0,0,0,1,1,0],
                            [0,0,0,1,1,0,0],
                            [0,0,0,1,1,0,0],
                            [0,0,1,1,0,0,0],
                            [0,0,1,1,0,0,0],
                            [0,0,1,1,0,0,0],
                            [0,0,1,1,0,0,0]
                        ],//7
                        [
                            [0,1,1,1,1,1,0],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [0,1,1,1,1,1,0],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [0,1,1,1,1,1,0]
                        ],//8
                        [
                            [0,1,1,1,1,1,0],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [1,1,0,0,0,1,1],
                            [0,1,1,1,0,1,1],
                            [0,0,0,0,0,1,1],
                            [0,0,0,0,0,1,1],
                            [0,0,0,0,1,1,0],
                            [0,0,0,1,1,0,0],
                            [0,1,1,0,0,0,0]
                        ],//9
                        [
                            [0,0,0,0],
                            [0,0,0,0],
                            [0,1,1,0],
                            [0,1,1,0],
                            [0,0,0,0],
                            [0,0,0,0],
                            [0,1,1,0],
                            [0,1,1,0],
                            [0,0,0,0],
                            [0,0,0,0]
                        ]//:
                    ];
            //绘制时间的数字 
            function drawDatetime(cxt){
                var nums = [];
                var offsetX = 70;
                var offsetY = 30;
                var date = new Date();
                //获取小时的数字和数组关联起来
                var hours = date.getHours();
                var num1 = Math.floor(hours/10);
                var num2 = hours%10;
                nums.push({num:num1});
                nums.push({num:num2});
                nums.push({num:10});//冒号
                //获取分的数字和数组关联起来
                var min = date.getMinutes();
                var num1 = Math.floor(min/10);
                var num2 = min%10;
                nums.push({num:num1});
                nums.push({num:num2});
                nums.push({num:10});//冒号
                //获取秒的数字和数组关联起来
                var sec = date.getSeconds();
                var num1 = Math.floor(sec/10);
                var num2 = sec%10;
                nums.push({num:num1});
                nums.push({num:num2});
                
                for(var x=0;x<nums.length;x++ ){
                    nums[x].offsetX = offsetX;
                    offsetX = drawNumber(offsetX,offsetY,nums[x].num,cxt);
                    //若果两个数字连在一起，应该有一定的间距
                    if(x<nums.length-1){
                        if((nums[x].num!=10) && (nums[x+1].num!=10)){
                            offsetX += padd;
                        }
                    }
                }

                if(currentNums.length == 0){
                    currentNums = nums;
                }else{
                    for(var i = 0;i<currentNums.length;i++){
                        if(currentNums[i].num != nums[i].num){
                            addBalls(nums[i]);//添加彩色小球
                            currentNums[i].num = nums[i].num;
                        }
                    }
                }
                //绘制出彩色小球
                drawBalls(cxt);
                //移除小球 
                moveBalls();
                
                //return date;
        }
            //绘制数字
            function drawNumber(offsetX,offsetY,num,cxt){
                var numMatrix = digit[num];
                context.fillStyle = "#6666ff";
                for(var y=0;y<numMatrix.length;y++){
                    for(var x=0;x<numMatrix[y].length;x++){
                        if(numMatrix[y][x]==1){
                            cxt.beginPath();
                            cxt.arc(offsetX+radius+radius*2*x,offsetY+radius+radius*2*y,radius,0,360,false);
                            cxt.fill();
                        }
                    }
                }
                offsetX += numMatrix[0].length*radius*2;
                return offsetX;
            }
            //添加彩色小球
            function addBalls(item){
                var num = item.num;
                var numMatrix = digit[num]
                for(var y=0;y<numMatrix.length;y++){
                    for(var x=0;x<numMatrix[y].length;x++){
                        if(numMatrix[y][x]==1){
                            var ball = {
                                offsetX : item.offsetX+radius+radius*2*x,
                                offsetY : 30+radius+radius*2*y,
                                color : color(),
                                sx : Math.pow(-1,Math.ceil(Math.random()*10))*4+Math.random(),
                                sy : -5,
                                g : 1.5+Math.random()
                            }
                            balls.push(ball);
                        }
                    }
                }
            }
            //绘制彩色小
            function drawBalls(cxt){
                for(var i = 0;i<balls.length;i++){
                    cxt.beginPath();
                    cxt.arc(balls[i].offsetX,balls[i].offsetY,radius,0,360,false);
                    cxt.fillStyle = balls[i].color;
                    cxt.fill();
                }

                
            }
            //移除彩色小球
            function moveBalls(){
                var n = 0;
                for(var i=0;i<balls.length;i++){
                    var ball = balls[i];
                    ball.offsetX += ball.sx;
                    ball.offsetY += ball.sy;
                    ball.sy += ball.g;
                    
                    if(ball.offsetY > (h-radius)){
                        ball.offsetY = h-radius;
                        ball.sy = -ball.sy*u;
                    }
                    if(ball.offsetX > radius && ball.offsetX < (w-radius)){
                        balls[n] = balls[i];
                        n++;
                    }
                }

                for(;n<balls.length;n++){
                    balls.pop();//删除并返回数组中的最后一个小球
                }

            }
            setInterval(function(){
                context.clearRect(0,0,w,h);
                drawDatetime(context);
            },50);
            //生成随机颜色
            function color(){
                var _color= Math.ceil(Math.random()*16777215).toString(16);
                while(_color.length<6){
                    _color += "0"+_color;
                }
                //console.log(_color);
                return "#"+_color
            }
            //绘制彩色小球 
            //彩色小球移动起来
            //在canvas   X边界上的彩色小球消失
        </script>
    </body>
</html>

```