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

  places = {
    "stoneranch1": ["dc", 502, 344],
    "watertower": ["ba", 456, 501],
    "koala": ["dd", 22, 456],
    "dnhsparking": ["da", 167, 293]
  }
  
  lid = ""

    letters = ["a", "b", "c", "d"]

    i = 0
    while (i < 4) {
      val = "url('" + letters[i] + ".png')"
      document.getElementById(letters[i]).style.backgroundImage = val
      i += 1
    }

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
            lid = x
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
      spotx = 1886
      spoty = 2402
      var eCell = document.getElementById("e");
      var eRect = eCell.getBoundingClientRect();
      
      var x = event.clientX - eRect.left;
      var y = event.clientY - eRect.top;

      diffx = Math.abs(spotx - (x + avals[lid][0]))
      diffy = Math.abs(spoty - (y + avals[lid][1]))

      dist = Math.sqrt((diffx ** 2) + (diffy ** 2)) * 1.589
      
      // Use the relative x and y coordinates as needed
      console.log("Relative Cursor position to cell 'e' - X: " + x + ", Y: " + y);
      console.log("distance: " + String(dist) + " meters")
    }


</script>
</html>