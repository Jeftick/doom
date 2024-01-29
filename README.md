<html>
  <head>
    <title>Doom on the Web</title>
    <script type='text/javascript'>
      var Module = {
        noInitialRun: true,
      };

      var lastPrint = Date.now();
      function clearConsole() {
        (document.getElementById('output') || {}).innerHTML = '';
      }
      var print = function(text) {
        //return;
        text = text.replace('<b>', '').replace('</b>', '');
        var element = document.getElementById('output')
        var diff = Date.now() - lastPrint;
        if (!element) {
          alert(text);
        } else {
          element.value += text + '\n';
          // scroll down
          var len = element.textLength;
          element.setSelectionRange(len-1, len);
        }
        lastPrint = Date.now();
      }
    </script>
    <script src='doom.cc.js' type='text/javascript'></script>
    <script type='text/javascript'>
      var PRE_NOTICE = "(resize using ctrl-+)";

      var dts = 0, num = 0, lastHUD = Date.now(), totalTime = 0;
      var lastTime = Date.now();
      function showFPS() {
        var now = Date.now();
        var dt = (now - lastTime)/1000;
        lastTime = now;
        dts += dt;
        num++;
        totalTime += dt;
        if (now - lastHUD > 333) {
          //document.getElementById('fps').innerHTML = '<b>FPS: ' + Math.ceil(1/(dts/num)) + '</b>';
          document.getElementById('fps').innerHTML = '<small>' +
            PRE_NOTICE + ' <b>FPS: ' + ((1/(dts/num)).toString()+'     ').substr(0,5) + '</b>'
          + '</small>';
          lastHUD = now;
          dts = 0;
          num = 0;
        }
        if (totalTime > 10) PRE_NOTICE = '';
      }
      showFPS(0);

      var mainLoopIntervalId = null, logClearerIntervalId;
      function mainLoop() {
        try {
          Module._doOneIteration();
        } catch(e) {
          print('FAILURE in loop iteration: <b>' + e + '</b>');
          clearInterval(mainLoopIntervalId);
          clearInterval(logClearerIntervalId);
          try {
            Module.SDL_Quit();
          } catch(e){}
          print = function(){};
          throw e;
        }
        showFPS();
      }

      function onLoad() {
        clearConsole();

        // Pass canvas and context to the generated code, and do the actual run() here
        Module.canvas = document.getElementById('canvas');
        Module.ctx2D = Module.canvas.getContext('2d');
        if (!Module.ctx2D) {
          alert('Canvas not available :(');
          return;
        }
        Module.run();
        mainLoopIntervalId = setInterval(mainLoop, 1/100);

        logClearerIntervalId = setInterval(function() {
          if (Date.now() - lastPrint > 1500) {
            clearConsole();
          }
        }, 3000);
      }
    </script>
  <body onload='onLoad()' style='background-color: black; color: white'>
    <center>
      <div id='fps'>-</div>
      <canvas id='canvas' width='320' height='200' style='-moz-transform: scale(2, 2); -moz-transform-origin: top center; -webkit-transform: scale(2, 2); -webkit-transform-origin: top center; -o-transform: scale(2, 2); -o-transform-origin: top center; -ie-transform: scale(2, 2); -ie-transform-origin: top center; -transform: scale(2, 2); -transform-origin: top center; '></canvas>
      <div style="height: 200px"></div>
      <div style="height: 0.5em"></div>
      <textarea id='output' rows=5 cols=100></textarea>
      <br>
      This is <a href="http://en.wikipedia.org/wiki/Doom_%28video_game%29">Doom</a> by <a href="http://www.idsoftware.com">id Software</a> compiled to JavaScript using <a href="http://emscripten.org">Emscripten</a>.<small> <a href="details.html">(more details)</a></small>
    </center>
  </body>
</html>
