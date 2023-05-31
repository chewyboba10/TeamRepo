<html>
<head>
  <link rel="stylesheet" href="./geo/style.css" />
  <title>GeoGuesser</title>
  <style>
    body {
      /* Background image and styles */
    }
    /* Other CSS styles */
  </style>
</head>
<body>
  <div class="button-container">
    <button class="button" id="button" onclick="promptUsername()">Click To Play</button>
    <button class="button" onclick="reloadPage()">Restart</button>
  </div>
  <div class="container">
    <div class="board" id="board">
      <!-- Board cells and divisions -->
    </div>
    <div class="cell3" id="picture"></div>
    <div id="text"></div>
  </div>

<script>
  let avals = {
    // Coordinate values
  };

  let places = [
    // Places and their coordinates
  ];

  let play = 0;
  let pid1 = "";
  let pid2 = "";
  let locx = 0;
  let locy = 0;
  let locname = "";
  let letters = ["a", "b", "c", "d"];

  // Add event listener to listen for the "esc" key press
  document.addEventListener("keydown", function(event) {
    if (event.key === "Escape") {
      zoomOut();
    }
  });

  function promptUsername() {
    var username = prompt("Enter your username:");
    if (username !== null && username !== "") {
      console.log("Username entered:", username);
    } else {
      // Handle no username entered or canceled by the user
    }
    initialize();
  }

  function initialize() {
    play = 1;
    let i = 0;
    while (i < 4) {
      let val = "url('geo/" + letters[i] + ".png')";
      document.getElementById(letters[i]).className = "cell1";
      document.getElementById(letters[i]).style.backgroundImage = val;
      i += 1;
    }

    // Pick random place
    let j = Math.floor(Math.random() * places.length);
    locname = places[j][0];
    let lid = places[j][1];
    locx = places[j][2] + avals[lid][0];
    locy = places[j][3] + avals[lid][1];

    document.getElementById("picture").className = "cell4";
    document.getElementById("picture").style.backgroundImage = "url('geo/" + locname + ".png')";
    document.getElementById("button").remove();

    // Log relevant information
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
      pid2 = x;
      while (i < 4) {
        document.getElementById(letters[i]).className = "cell3";
        i += 1;
      }
      document.getElementById("e").className = "cell2";
      document.getElementById("e").style.backgroundImage = "url('geo/r" + x + ".png')";
    }
  }

  function end() {
    if (play == 0 || play == 2) {
      return;
    }
    play = 2;
    // Rest of the end function
  }

  function calculatePoints(distance) {
    // Calculation logic
  }

  function reloadPage() {
    location.reload();
  }

  function zoomOut() {
    if (play === 0 || play === 2) {
      return;
    }

    play = 0;
    pid1 = "";
    pid2 = "";
    locx = 0;
    locy = 0;
    locname = "";

    let i = 0;
    while (i < 4) {
      document.getElementById(letters[i]).innerHTML = letters[i];
      document.getElementById(letters[i]).className = "cell3";
      document.getElementById(letters[i]).style.backgroundImage = "";
      i += 1;
    }

    document.getElementById("e").className = "cell3";
    document.getElementById("e").style.backgroundImage = "";
    document.getElementById("bigmap").className = "cell3";
    document.getElementById("bigmap").style.backgroundImage = "";

    document.getElementById("text").innerHTML = "";

    let button = document.createElement("button");
    button.className = "button";
    button.innerHTML = "Click To Play";
    button.onclick = promptUsername;

    let buttonContainer = document.querySelector(".button-container");
    buttonContainer.innerHTML = "";
    buttonContainer.appendChild(button);
  }
</script>
</body>
</html>
