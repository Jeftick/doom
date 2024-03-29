<html>
<head>
    <title>Графический редактор</title>
    <style>
        #canvas {
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="canvas" width="500" height="500"></canvas>
    <br>
    <input type="color" id="colorPicker">
    <input type="range" id="lineWidthSlider" min="1" max="10" value="1">
    <button onclick="setMode('draw')">Рисовать</button>
    <button onclick="setMode('erase')">Ластик</button>
    <button onclick="setMode('fill')">Заливка</button>

    <script>
        var canvas = document.getElementById('canvas');
        var ctx = canvas.getContext('2d');
        var mode = 'draw';
        var isDrawing = false;

        canvas.addEventListener('mousedown', startDrawing);
        canvas.addEventListener('mousemove', draw);
        canvas.addEventListener('mouseup', stopDrawing);

        function startDrawing(e) {
            isDrawing = true;
            draw(e);
        }

        function draw(e) {
            if (!isDrawing) return;
            var x = e.clientX - canvas.offsetLeft;
            var y = e.clientY - canvas.offsetTop;
            var color = document.getElementById('colorPicker').value;
            var lineWidth = document.getElementById('lineWidthSlider').value;

            ctx.beginPath();
            ctx.lineCap = 'round';
            ctx.strokeStyle = color;
            ctx.lineWidth = lineWidth;

            if (mode === 'draw') {
                ctx.lineTo(x, y);
                ctx.stroke();
            } else if (mode === 'erase') {
                ctx.globalCompositeOperation = 'destination-out';
                ctx.lineTo(x, y);
                ctx.stroke();
                ctx.globalCompositeOperation = 'source-over';
            } else if (mode === 'fill') {
                ctx.fillStyle = color;
                ctx.fillRect(0, 0, canvas.width, canvas.height);
            }
        }

        function stopDrawing() {
            isDrawing = false;
            ctx.closePath();
        }

        function setMode(newMode) {
            mode = newMode;
        }
    </script>
</body>
</html>


