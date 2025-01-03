<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fireworks</title>
    <style>
        body { margin: 0; overflow: hidden; background: black; }
        canvas { position: absolute; top: 0; left: 0; }
    </style>
</head>
<body>
    <canvas id="fireworksCanvas"></canvas>

    <script>
        const canvas = document.getElementById('fireworksCanvas');
        const ctx = canvas.getContext('2d');
        const particles = [];
        let w, h;

        // 设置画布尺寸
        function resizeCanvas() {
            w = window.innerWidth;
            h = window.innerHeight;
            canvas.width = w;
            canvas.height = h;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // 粒子类
        class Particle {
            constructor(x, y, color, size, speed, angle) {
                this.x = x;
                this.y = y;
                this.color = color;
                this.size = size;
                this.speed = speed;
                this.angle = angle;
                this.life = 1;
            }

            update() {
                this.x += Math.cos(this.angle) * this.speed;
                this.y += Math.sin(this.angle) * this.speed;
                this.size *= 0.98;
                this.life -= 0.02;
            }

            draw() {
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fillStyle = this.color;
                ctx.fill();
            }
        }

        // 生成烟花
        function createFirework(x, y) {
            const colors = ['#ff0033', '#33ccff', '#ffcc00', '#33ff33', '#cc33ff'];
            const numParticles = 100;
            for (let i = 0; i < numParticles; i++) {
                const angle = Math.random() * Math.PI * 2;
                const speed = Math.random() * 3 + 2;
                const color = colors[Math.floor(Math.random() * colors.length)];
                const size = Math.random() * 3 + 1;
                particles.push(new Particle(x, y, color, size, speed, angle));
            }
        }

        // 主动画循环
        function animate() {
            ctx.clearRect(0, 0, w, h);
            particles.forEach((particle, index) => {
                particle.update();
                particle.draw();
                if (particle.life <= 0) {
                    particles.splice(index, 1);
                }
            });

            // 随机生成烟花
            if (Math.random() < 0.05) {
                createFirework(Math.random() * w, Math.random() * h);
            }

            requestAnimationFrame(animate);
        }

        animate();
    </script>
</body>
</html>
