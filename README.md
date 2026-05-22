
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>AI Level Game</title>
<style>
body { margin:0; background:#0b0f1a; color:white; font-family:Arial; text-align:center; }
canvas { background:#111827; display:block; margin:auto; }
</style>
</head>
<body>

<h2>🤖 AI GAME LEVELS</h2>
<canvas id="game" width="900" height="450"></canvas>

<script>
const c = document.getElementById("game");
const ctx = c.getContext("2d");

class Player {
    constructor(name,color){
        this.name = name;
        this.x = Math.random()*700;
        this.y = 350;
        this.vx = 0;
        this.vy = 0;
        this.hp = 100;
        this.color = color;
        this.grounded = true;
    }

    move(){
        this.x += this.vx;
        this.y += this.vy;

        // gravity
        this.vy += 0.5;

        // ground
        if(this.y > 350){
            this.y = 350;
            this.vy = 0;
            this.grounded = true;
        }

        // boundaries
        if(this.x < 0) this.x = 0;
        if(this.x > 900) this.x = 900;
    }

    run(){
        this.vx = (Math.random() - 0.5) * 6;
    }

    jump(){
        if(this.grounded){
            this.vy = -10;
            this.grounded = false;
        }
    }

    attack(enemy){
        if(Math.abs(this.x - enemy.x) < 50){
            enemy.hp -= 5;
        }
    }

    draw(){
        ctx.fillStyle = this.color;
        ctx.fillRect(this.x,this.y,20,20);

        ctx.fillStyle="red";
        ctx.fillRect(this.x,this.y-10,this.hp/2,5);

        ctx.fillStyle="white";
        ctx.fillText(this.name,this.x-10,this.y-15);
    }

    dead(){
        return this.hp <= 0;
    }
}

// AI Players
let bots = [
    new Player("AI-1","red"),
    new Player("AI-2","cyan"),
    new Player("AI-3","yellow")
];

let level = 1;

function loop(){
    ctx.clearRect(0,0,900,450);

    ctx.fillStyle="white";
    ctx.fillText("LEVEL: "+level,20,20);

    bots.forEach(b=>{
        if(!b.dead()){
            b.run();
            if(Math.random()<0.02) b.jump();

            let enemy = bots.find(x => x !== b);
            if(enemy){
                b.attack(enemy);
            }

            b.move();
            b.draw();
        }
    });

    // remove dead
    bots = bots.filter(b => !b.dead());

    // level up
    if(bots.length <= 1){
        level++;
        bots = [
            new Player("AI-1","red"),
            new Player("AI-2","cyan"),
            new Player("AI-3","yellow")
        ];
    }

    requestAnimationFrame(loop);
}

loop();
</script>

</body>
</html>
