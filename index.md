<!DOCTYPE html>
<html lang="en">
<style>
    body {
        animation-name: color;
        animation-duration: 5s;
        animation-iteration-count: infinite;
    }    
    @keyframes color {
        0% {
            background-color: #8fd4ff;
        }
        50% {
            background-color: #5cb8f1;
        }
        100% {
            background-color: #8fd4ff;
        }
    }
</style>
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ramen KJ Geoguessrr</title>
    <link rel="stylesheet" href="./assets/style.css">
</head>
<script>
function getCookie(name) {
    const value = `; ${document.cookie}`;
    const parts = value.split(`; ${name}=`);
    if (parts.length === 2) return parts.pop().split(';').shift();
}

function authenticateLogin() {
    username = (getCookie("auth")).split(":")[0];
    key = (getCookie("auth")).split(":")[1];
    const body = {
        username: username,
        key: key
    };
    const read_options = {
        method: 'POST', // *GET, POST, PUT, DELETE, etc.
        mode: 'cors', // no-cors, *cors, same-origin
        cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
        body: JSON.stringify(body),
        credentials: 'omit', // include, *same-origin, omit
        headers: {
            'Content-Type': 'application/json'
        },
    };
    // fetch the data from API
    fetch("http://127.0.0.1:8570/api/leaderboard/wap", read_options)
        // response is a RESTful "promise" on any successful fetch
        .then(response => {
            // check for response errors
            if (response.status !== 200) {
                const errorMsg = 'Database read error: ' + response.status;
                return;
            }
            // valid response will have json data
            response.json().then(data => {
            if (data !== null) {
                if (data) {
                    document.getElementById("text5").innerHTML = username;
                }
            }
            else {
                // display invalid creds
            }
        })
    })
    // catch fetch errors (ie ACCESS to server blocked)
    .catch(err => {
      console.error(err);
    });
}

function getCookie(name) {
    let cookieArr = document.cookie.split("; ");
    
    for(let i = 0; i < cookieArr.length; i++) {
        let cookiePair = cookieArr[i].split("=");
        
        if(name == cookiePair[0]) {
            return decodeURIComponent(cookiePair[1]);
        }
    }
    return null;
}
function signOut() {
    var Cookies = document.cookie.split(';');
    for (var i = 0; i < Cookies.length; i++) {
       document.cookie = Cookies[i] + "=; expires="+ new Date(0).toUTCString();
    }
    location.reload()
}

</script>
<body style="background-color: #f4faf8;" onload="resize();authenticateLogin();">
    <div class="bg" id="bg">
        <div id="text1" class="menuText" style="top: 235px;" onclick="window.location = './project.html'">PLAY</div>
        <div id="text2" class="menuText" style="top: 340px;" onclick="window.location = './Stats.html'">STATS</div>
        <div id="text3" class="menuText" style="top: 445px;" onclick="window.location = './howtoplay.html'">TUTORIAL</div>
        <div id="text4" class="menuText" style="top: 550px;" onclick="window.location = './leaderboard.html'">LEADERBOARD</div>
        <img src = "./images/ramen.png" id="logo" width = "392" class = "home" style="top: 13px; left: 40px; opacity: 1;">
        <img src = "./images/RamenKJ.png" id="photos1" width = "640" height = "640" class = "home" style="top: 112px; left: 775px; opacity: 1;">
</body>

<script>
    var round = 1;
    // resizes position
    function dynamic_Positioning(idName, origionalTop, origionalLeft, size) {
        var element = document.getElementById(idName);
        var element_Top = size * origionalTop;
        element.style.top = element_Top + "px";
        var element_Left = size * origionalLeft;
        element.style.left = element_Left + "px";
    }

    // resizes text
    function dynamic_TextSize(className, origionalSize, size) {
        var element_Font = size * origionalSize;
        var elements = document.getElementsByClassName(className);
        for (var i = 0; i < elements.length; i++) {
            var element = elements[i];
            element.style.fontSize = element_Font + "px";
        }
    }
    //resizes image display
    function dynamic_imgSize(idName, origionalHeight, origionalWidth, origionalTop, origionalLeft, size) {
        var element = document.getElementById(idName);
        var element_Height = size * origionalHeight;
        element.style.width = element_Height + "px";
        var element_Width = size * origionalWidth;
        element.style.height = element_Width + "px";
        var element_Top = size * origionalTop;
        element.style.top = element_Top + "px";
        var element_Left = size * origionalLeft;
        element.style.left = element_Left + "px";
    }
    //resizes image size
    function dynamic_imgSizeRight(idName, origionalHeight, origionalWidth, origionalTop, origionalRight, size) {
        var element = document.getElementById(idName);
        var element_Height = size * origionalHeight;
        element.style.width = element_Height + "px";
        var element_Width = size * origionalWidth;
        element.style.height = element_Width + "px";
        var element_Top = size * origionalTop;
        element.style.top = element_Top + "px";
        var element_Right = size * origionalRight;
        element.style.right = element_Right + "px";
    }
    window.onscroll = function() {
        window.scrollTo(0, 0)
    };
    window.onresize = resize;
    function resize () {
        var current = window.innerWidth;
        var size = window.innerWidth/1536;

        // LOGO RESIZE
        dynamic_imgSize('logo', 392, 67, 13, 40, size)

        // MENU RESIZE
        dynamic_Positioning('text1', 235, 91, size)
        dynamic_Positioning('text2', 340, 91, size)
        dynamic_Positioning('text3', 445, 91, size)
        dynamic_Positioning('text4', 550, 91, size)
        dynamic_Positioning('text5', 22, 1175, size)
        dynamic_TextSize('menuText', 63, size)
        dynamic_Positioning('signOut', 800, 1270, size)
        dynamic_TextSize('signOut', 35, size)

        // SMALL IMAGE RESIZE
        dynamic_imgSize('profile', 80, 80, 20, 1436, size)
        dynamic_imgSize('photos2', 40, 40, 801, 1430, size)

        // IMAGE RESIZE
        dynamic_imgSize('photos1', 640, 640, 112, 775, size)

        // TRANSITION SCREEN
        dynamic_imgSize('difficultyPrompt', 734, 250, 307, 401, size)
        dynamic_imgSize('difficultyEasy', 175, 88, 450, 446, size)
        dynamic_imgSize('difficultyMedium', 175, 88, 450, 681, size)
        dynamic_imgSize('difficultyHard', 175, 88, 450, 915, size)
        dynamic_imgSize('difficultyClose', 21, 21, 336, 1084, size)
    }
</script>

</html>


<!-- Start of Website Content
<div class="index-header">
    <h1>RAMEN KJ</h1>
    <p>4S Ranch/Del Sur Geoguesser</p>
</div>

<!--About Our Team-->
<!-- <section class="team">
    <h1>CONTRIBUTORS</h1>
    <p>description of the team</p>
    <div class="row">
        <div class="team-col">
            <h1><a href="https://github.com/chewyboba10"><img src ="https://avatars.githubusercontent.com/u/111486836?v=4"></a></h1>
            <h3>Evan Aparri</h3>
            <p>Backend/DevOp</p>
        </div>
        <div class="team-col">
            <h1><a href="https://github.com/nsk1207"><img src ="https://avatars.githubusercontent.com/u/111671962?v=4"></a></h1>
            <h3>Nathan Kim</h3>
            <p>Frontend</p>
        </div>
        <div class="team-col">
            <h1><a href="https://github.com/mmaxwu"><img src ="https://avatars.githubusercontent.com/u/111472429?v=4"></a></h1>
            <h3>Max Wu</h3>
            <p>Frontend</p>
        </div>
    </div>
</section>
<section class="team1">
<div class="row">
    <div class="team-col">
        <h1><a href="https://github.com/RyanHaki"><img src ="https://avatars.githubusercontent.com/u/111466991?v=4"></a></h1>
        <h3>Ryan Hakimipour</h3>
        <p>Frontend</p>
    </div>
    <div class="team-col">
        <h1><a href="https://github.com/AniCricKet"><img src ="https://avatars.githubusercontent.com/u/91163802?v=4"></a></h1>
        <h3>Aniket Chakradeo</h3>
        <p>Frontend</p>
    </div>
    <div class="team-col">
        <h1><a href="https://github.com/kalanicabralomana"><img src ="https://avatars.githubusercontent.com/u/111479439?v=4"></a></h1>
        <h3>Kalani Cabral-Omana</h3>
        <p>Frontend</p>
    </div>
    <div class="team-col">
        <h1><a href="https://github.com/raisinbran25"><img src ="https://avatars.githubusercontent.com/u/83891698?v=4"></a></h1>
        <h3>Jaden Nguyen</h3>
        <p>Game Developer</p>
    </div>
</div>
</section> --> -->