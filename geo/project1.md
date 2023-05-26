<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="./style.css" />
  <title>geoguessr</title>
</head>
<body>
  <div class="container">
    <div class="board" id="board">
      <div class="cell1" id="a" onclick="button('a')">a</div>
      <div class="cell1" id="b" onclick="button('b')">b</div>
      <div class="cell1" id="c" onclick="button('c')">c</div>
      <div class="cell1" id="d" onclick="button('d')">d</div>
      <div class="cell3" id="e" onclick="pin()"></div>
    </div>
    <div class="cell3" id="picture"></div>
  </div>
</body>
<script>
  avals = {
    "aa": [0,0],
    "ab": [702,0],
    "ac": [0,702],
    "ad": [702,702],
    "ba": [1404,0],
    "bb": [2106,0],
    "bc": [1404,702],
    "bd": [2106,702],
    "ca": [0,1404],
    "cb": [702,1404],
    "cc": [0,2106],
    "cd": [702,2106],
    "da": [1404,1404],
    "db": [2106,1404],
    "dc": [1404,2106],
    "dd": [2106,2106]
  }
  places = [
    ["stoneranch", "dc", 502, 344],
    ["watertower", "ba", 456, 501],
    ["koala", "dd", 22, 456],
    ["dnhsparking", "da", 167, 293]
  ]
  pid = "" // pin id
  locx = 0 // location x value
  locy = 0 //location y value
  locname = ""
    letters = ["a", "b", "c", "d"]
    function initialize() {
      i = 0
      while (i < 4) {
        val = "url('" + letters[i] + ".png')"
        document.getElementById(letters[i]).style.backgroundImage = val
        i += 1
      }
      //pick random place
      j = Math.floor(Math.random() * (places.length - 1));
      locname = places[j][0]
      lid = places[j][1]
      locx = places[j][2] + avals[lid][0]
      locy = places[j][3] + avals[lid][1]
      console.log(locname)
      console.log(lid)
      console.log(locx)
      console.log(locy)
    } 
    initialize()
    function button(id) {
        i = 0
        j = 0
        if (document.getElementById("a").innerHTML.length == 1) {
            while (i < 4) {
                document.getElementById(letters[i]).innerHTML = String(id) + letters[i]
                i += 1
            }
            while (j < 4) {
              document.getElementById(letters[j]).style.backgroundImage = "url('" + String(document.getElementById(letters[j]).innerHTML) + ".png')"
              console.log(document.getElementById(letters[j]).style.backgroundImage)
              j += 1
            }
        }
        else {
            x = document.getElementById(String(id)).innerHTML
            pid = x //pin id is set to smallest square division
            while (i < 4) {    
                document.getElementById(letters[i]).remove()
                i += 1
            }
            document.getElementById("e").className = "cell2"
            document.getElementById("e").style.backgroundImage = "url('r" + x + ".png')"
        }
    }
    function pin() {
        var eCell = document.getElementById("e");
        eCell.addEventListener("click", getPosition);
    }
    function getPosition(event) {
      var eCell = document.getElementById("e");
      var eRect = eCell.getBoundingClientRect();     
      var x = event.clientX - eRect.left;
      var y = event.clientY - eRect.top;
      diffx = Math.abs(locx - (x + avals[pid][0]))
      diffy = Math.abs(locy - (y + avals[pid][1]))
      dist = Math.sqrt((diffx ** 2) + (diffy ** 2)) * 1.589      
      console.log("distance: " + String(dist) + " meters")
    }


</script>
</html>

<html>
<head>
  <title>Stopwatch</title>
  <style>
    body {
      text-align: center;
      font-family: Arial, sans-serif;
    }
    h1 {
      color: #333;
    }
    #timer {
      font-size: 48px;
      margin-top: 20px;
    }
    button {
      font-size: 16px;
      padding: 10px 20px;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>Stopwatch</h1>
  <div id="timer">00:00:00</div>
  <button id="startButton">Start</button>
  <button id="stopButton">Stop</button>
  <button id="resetButton">Reset</button>

  <script>
    var startTime;
    var elapsedTime = 0;
    var timerInterval;

    function startTimer() {
      startTime = Date.now() - elapsedTime;
      timerInterval = setInterval(updateTimer, 10);
    }

    function stopTimer() {
      clearInterval(timerInterval);
    }

    function resetTimer() {
      clearInterval(timerInterval);
      elapsedTime = 0;
      updateTimer();
    }

    function updateTimer() {
      var currentTime = Date.now();
      elapsedTime = currentTime - startTime;
      var formattedTime = formatTime(elapsedTime);
      document.getElementById("timer").textContent = formattedTime;
    }

    function formatTime(time) {
      var milliseconds = Math.floor(time % 1000 / 10);
      var seconds = Math.floor(time / 1000 % 60);
      var minutes = Math.floor(time / 60000 % 60);
      var hours = Math.floor(time / 3600000 % 24);

      return (
        pad(hours, 2) + ":" +
        pad(minutes, 2) + ":" +
        pad(seconds, 2) + ":" +
        pad(milliseconds, 2)
      );
    }

    function pad(value, width) {
      var padding = "";
      while (padding.length < width - 1 && value < Math.pow(10, width - padding.length - 1)) {
        padding += "0";
      }
      return padding + value.toString();
    }

    document.getElementById("startButton").addEventListener("click", startTimer);
    document.getElementById("stopButton").addEventListener("click", stopTimer);
    document.getElementById("resetButton").addEventListener("click", resetTimer);

    // Start the timer immediately when the page is loaded
    startTimer();
  </script>
</body>
</html>

