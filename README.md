# snake 
snake game (简单版贪吃蛇小游戏、使用js和canvas制作)
<img width="647" alt="WX20221222-172804@2x" src="https://user-images.githubusercontent.com/22751103/209102980-fd2b701e-70a4-407e-bfa2-ec7211119cc2.png">
思路
任何代码开发之前都需要有个清晰的思路，这样后续开发才能游刃有余。
● 生成一个表格
● 随机在表格空白坐标点生成食物
● 生成一条小蛇
● 蛇不能撞墙，也不能碰到自己
● 每吃一个食物身体长一格
● 蛇有固定的初始速度
● 方向的改变用键盘的方向键控制，不能直接往反方向转向

表格生成
    const grawGrid = function() {
        // 绘制 网格 横线
        for (let i = 1; i < maxY; i++) {
            ctx.moveTo(0, i * itemWidth)
            ctx.lineTo(canvasHeight, i * itemWidth)
        }
        
        // 绘制 网格 竖线
        for (let i = 1; i < maxX; i++) {
            ctx.moveTo(i * itemWidth, 0)
            ctx.lineTo(i * itemWidth, canvasWidth)
        }
        ctx.lineWidth = 1
        ctx.strokeStyle = '#ddd'
        ctx.stroke()
    }

食物的生成
        // 绘制食物
        ctx.fillStyle="#FFB433"
        ctx.fillRect(
            food[0] * itemWidth + flashing,
            food[1] * itemWidth + flashing,
            itemWidth - flashing * 2,
            itemWidth - flashing * 2 
        )

        // XOR异或运算 同值取0 异值取1 （食物的闪缩效果）
        flashing ^= 1

蛇的生成
        // 绘制蛇头
        ctx.fillStyle="#FF5733"
        ctx.fillRect(
            snake[0][0] * itemWidth + 0.5,
            snake[0][1] * itemWidth + 0.5,
            itemWidth - 1,
            itemWidth - 1
        )

        // 绘制蛇身 与蛇头的颜色不同
        ctx.fillStyle="#E4FF33"
        for (let i = 1; i < snake.length; i++) {
            ctx.fillRect(
                snake[i][0] * itemWidth + 0.5,
                snake[i][1] * itemWidth + 0.5,
                itemWidth - 1,
                itemWidth - 1
            )
        }

蛇的运动和成长
    // 蛇的自动运动
    const snakeAutoMove = function(){
        let [x, y] = snake[0]
        switch(direction) {
            case 'left':
                x--
                break
            case 'right':
                x++
                break
            case 'up':
                y--
                break
            case 'down':
                y++
                break
        }

        // 是否吃到食物
        if(x === food[0] && y === food[1]){
            makeFood()
            addScore()
        } else {
            snake.pop()
        }
        // 结束判断
        if(over([x, y])){
            clearInterval(timer)
            return 
        }
        snake.unshift([x, y])
    }
蛇的运动
主要是采用定时器的写法，配合canvas不断刷新，显示出运动的效果
        timer = setInterval(()=>{
          // 绘制图像
            draw()
          // 改变蛇的位置
            snakeAutoMove()
        }, snakeSpeed)
方向控制
    // 蛇的案件控制
    const snakeControlListener = function() {
        document.onkeydown = function(e) {
            switch(e.keyCode) {
            case 37:
                if (direction !== 'right') {
                    direction = 'left'
                }
                break
            case 38:
                if (direction !== 'down') {
                    direction = 'up'
                }
                break
            case 39:
                if (direction !== 'left') {
                    direction = 'right'
                }
                break
            case 40:
                if (this.direction !== 'up') {
                    direction = 'down'
                }
                break
            }
        }
    }
