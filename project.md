<head>
    <link rel="stylesheet" href="./geo/style.css" />
  <title>GeoGuesser</title>
  <style>
    .button {
      background-color: #4169E1;
      color: white;
      padding: 12px 24px;
      font-size: 20px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
      display: block; 
      margin: 0 auto; 
      transition: background-color 0.3s ease; 
      justify-content: center;
      align-items: center;
    }
    .button:hover {
      background-color: #45a049; 
    }
  </style>
</head>
<body>
  <button class="button" id="button" onclick="initialize()">Click To Play</button>
  <div class="container">
    <div class="board" id="board">
      <div class="cell3" id="a" onclick="button('a')">a</div>
      <div class="cell3" id="b" onclick="button('b')">b</div>
      <div class="cell3" id="c" onclick="button('c')">c</div>
      <div class="cell3" id="d" onclick="button('d')">d</div>
      <div class="cell3" id="e" onclick="pin()"></div> <!--smallest division-->
      <canvas class="cell3" id="bigmap"></canvas>
    </div>
    <div class="cell3" id="picture"></div>
    <div id="text"></div>
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
  play = 0
  pid1 = "" //first square pin id to zoom out
  pid2 = "" // smallest square pin id
  locx = 0 // location x value
  locy = 0 //location y value
  locname = ""
    letters = ["a", "b", "c", "d"]
    function initialize() {
      play = 1
      i = 0
      while (i < 4) {
        val = "url('geo/" + letters[i] + ".png')"
        document.getElementById(letters[i]).className = "cell1"
        document.getElementById(letters[i]).style.backgroundImage = val
        i += 1
      }//pick random place
      j = Math.floor(Math.random() * (places.length))
      locname = places[j][0]
      lid = places[j][1]
      locx = places[j][2] + avals[lid][0]
      locy = places[j][3] + avals[lid][1]
      document.getElementById("picture").className = "cell4"
      document.getElementById("picture").style.backgroundImage = "url('geo/" + locname + ".png')"
      document.getElementById("start").innerHTML = ""
      document.getElementById("button").remove()
      console.log(document.getElementById("picture").style.backgroundImage)
      console.log(locname)
      console.log(lid)
      console.log(locx)
      console.log(locy)
    }
    function button(id) {
      if (play == 0 || play == 2) {
        return
      }
        i = 0
        j = 0
        if (document.getElementById("a").innerHTML.length == 1) {
          pid1 = document.getElementById(String(id)).innerHTML
          console.log(pid1)
            while (i < 4) {
                document.getElementById(letters[i]).innerHTML = String(id) + letters[i]
                i += 1
            }
            while (j < 4) {
              document.getElementById(letters[j]).style.backgroundImage = "url('geo/" + String(document.getElementById(letters[j]).innerHTML) + ".png')"
              console.log(document.getElementById(letters[j]).style.backgroundImage)
              j += 1
            }
        }
        else {
            x = document.getElementById(String(id)).innerHTML
            pid2 = x //pin id is set to smallest square division
            while (i < 4) {    
                document.getElementById(letters[i]).className = "cell3"
                i += 1
            }
            document.getElementById("e").className = "cell2"
            document.getElementById("e").style.backgroundImage = "url('geo/r" + x + ".png')"
        }
    }
    function pin() {
        var eCell = document.getElementById("e");
        eCell.addEventListener("click", end);
    }
    pin()
    function end(event) {
      if (play == 0 || play == 2) {
        return
      }
      play = 2
      var eCell = document.getElementById("e");
      var eRect = eCell.getBoundingClientRect();      
      var x = event.clientX - eRect.left;
      var y = event.clientY - eRect.top;
      diffx = Math.abs(locx - (x + avals[pid2][0]))
      diffy = Math.abs(locy - (y + avals[pid2][1]))
      dist = Math.floor(Math.sqrt((diffx ** 2) + (diffy ** 2)) * 1.589)      
      console.log("distance: " + String(dist) + " meters")
      document.getElementById("text").innerHTML = "you were " + String(dist) + " meters from the location"
      document.getElementById("e").className = "cell3"
      document.getElementById("bigmap").className = "cell2"
      document.getElementById("bigmap").style.backgroundImage = "url('geo/bigmap.png')"
      let c = document.getElementById("bigmap");
      let ctx = c.getContext("2d");
      ctx.beginPath();
      ctx.moveTo(((x + avals[pid2][0]) / 9.36), ((y + avals[pid2][1])) / 18.72); //pin
      ctx.lineTo((locx / 9.36), (locy / 18.72)); //location
      ctx.strokeStyle = "#0000FF"
      ctx.stroke();
    }
    function unzoom() {
      if (play == 0 || play == 2) {
        return
      }
      else if (document.getElementById("a").innerHTML.length == 1) { //if already zoomed out
        return
      }
      else if (document.getElementById("a").className == "cell3") { //if enlarged fully
        document.getElementById("e").className = "cell3"
        i = 0
        while (i < 4) {
          document.getElementById(letters[i]).className = "cell1"
          document.getElementById(letters[i]).style.backgroundImage = "url('geo/" + String(document.getElementById(letters[i]).innerHTML) + ".png')"
          i += 1
        }
      }
      else { //if enlarged once
        i = 0
        while (i < 4) {
          document.getElementById(letters[i]).innerHTML = String(letters[i])
          document.getElementById(letters[i]).style.backgroundImage = "url('geo/" + String(letters[i]) + ".png')"
          i += 1
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
