<head>
  <link rel="stylesheet" href="geo/style.css" />
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

<html>
<head>
<style>
  body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
  }
  .grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    max-width: 800px;
    margin: 0 auto;
  }
  .cell {
    border: 1px solid #ccc;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;
    font-weight: bold;
    background-color: #f5f5f5;
    color: #333;
    cursor: pointer;
  }
  .cell:hover {
    background-color: #e0e0e0;
  }
  .picture {
    width: 100%;
    height: 300px;
    background-size: cover;
    background-position: center;
  }
  .button {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
  }
  .button button {
    padding: 10px 20px;
    font-size: 18px;
    background-color: #4caf50;
    color: #fff;
    border: none;
    cursor: pointer;
  }
  .button button:hover {
    background-color: #45a049;
  }
  .text {
    margin-top: 20px;
    text-align: center;
    font-size: 20px;
    font-weight: bold;
  }
  canvas {
    width: 100%;
    height: 300px;
    margin-top: 20px;
    border: 1px solid #ccc;
  }
</style>
</head>
<body>
<div class="grid">
  <div id="a" class="cell"></div>
  <div id="b" class="cell"></div>
  <div id="c" class="cell"></div>
  <div id="d" class="cell"></div>
</div>
<div id="picture" class="picture"></div>
<div id="start" class="button">
  <button onclick="initialize()">Start Game</button>
</div>
<div id="text" class="text"></div>
<canvas id="bigmap"></canvas>

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
  };
  
  places = [
    ["Stoneranch", "dc", 502, 344],
    ["Watertower", "ba", 456, 501],
    ["Koala", "dd", 22, 456],
    ["DNHS Parking", "da", 167, 293]
  ];
  
  play = 0;
  pid1 = ""; //first square pin id to zoom out
  pid2 = ""; // smallest square pin id
  locx = 0; // location x value
  locy = 0; // location y value
  locname = "";
  letters = ["a", "b", "c", "d"];
  
  function initialize() {
    play = 1;
    let i = 0;
    while (i < 4) {
      const val = "url('geo/" + letters[i] + ".png')";
      document.getElementById(letters[i]).className = "cell";
      document.getElementById(letters[i]).style.backgroundImage = val;
      i += 1;
    }
  
    const j = Math.floor(Math.random() * places.length);
    locname = places[j][0];
    const lid = places[j][1];
    locx = places[j][2] + avals[lid][0];
    locy = places[j][3] + avals[lid][1];
  
    document.getElementById("picture").className = "picture";
    document.getElementById("picture").style.backgroundImage = "url('geo/" + locname + ".png')";
    document.getElementById("start").innerHTML = "";
    document.getElementById("start").remove();
  
    console.log(document.getElementById("picture").style.backgroundImage);
    console.log(locname);
    console.log(lid);
    console.log(locx);
    console.log(locy);
  }
  
  function button(id) {
    if (play === 0 || play === 2) {
      return;
    }
  
    let i = 0;
    let j = 0;
  
    if (document.getElementById("a").innerHTML.length === 0) {
      pid1 = document.getElementById(String(id)).innerHTML;
      console.log(pid1);
  
      while (i < 4) {
        document.getElementById(letters[i]).innerHTML = String(id) + letters[i];
        i += 1;
      }
  
      while (j < 4) {
        document.getElementById(letters[j]).style.backgroundImage = "url('geo/" + String(document.getElementById(letters[j]).innerHTML) + ".png')";
        console.log(document.getElementById(letters[j]).style.backgroundImage);
        j += 1;
      }
    } else {
      const x = document.getElementById(String(id)).innerHTML;
      pid2 = x; //pin id is set to smallest square division
  
      while (i < 4) {    
        document.getElementById(letters[i]).className = "cell";
        i += 1;
      }
  
      document.getElementById("picture").className = "picture";
      document.getElementById("picture").style.backgroundImage = "url('geo/r" + x + ".png')";
    }
  }
  
  function pin() {
    const eCell = document.getElementById("picture");
    eCell.addEventListener("click", end);
  }
  
  pin();
  
  function end(event) {
    if (play === 0 || play === 2) {
      return;
    }
  
    play = 2;
  
    const eCell = document.getElementById("picture");
    const eRect = eCell.getBoundingClientRect();
    const x = event.clientX - eRect.left;
    const y = event.clientY - eRect.top;
  
    const diffx = Math.abs(locx - (x + avals[pid2][0]));
    const diffy = Math.abs(locy - (y + avals[pid2][1]));
    const dist = Math.floor(Math.sqrt((diffx ** 2) + (diffy ** 2)) * 1.589);
  
    console.log("distance: " + String(dist) + " meters");
  
    document.getElementById("text").innerHTML = "You were " + String(dist) + " meters from the location";
    document.getElementById("picture").className = "picture";
    document.getElementById("bigmap").className = "cell";
    document.getElementById("bigmap").style.backgroundImage = "url('geo/bigmap.png')";
  
    const c = document.getElementById("bigmap");
    const ctx = c.getContext("2d");
  
    ctx.beginPath();
    ctx.moveTo(((x + avals[pid2][0]) / 9.36), ((y + avals[pid2][1])) / 18.72); //pin
    ctx.lineTo((locx / 9.36), (locy / 18.72)); //location
    ctx.strokeStyle = "#0000FF";
    ctx.stroke();
  }
  
  function unzoom() {
    if (play === 0 || play === 2) {
      return;
    } else if (document.getElementById("a").innerHTML.length === 0) { //if already zoomed out
      return;
    } else if (document.getElementById("a").className === "cell") { //if enlarged fully
      document.getElementById("picture").className = "picture";
  
      let i = 0;
      while (i < 4) {
        document.getElementById(letters[i]).className = "cell";
        document.getElementById(letters[i]).style.backgroundImage = "url('geo/" + String(document.getElementById(letters[i]).innerHTML) + ".png')";
        i += 1;
      }
    } else { //if enlarged once
      let i = 0;
      while (i < 4) {
        document.getElementById(letters[i]).innerHTML = String(letters[i]);
        document.getElementById(letters[i]).style.backgroundImage = "url('geo/" + String(letters[i]) + ".png')";
        i += 1;
      }
    }
  }
  
  document.onkeydown = function(evt) { //escape function
    evt = evt || window.event;
    if (evt.keyCode == 27) {
      unzoom();
    } 
  };
</script>
</body>
</html>
