<!DOCTYPE html>
<html>
<head>
  <title>Графический редактор</title>
  <style>
    canvas {
      border: 1px solid black;
      touch-action: none;
    }
  </style>
</head>
<body>
  <h1>Графический редактор</h1>
  <canvas id="canvas" width="500" height="500"></canvas>
  <div>
    <button onclick="changeColor('red')">Красный</button>
    <button onclick="changeColor('green')">Зеленый</button>
    <button onclick="changeColor('blue')">Синий</button>
    <button onclick="changeColor('black')">Черный</button>
  </div>
  <div>
    <button onclick="changeThickness(1)">Тонкая</button>
    <button onclick="changeThickness(5)">Средняя</button>
    <button onclick="changeThickness(10)">Толстая</button>
  </div>
  <div>
    <button onclick="enableEraser()">Ластик</button>
    <button onclick="disableEraser()">Рисовать</button>
  </div>
</body>
<script>
  var canvas = document.getElementById('canvas');
  var context = canvas.getContext('2d');
  var isDrawing = false;
  var isErasing = false;
  var color = 'black';
  var thickness = 1;

  canvas.addEventListener('touchstart', startDrawing);
  canvas.addEventListener('touchmove', draw);
  canvas.addEventListener('touchend', stopDrawing);

  function startDrawing(event) {
    isDrawing = true;
    var x = event.touches[0].clientX - canvas.offsetLeft;
    var y = event.touches[0].clientY - canvas.offsetTop;
    context.beginPath();
    context.moveTo(x, y);
  }

  function draw(event) {
   

