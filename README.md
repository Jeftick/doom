<html>
<head>
  <title>Графический редактор</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f2f2f2;
      margin: 0;
      padding: 0;
    }

    h1 {
      text-align: center;
      margin-top: 20px;
    }

    canvas {
      display: block;
      margin: 20px auto;
      border: 1px solid #ccc;
      background-color: white;
      touch-action: none;
    }

    .controls {
      text-align: center;
      margin-bottom: 20px;
    }

    .controls button {
      font-size: 16px;
      padding: 8px 12px;
      margin: 0 5px;
      border-radius: 4px;
      cursor: pointer;
    }

    .controls button.active {
      background-color: #ccc;
    }
  </style>
</head>
<body>
  <h1>Графический редактор</h1>
  <canvas id="canvas" width="800" height="400"></canvas>
  <div class="controls">
    <button id="drawButton" class="active">Рисовать</button>
    <button id="eraseButton">Ластик</button>
    <input type="color" id="colorPicker" value="#000000">
    <input type="range" id="thicknessSlider" min="1" max="20" value="5">
    <button id="clearButton">Очистить</button>
  </div>
</body>
<script>
  var canvas = document.getElementById('canvas');
  var context = canvas.getContext('2d');
  var isDrawing = true;
  var color = '#000000';
  var thickness = 5;

  canvas.addEventListener('mousedown', startDrawing);
  canvas.addEventListener('mousemove', draw);
  canvas.addEventListener('mouseup', stopDrawing);
  canvas.addEventListener('mouseleave', stopDrawing);

  canvas.addEventListener('touchstart', startDrawing);
  canvas.addEventListener('touchmove', draw);
  canvas.addEventListener('touchend', stopDrawing);

  var drawButton = document.getElementById('drawButton');
  var eraseButton = document.getElementById('eraseButton');
  var colorPicker = document.getElementById('colorPicker');
  var thicknessSlider = document.getElementById('thicknessSlider');
  var clearButton = document.getElementById('clearButton');

  drawButton.addEventListener('click', enableDrawing);
  eraseButton.addEventListener('click', enableErasing);
  colorPicker.addEventListener('change', changeColor);
  thicknessSlider.addEventListener('input', changeThickness);
  clearButton.addEventListener('click', clearCanvas);

  function startDrawing(event) {
    isDrawing = true;
    var x = event.clientX || event.touches[0].clientX;
    var y = event.clientY || event.touches[0].clientY;
    x -= canvas.offsetLeft;
    y -= canvas.offsetTop;
    context.beginPath();
    context.moveTo(x, y);
  }

  function draw(event) {
    if (!isDrawing) return;
    var x = event.clientX || event.touches[0].clientX;
    var y = event.clientY || event.touches[0].clientY;
    x -= canvas.offsetLeft;
    y -= canvas.offsetTop;
    context.lineTo(x, y);
    context.strokeStyle = isDrawing ? color : 'white';
    context.lineWidth = thickness;
    context.lineCap = 'round';
    context.lineJoin = 'round';
    context.stroke();
  }

  function stopDrawing() {
    isDrawing = false;
    context.closePath();
  }

  function enableDrawing() {
    isDrawing = true;
    drawButton.classList.add('active');
    eraseButton.classList.remove('active');
  }

  function enableErasing() {
    isDrawing = false;
    eraseButton.classList.add('active');
    drawButton.classList.remove('active');
  }

  function changeColor() {
    color = colorPicker.value;
  }

  function changeThickness() {
    thickness = thicknessSlider.value;
  }

  function clearCanvas() {
    context.clearRect(0, 0, canvas.width, canvas.height);
  }
</script>
</html>

