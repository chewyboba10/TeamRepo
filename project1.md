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
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
</head>
<body>
  <div class="button-container">
    <button class="button" id="username" onclick="promptUsername()">Click To Play</button>
    <button class="button" id="button" onclick="reloadPage()">Restart</button>
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
  <script>
    let username = "";
    let avals = {
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
    let places = [
      ["stoneranch", "dc", 502, 344],
      ["watertower", "ba", 456, 501],
      ["koala", "dd", 22, 456],
      ["dnhsparking", "da", 167, 293],
      ["spreckles", "ca", 439, 391],
      ["boysgirls", "dc", 427, 432],
      ["intersection", "dc", 561, 92],
      ["bball", "da", 511, 357],
      ["playground", "da", 495, 300]
    ];
    let play = 0;
    let pid1 = ""; //first square pin id to zoom out
    let pid2 = ""; // smallest square pin id
    let locx = 0; // location x value
    let locy = 0; //location y value
    let locname = "";
    let letters = ["a", "b", "c", "d"];    
    function promptUsername() {
      username = prompt("Enter your username:");
      if (username !== null && username !== "") {
        initialize(username);
      }
    }   
    function initialize(username) {
      play = 1;
      let i = 0;
      while (i < 4) {
        let val = "url('geo/" + letters[i] + ".png')";
        document.getElementById(letters[i]).className = "cell1";
        document.getElementById(letters[i]).style.backgroundImage = val;
        i += 1;
      }
      //pick random place
      let j = Math.floor(Math.random() * places.length);
      locname = places[j][0];
      let lid = places[j][1];
      locx = places[j][2] + avals[lid][0];
      locy = places[j][3] + avals[lid][1];
      document.getElementById("picture").className = "cell4";
      document.getElementById("picture").style.backgroundImage = "url('geo/" + locname + ".png')";
      document.getElementById("username").remove();
      console.log(document.getElementById("picture").style.backgroundImage);
      console.log(locname);
      console.log(lid);
      console.log(locx);
      console.log(locy);
    }    
    function button(id) {
      if (play == 0 || play == 2) {
        return;
      }
      let i = 0;
      let j = 0;
      if (document.getElementById("a").innerHTML.length == 1) {
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
        let x = document.getElementById(String(id)).innerHTML;
        pid2 = x; //pin id is set to smallest square division
        while (i < 4) {
          document.getElementById(letters[i]).className = "cell3";
          i += 1;
        }
        document.getElementById("e").className = "cell2";
        document.getElementById("e").style.backgroundImage = "url('geo/r" + x + ".png')";
      }
    }
    let points = 0;
    function end() {
      if (play == 0 || play == 2) {
        return;
      }
      play = 2;
      var eCell = document.getElementById("e");
      var eRect = eCell.getBoundingClientRect();
      var x = event.clientX - eRect.left;
      var y = event.clientY - eRect.top;
      let diffx = Math.abs(locx - (x + avals[pid2][0]));
      let diffy = Math.abs(locy - (y + avals[pid2][1]));
      let dist = Math.floor(Math.sqrt((diffx ** 2) + (diffy ** 2)) * 1.589);
      let points = calculatePoints(dist);
      console.log("distance: " + String(dist) + " meters");
      console.log("points: " + String(points));
      document.getElementById("text").innerHTML = "You were " + String(dist) + " meters from the location. Points: " + String(points);
      // Added code for sending post request to server with username and points
      document.getElementById("e").className = "cell3";
      document.getElementById("bigmap").className = "cell2";
      document.getElementById("bigmap").style.backgroundImage = "url('geo/bigmap.png')";
      let c = document.getElementById("bigmap");
      let ctx = c.getContext("2d");
      ctx.beginPath();
      ctx.moveTo(((x + avals[pid2][0]) / 9.36), ((y + avals[pid2][1])) / 18.72); //pin
      ctx.lineTo((locx / 9.36), (locy / 18.72)); //location
      ctx.strokeStyle = "#0000FF"
      ctx.stroke();
      const url = "https://ramen-kj.duckdns.org/api/geoguessr/";
      // Load games on page entry
      function create_game(){
        // Creating json for the game
        const body = {
            username: username, // Pass the username variable directly
            score: String(points)
        };
        //using the POST method
        const requestOptions = {
            method: 'POST',
            body: JSON.stringify(body),
            mode: 'cors',
            cache: 'default',
            //credentials: 'include',
            headers: {
                "content-type": "application/json",
                'Authorization': 'Bearer my-token',
            },
        };
        // URL for Create API
        // Fetch API call to the database to create a new game
        fetch(url, requestOptions)
          .then(response => {
            // trap error response from Web API
            if (response.status !== 200) {
              const errorMsg = 'Database create error: ' + response.status;
              console.log(errorMsg);
              return;
            }
            // response contains valid result
            response.json().then(data => {
                console.log(data);
            })
        })
      }  
      create_game();
    }    
    function calculatePoints(distance) {
      const basePoints = 1000;
      const maxDistance = 5000; // maximum distance for full points
      const minDistance = 100; // minimum distance for any points
      const penaltyFactor = 1.5; // factor to multiply the base points by for each meter beyond maxDistance 
      if (distance <= minDistance) {
        return basePoints;
      }
      if (distance >= maxDistance) {
        const penaltyPoints = Math.floor((distance - maxDistance) * penaltyFactor);
        return basePoints - penaltyPoints;
      }
      const range = maxDistance - minDistance;
      const scaledDistance = distance - minDistance;
      const points = basePoints - Math.floor((scaledDistance / range) * basePoints);
      return Math.floor(points / penaltyFactor);
    }   
    function unzoom() {
      if (document.getElementById("a").innerHTML.length == 1) { //if already zoomed out
        return
      }
      else if (document.getElementById("a").className == "cell3") { //if enlarged fully
        document.getElementById("e").className = "cell3"
        document.getElementById("bigmap").className = "cell3"
        let i = 0;
        while (i < 4) {
          document.getElementById(letters[i]).className = "cell1"
          i += 1
        }
        return
      }
      else {
        document.getElementById("e").className = "cell3"
        document.getElementById("bigmap").className = "cell3"
        let i = 0;
        while (i < 4) {
          document.getElementById(letters[i]).className = "cell2"
          i += 1
        }
        return
      }
    }    
    document.onkeydown = function(evt) { //escape function
      evt = evt || window.event;
      if (evt.keyCode == 27) {
          unzoom();
      } 
    };
    function reloadPage() {
      location.reload();
    }
  </script>
</body>
</html>
