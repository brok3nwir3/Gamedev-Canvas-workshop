<canvas id="myCanvas" width="895" height="640"></canvas>

<script>
	//////////---Ball Bouncing Game---//////////
    var canvas = document.getElementById("myCanvas");
    var ctx = canvas.getContext("2d");

    // Determines ball starting location (bottom by default).
    var x = canvas.width/2;
    var y = canvas.height-30;

    // Set random ball direction from 2-11.
    var dx = Math.floor((Math.random()*10)+1);

    // Initial ball speed.
    var dy = -4;
    
    // Global variables.
    var score = 0;
    var tempScore = 0;
    var totalScore = 0;
    var time = 0;
    var timePenalty = 0;
    var lives = 3;
    var ballRadius = 10;
    var tailRadius1 = 6;
    var tailRadius2 = 4;
    var tailRadius3 = 2;
    var setTailLeft = true;
    var setTailRight = false;
    var setTailBottom = true;
    var setTailTop = false;
    var colorSwap = false;
    var brickExplosion = false;
    var delayExplosion = 0;
    var savedPlacement = false;
    var explosionX = 0;
    var explosionY = 0;
    var paddleHeight = 10;
    var paddleWidth = 75;
    var paddleX = (canvas.width-paddleWidth)/2;
    var rightPressed = false;
    var leftPressed = false;
    var brickRowCount = 8;
    var brickColumnCount = 10;
    var brickWidth = 75;
    var brickHeight = 20;
    var brickPadding = 10;
    var brickOffsetTop = 30;
    var brickOffsetLeft = 30;

    // Instantiate a 2D array for the bricks, setting all x and y values to 0 by default.
    var bricks = [];
    for(var c=0; c<brickColumnCount; c++){
        bricks[c] = [];
        for(var r=0; r<brickRowCount; r++){
            bricks[c][r] = { x: 0, y: 0, status: 1 };
        }
    }

    // Listeners
    document.addEventListener("keydown", keyDownHandler, false);
    document.addEventListener("keyup", keyUpHandler, false);
    document.addEventListener("mousemove", mouseMoveHandler, false);

    // Assign new random ball trajectory, if greater than 5. 
    // Acceptable value range = 2-5.
    function adjustDX(){
        while(dx > 5){
            dx = Math.floor((Math.random()*10)+1);
        }
    }

    // Collision detections for the bricks.
    function collisionDetection(){
        for(var c=0; c<brickColumnCount; c++){
            for(var r=0; r<brickRowCount; r++){
                var b = bricks[c][r];
                if(b.status == 1) {
                    if(x > b.x && x < b.x+brickWidth && y > b.y && y < b.y+brickHeight){
                        dy = -dy;
                        b.status = 0;
                        savedPlacement = false;
                        brickExplosion = true;
                        score++;
                        // Win condition.
                        if(score == brickRowCount*brickColumnCount){
                            alert("YOU WIN, CONGRATULATIONS!\n\nScore: "+score+"\nTime Penalty: "+timePenalty+"\nLives Multiplier: "+lives+"\n\nTotal Score: "+totalScore);
                            document.location.reload();
                        }
                    }
                }
            }
        }
    }

    function explosion1(){
        if(brickExplosion == true){
            ctx.beginPath();
            ctx.fillStyle = "Gold";
            // Regular particles.
            ctx.rect(explosionX+10, explosionY+10, 10, 10);
            ctx.rect(explosionX+10, explosionY-10, 10, 10);
            ctx.rect(explosionX-10, explosionY+10, 10, 10);
            ctx.rect(explosionX-10, explosionY-10, 10, 10);
            ctx.fill();
            ctx.closePath();
            //BrickExplosion = false;
        }
        else if(brickExplosion == false){
        // Do nothing.
        }
    }

    function explosion2(){
        if(brickExplosion == true){
            ctx.beginPath();
            ctx.fillStyle = "Orange";
            // Small particles.
            ctx.rect(explosionX+20, explosionY+20, 5, 5);
            ctx.rect(explosionX+20, explosionY-20, 5, 5);
            ctx.rect(explosionX-20, explosionY+20, 5, 5);
            ctx.rect(explosionX-20, explosionY-20, 5, 5);
            ctx.fill();
            ctx.closePath();
            //BrickExplosion = false;
        }
        else if(brickExplosion == false){
        // Do nothing.
        }
    }

    function explosion3(){
        if(brickExplosion == true){
            ctx.beginPath();
            ctx.fillStyle = "Red";
            // Small particles.
            ctx.rect(explosionX+25, explosionY+25, 3, 3);
            ctx.rect(explosionX+25, explosionY-25, 3, 3);
            ctx.rect(explosionX-25, explosionY+25, 3, 3);
            ctx.rect(explosionX-25, explosionY-25, 3, 3);
            ctx.fill();
            ctx.closePath();
            //BrickExplosion = false;
        }
        else if(brickExplosion == false){
        // Do nothing.
        }
    }

    function drawScore(){
        ctx.font = "bold 16px Arial";
        ctx.fillStyle = "Black";
        ctx.fillText("Score: "+score, 8, 20);
    }

    // XY display function for troubleshooting.
    function drawXY(){
        ctx.font = "bold 16px Arial";
        ctx.fillStyle = "Black";
        ctx.fillText("X: "+x, canvas.width/4, 20);
        ctx.fillText("Y: "+y, canvas.width/3, 20);
    }

    // Up, down, left, right display function for troubleshooting.
    function drawDirection(){
        ctx.font = "bold 16px Arial";
        ctx.fillStyle = "Black";
        ctx.fillText("Top: "+setTailTop, canvas.width/4, 20);
        ctx.fillText("Bottom: "+setTailBottom, canvas.width/3, 20);
        ctx.fillText("Left: "+setTailLeft, canvas.width/8, 20);
        ctx.fillText("Right: "+setTailRight, canvas.width/40, 20);
    }

    function drawLives(){
    // If lives equals 1 set color to red.
        if(lives ==  1){
            ctx.font = "bold 16px Arial";
            ctx.fillStyle = "#FF0000";
            ctx.fillText("Lives: "+lives, canvas.width-65, 20);
        }
    // If lives equals 2 set color to orange.
        if(lives == 2){
            ctx.font = " bold 16px Arial";
            ctx.fillStyle = "#FFA500";
            ctx.fillText("Lives: "+lives, canvas.width-65, 20);
        }
    // If lives equals 3 set color to green.
        else if(lives == 3){
            ctx.font = "bold 16px Arial";
            ctx.fillStyle = "#008000";
            ctx.fillText("Lives: "+lives, canvas.width-65, 20);
        }
    }

    function drawTime(){
        ctx.font = "bold 16px Arial";
        ctx.fillStyle = "#000000";
        ctx.fillText("Time: "+time/100, canvas.width/8, 20);
    }

    function keyDownHandler(e){
    if(e.key == "Right" || e.key == "ArrowRight"){
        rightPressed = true;
        }
    else if(e.key == "Left" || e.key == "ArrowLeft"){
        leftPressed = true;
        }
    }

    function keyUpHandler(e){
    if(e.key == "Right" || e.key == "ArrowRight"){
        rightPressed = false;
        }
    else if(e.key == "Left" || e.key == "ArrowLeft"){
        leftPressed = false;
        }
    }

    function mouseMoveHandler(e){
    var relativeX = e.clientX - canvas.offsetLeft;
        if(relativeX > 0 && relativeX < canvas.width){
            paddleX = relativeX - paddleWidth/2;
        }
    }

    function drawBall(){
    ctx.beginPath();
    ctx.arc(x, y, ballRadius, 0, Math.PI*2);
    ctx.fillStyle = "Crimson";
    ctx.fill();
    ctx.closePath();
    }

    function drawTail(){
        //Bottom right tail.
        if(setTailBottom == true && setTailRight == true){
            ctx.arc(x+15, y+15, tailRadius1, 0, Math.PI*2);
            ctx.arc(x+25, y+25, tailRadius2, 0, Math.PI*2);
            ctx.arc(x+35, y+35, tailRadius3, 0, Math.PI*2);
        }
        // Top right tail.
        if(setTailTop == true && setTailRight == true){
            ctx.arc(x+15, y-15, tailRadius1, 0, Math.PI*2);
            ctx.arc(x+25, y-25, tailRadius2, 0, Math.PI*2);
            ctx.arc(x+35, y-35, tailRadius3, 0, Math.PI*2);
        }
        // Top left tail.
        if(setTailTop == true && setTailLeft == true){
            ctx.arc(x-15, y-15, tailRadius1, 0, Math.PI*2);
            ctx.arc(x-25, y-25, tailRadius2, 0, Math.PI*2);
            ctx.arc(x-35, y-35, tailRadius3, 0, Math.PI*2);
        }
        // Bottom left tail.
        else if(setTailBottom == true && setTailLeft == true){
            ctx.arc(x-15, y+15, tailRadius1, 0, Math.PI*2);
            ctx.arc(x-25, y+25, tailRadius2, 0, Math.PI*2);
            ctx.arc(x-35, y+35, tailRadius3, 0, Math.PI*2);
        }
        if(colorSwap == false && time%3 == 0){
            colorSwap = true;
            ctx.fillStyle = "Coral";
            ctx.fill();
            ctx.closePath();
        }
        else if(colorSwap == true && time%3 == 0){
            colorSwap = false;
            ctx.fillStyle = "Crimson";
            ctx.fill();
            ctx.closePath();
        }
    }

    function drawPaddle(){
    ctx.beginPath();
    ctx.rect(paddleX, canvas.height-paddleHeight, paddleWidth, paddleHeight);
    ctx.fillStyle = "MidnightBlue";
    ctx.fill();
    ctx.closePath();
    }

    function drawBricks(){
        for(var c=0; c<brickColumnCount; c++){
            for(var r=0; r<brickRowCount; r++){
                if(bricks[c][r].status == 1){
                    var brickX = (c*(brickWidth+brickPadding))+brickOffsetLeft;
                    var brickY = (r*(brickHeight+brickPadding))+brickOffsetTop;
                    bricks[c][r].x = brickX;
                    bricks[c][r].y = brickY;
                    ctx.beginPath();
                    ctx.rect(brickX, brickY, brickWidth, brickHeight);
                    // Set odd numbered bricks to a different color.
                    if(c%2 > 0){
                       ctx.fillStyle = "CadetBlue"; 
                    }
                    else{
                       ctx.fillStyle = "DarkCyan";
                    }
                    ctx.fill();
                    ctx.closePath();
                }
            }
        }
    }

    // Increments the ball speed, every 10 points.
    function speedIncrease(){
        if(tempScore != score){
            if(score%10 == 0 && dy < 0){
                dy--;
                tempScore = score;
            }
            if(score%10 == 0 && dy > 0){
                dy++;
                tempScore = score;
            }
            else{
                tempScore = score;
            }
        }
    }

    function calcPenalty(){
        if(time/100 > 180){
            timePenalty = 30;
        }
        if(time/100 > 120){
            timePenalty = 20;
        }
        else{
            timePenalty = 10;
        }
    }

    function draw(){
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        calcPenalty();
        adjustDX();
        drawBall();
        drawTail();
        drawPaddle();
        drawBricks();
        drawScore();
        //drawXY();
        //drawDirection();
        drawLives();
        totalScore = (score-timePenalty)*lives;
        explosion1();
        collisionDetection();
        speedIncrease();

        // Right wall bounce.
        if(x + dx > canvas.width-ballRadius){
            dx = -dx;
            setTailLeft = false;
            setTailRight = true;
        }
        // Left wall bounce.
        else if( x + dx < ballRadius){
            dx = -dx;
            setTailRight = false;
            setTailLeft = true;
        }

        // Ceiling bounce.
        if(y + dy < ballRadius){
            dy = -dy;
        }

        else if(y + dy > canvas.height-ballRadius){
            // Paddle bounce.
            if(x > paddleX && x < paddleX + paddleWidth){
                dy = -dy;
            }
            // Paddle miss.
            else{
            lives--;
            totalScore = (score-timePenalty)*lives;
            drawLives();
                if(!lives){
                    alert("GAME OVER\n\nScore: "+score+"\nTime Penalty: "+timePenalty+"\nLives Multiplier: 1\n\nTotal Score: "+totalScore);
                    document.location.reload();
                }
                // When a life is lost, reset the ball and apply a new random direction.
                else {
                    x = canvas.width/2;
                    y = canvas.height-30;
                    dx = Math.floor((Math.random()*10)+1);
                    dy = -4;
                    paddleX = (canvas.width-paddleWidth)/2;
                    setTailLeft = true;
                    setTailRight = false;
                }
            }
        }
        
        // Right arrow key moves paddle right.
        if(rightPressed){
            paddleX += 7;
            if (paddleX + paddleWidth > canvas.width){
            paddleX = canvas.width - paddleWidth;
            }
        }

        // Left arrow key moves paddle left.
        else if(leftPressed){
            paddleX -= 7;
            if (paddleX < 0){
            paddleX = 0;
            }
        }

        // Increment ball position.
        y += dy;
        x += dx;

        // Determine tail up or down direction.
        if((y + dy) > y){
            setTailBottom = false;
            setTailTop = true;
        }
        else if((y + dy) < y ){
            setTailBottom = true;
            setTailTop = false;
        }

        // Save explosion placement.
        if(savedPlacement == false && brickExplosion == true){
            explosionX = x;
            explosionY = y;
            savedPlacement = true;
        }
        else if(savedPlacement == true){
        // Do nothing.
        }

        // Start explosion visual delay.
        if(brickExplosion == true){
           delayExplosion++;
        }
        else if(brickExplosion == false){
        // Do nothing.
        }
        
        // Start secondary explosion.
        if(delayExplosion > 10){
            explosion2();
        }
        else if(delayExplosion < 10){
        // Do nothing.
        }

        // Start tertiary explosion.
        if(delayExplosion > 20){
            explosion3();
        }
        else if(delayExplosion < 20){
        // Do nothing.
        }

        // End explosion visual delay.
        if(delayExplosion == 30){
           delayExplosion = 0;
           brickExplosion = false;
        }
        else if(delayExplosion != 30){
        // Do nothing.
        }
        
        // Increment game time.
        time++;

        drawTime();
        requestAnimationFrame(draw);
    }
    draw();

</script>
