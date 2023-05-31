<html>
<head>
  <link rel="stylesheet" href="./geo/style.css" />
  <title>GeoGuesser</title>
  <style>
    body {
      background-image: url('geo/earth.png');
      background-repeat: no-repeat;
      background-size: cover;
    }
    .button-container {
      display: flex;
      justify-content: center;
      margin-bottom: 20px;
    }
    .button {
      justify-content: center;
      align-items: center;
      background-color: #4169E1;
      color: white;
      padding: 12px 24px;
      font-size: 20px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
      transition: background-color 0.3s ease;
    }
    .button:hover {
      background-color: #6495ED;
    }
    #text {
      color: #FFFFFF;
    }
  </style>
</head>
<body>
  <div class="button-container">
    <button class="button" id="button" onclick="promptUsername()">Click To Play</button>
    <button class="button" onclick="reloadPage()">Restart</button>
  </div>
  <div class="container">
    <div class="board" id="board">
      <div class="cell3" id="a" onclick="button('a')">a</div>
      <div class="cell3" id="b" onclick="button('b')">b</div>
      <div class="cell3" id="c" onclick="button('c')">c</div>
      <div class="cell3" id="d" onclick="button('d')">d</div>
      <div class="cell3" id="e" onclick="end()"></div> <!--smallest division-->
      <canvas class="cell3" id="bigmap"></canvas>
    </div>
    <div class="cell3" id="picture"></div>
    <div id="text"></div>
  </div>
</body>
<script>
  // Game constructor
  function GeoGuesser() {
    this.avals = {
      "aa": [0, 0],
      "ab": [702, 0],
      "ac": [0, 702],
      "ad": [702, 702],
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
    this.places = [
      ["stoneranch", "dc", 502, 344],
      ["watertower", "ba", 456, 501],
      ["koala", "dd", 22, 456],
      ["dnhsparking", "da", 167, 293]
    ];
    this.play = 0;
    this.pid1 = ""; //first square pin id to zoom out
    this.pid2 = ""; // smallest square pin id
    this.locx = 0; // location xvalue
    this.locy = 0; //location y value
    this.locname = "";
    this.letters = ["a", "b", "c", "d"];
  }
  // Game initialization method
  GeoGuesser.prototype.initialize = function() {
    this.play = 1;
    let i = 0;
    while (i < 4) {
      let val = "url('geo/" + this.letters[i] + ".png')";
      document.getElementById(this.letters[i]).className = "cell1";
      document.getElementById(this.letters[i]).style.backgroundImage = val;
      i += 1;
    }
    //pick random place
    let j = Math.floor(Math.random() * this.places.length);
    this.locname = this.places[j][0];
    let lid = this.places[j][1];
    this.locx = this.places[j][2] + this.avals[lid][0];
    this.locy = this.places[j][3] + this.avals[lid][1];
    document.getElementById("picture").className = "cell4";
    document.getElementById("picture").style.backgroundImage = "url('geo/" + this.locname + ".png')";
  };
  // Game button click method
  GeoGuesser.prototype.button = function(id) {
    if (this.play == 0 || this.play == 2) {
      return;
    }
    let i = 0;
    let j = 0;
    if (document.getElementById("a").innerHTML.length == 1) {
      this.pid1 = document.getElementById(String(id)).innerHTML;
      console.log(this.pid1);
      while (i < 4) {
        document.getElementById(this.letters[i]).innerHTML = String(id) + this.letters[i];
        i += 1;
      }
      while (j < 4) {
        document.getElementById(this.letters[j]).style.backgroundImage = "url('geo/" + String(document.getElementById(this.letters[j]).innerHTML) + ".png')";
        console.log(document.getElementById(this.letters[j]).style.backgroundImage);
        j += 1;
      }
    } else {
      let x = document.getElementById(String(id)).innerHTML;
      this.pid2 = x; //pin id is set to smallest square division
      while (i < 4) {
        document.getElementById(this.letters[i]).className = "cell3";
        i += 1;
      }
      document.getElementById("e").className = "cell2";
      document.getElementById("e").style.backgroundImage = "url('geo/r" + x + ".png')";
    }
  };
  // Game end method
  GeoGuesser.prototype.end = function(event) {
    if (this.play == 0 || this.play == 2) {
      return;
    }
    this.play = 2;
    var eCell = document.getElementById("e");
    var eRect = eCell.getBoundingClientRect();
    var x = event.clientX - eRect.left;
    var y = event.clientY - eRect.top;
    let diffx = Math.abs(this.locx - (x + this.avals[this.pid2][0]));
    let diffy = Math.abs(this.locy - (y + this.avals[this.pid2][1]));
    let dist = Math.floor(Math.sqrt((diffx ** 2) + (diffy ** 2)) * 1.589);
    let points = this.calculatePoints(dist);
    console.log("distance: " + String(dist) + " meters");
    console.log("points: " + String(points));
    document.getElementById("text").innerHTML = "You were " + String(dist) + " meters from the location. Points: " + String(points);
    document.getElementById("e").className = "cell3";
    document.getElementById("bigmap").className = "cell2";
    document.getElementById("bigmap").style.backgroundImage = "url('geo/bigmap.png')";
    var c = document.getElementById("bigmap");
    var ctx = c.getContext("2d");
    ctx.beginPath();
    ctx.arc(x + this.avals[this.pid2][0], y + this.avals[this.pid2][1], 5, 0, 2 * Math.PI);
    ctx.fillStyle = "red";
    ctx.fill();
    ctx.lineWidth = 3;
    ctx.strokeStyle = "red";
    ctx.stroke();
    localStorage.setItem("username", localStorage.getItem("username"));
    localStorage.setItem("points", points);
    // Redirect to another page
    window.location.href = "leaderboard.html";
  };
  // Game points calculation method
  GeoGuesser.prototype.calculatePoints = function(distance) {
    const basePoints = 1000;
    const maxDistance = 5000; // maximum distance for full points
    const minDistance = 100; // minimum distance for any points
    if (distance <= minDistance) {
      return basePoints;
    }
    if (distance >= maxDistance) {
      return 0;
    }
    const range = maxDistance - minDistance;
    const scaledDistance = distance - minDistance;
    const points = basePoints - Math.floor((scaledDistance / range) * basePoints);
    return points;
  };
  // Game start method
  function startGame() {
    var username = prompt("Enter your username:");
    if (username !== null && username !== "") {
      localStorage.setItem("username", username);
      console.log("Username entered:", username);
    } else {
      // No username entered or canceled by the user
      // Handle this case as per your requirements
    }
    var game = new GeoGuesser();
    game.initialize();
  }
  // Game reload method
  function reloadPage() {
    // Clear the stored data in localStorage
    localStorage.removeItem("username");
    localStorage.removeItem("points");
    location.reload();
  }
</script>
</html>
