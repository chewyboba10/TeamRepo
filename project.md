<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="./geo/style.css" />
  <title>GeoGuesser</title>
  <style>
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
      display: block; 
      margin: 0 auto; 
      transition: background-color 0.3s ease; 
    }
    .button:hover {
      background-color: #6495ED; 
    }
  </style>
</head>
<body>
  <button class="button" id="reloadButton" onclick="reloadSite()">Reload Site</button>
  <div class="container">
    <div class="board" id="board">
      <div class="cell3" id="a" onclick="button('a')">a</div>
      <div class="cell3" id="b" onclick="button('b')">b</div>
      <div class="cell3" id="c" onclick="button('c')">c</div>
      <div class="cell3" id="d" onclick="button('d')">d</div>
      <div class="cell3" id="e" onclick="pin()"></div>
      <canvas class="cell3" id="bigmap"></canvas>
    </div>
    <div class="cell3" id="picture"></div>
    <div id="text"></div>
  </div>

  <script>
    const avals = {
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

    const places = [
      ["stoneranch", "dc", 502, 344],
      ["watertower", "ba", 456, 501],
      ["koala", "dd", 22, 456],
      ["dnhsparking", "da", 167, 293]
    ];

    let play = 0;
    let pid1 = "";
    let pid2 = "";
    let locx = 0;
    let locy = 0;
    let locname = "";
    const letters = ["a", "b", "c", "d"];

    function initialize() {
      play = 1;

      for (let i = 0; i < 4; i++) {
        const val = `url('geo/${letters[i]}.png')`;
        const element = document.getElementById(letters[i]);
        element.className = "cell1";
        element.style.backgroundImage = val;
      }

      const j = Math.floor(Math.random() * places.length);
      locname = places[j][0];
      const lid = places[j][1];
      locx = places[j][2] + avals[lid][0];
      locy = places[j][3] + avals[lid][1];

      const pictureElement = document.getElementById("picture");
      pictureElement.className = "cell4";
      pictureElement.style.backgroundImage = `url('geo/${locname}.png')`;

      document.getElementById("start").innerHTML = "";
      document.getElementById("button").remove();

      console.log(pictureElement.style.backgroundImage);
      console.log(locname);
      console.log(lid);
      console.log(locx);
      console.log(locy);
    }

    function button(id) {
      if (play === 0 || play === 2) {
        return;
      }

      if (document.getElementById("a").innerHTML.length === 1) {
        pid1 = document.getElementById(String(id)).innerHTML;
        console.log(pid1);

        for (let i = 0; i < 4; i++) {
          document.getElementById(letters[i]).innerHTML = String(id) + letters[i];
          document.getElementById(letters[i]).style.backgroundImage = `url('geo/${document.getElementById(letters[i]).innerHTML}.png')`;
          console.log(document.getElementById(letters[i]).style.backgroundImage);
        }
      } else {
        const x = document.getElementById(String(id)).innerHTML;
        pid2 = x;

        for (let i = 0; i < 4; i++) {
          document.getElementById(letters[i]).className = "cell3";
        }

        const eCell = document.getElementById("e");
        eCell.className = "cell2";
        eCell.style.backgroundImage = `url('geo/r${x}.png')`;
      }
    }

    function pin() {
      const eCell = document.getElementById("e");
      eCell.addEventListener("click", end);
    }

    function end(event) {
      if (play === 0 || play === 2) {
        return;
      }

      play = 2;
      const eCell = document.getElementById("e");
      const eRect = eCell.getBoundingClientRect();      
      const x = event.clientX - eRect.left;
      const y = event.clientY - eRect.top;
      const diffx = Math.abs(locx - (x + avals[pid2][0]));
      const diffy = Math.abs(locy - (y + avals[pid2][1]));
      const dist = Math.floor(Math.sqrt((diffx ** 2) + (diffy ** 2)) * 1.589);

      console.log(`distance: ${dist} meters`);
      document.getElementById("text").innerHTML = `you were ${dist} meters from the location`;

      const bigmapElement = document.getElementById("bigmap");
      bigmapElement.className = "cell2";
      bigmapElement.style.backgroundImage = "url('geo/bigmap.png')";
      
      const c = document.getElementById("bigmap");
      const ctx = c.getContext("2d");
      ctx.beginPath();
      ctx.moveTo(((x + avals[pid2][0]) / 9.36), ((y + avals[pid2][1])) / 18.72);
      ctx.lineTo((locx / 9.36), (locy / 18.72));
      ctx.strokeStyle = "#0000FF";
      ctx.stroke();
    }

    function unzoom() {
      if (play === 0 || play === 2) {
        return;
      }

      if (document.getElementById("a").innerHTML.length === 1) {
        return;
      }

      if (document.getElementById("a").className === "cell1") {
        for (let i = 0; i < 4; i++) {
          document.getElementById(letters[i]).className = "cell2";
        }
      } else {
        for (let i = 0; i < 4; i++) {
          document.getElementById(letters[i]).className = "cell1";
        }
      }
    }

    function reloadSite() {
      location.reload();
    }
  </script>
</body>
</html>
