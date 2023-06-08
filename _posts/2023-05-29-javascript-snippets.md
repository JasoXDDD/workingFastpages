---
permalink: /javascriptTicket
---
<head>
    <script src="https://code.jquery.com/jquery-1.12.4.min.js"></script>
</head>
<h1 style="color:white; text-align: left;">Table</h1>
<style>
    #sample_style{
        width: 100%;
        color:white;
        border: 4px solid #808080;
    }
    canvas{
        margin-left: auto;
        margin-right: auto;
        display: block;
    }
</style>
<label>Time:</label>
<label id="time">30.00</label><br><br>
<canvas id="canvas" width="800" height="500" style="border:10px solid #000000;"></canvas>
<br><br>
<div id="menu" style="display:none">
<label>Name:</label>
<input type="text" id="name"><br><br>
<label>Score:</label>
<label id="score">0</label><br><br>
<button onclick="addEntry()">Submit</button>
</div>
<table id="table" style="width: 100%; color: #707070; border: 5px solid #909090; display:none">
  <tr>
    <th>Name</th>
    <th>Score</th>
  </tr>
  <tbody id="get">
  </tbody>
</table>
<button onclick="retry()">Retry</button>
<script>
    function partition(arr, l, m, r){
        var n1 = m - l + 1;
        var n2 = r - m;
        var L = new Array(n1);
        var R = new Array(n2);
        
        for (var i = 0; i < n1; i++)
            L[i] = arr[l + i];
        for (var j = 0; j < n2; j++)
            R[j] = arr[m + 1 + j];
        
        var i = 0;
        var j = 0;
        var k = l;
     
        while (i < n1 && j < n2) {
            if (L[i]["score"] <= R[j]["score"]) {
                arr[k] = L[i];
                i++;
            }
            else {
                arr[k] = R[j];
                j++;
            }
            k++;
        }
        while (i < n1) {
            arr[k] = L[i];
            i++;
            k++;
        }
        while (j < n2) {
            arr[k] = R[j];
            j++;
            k++;
        }
    }
    

    function mergeSort(arr,l, r){
        if(l>=r){
            return;
        }
        var m =l+ parseInt((r-l)/2);
        mergeSort(arr,l,m);
        mergeSort(arr,m+1,r);
        partition(arr,l,m,r);
    }

    let array = [];
    let stopped = false;
    

    function addEntry(){
        name = document.getElementById('name').value;
        score = document.getElementById("score").innerText;
        array.push({name,score});
        mergeSort(array,0,array.length-1);
        for (let i=0;i<array.length-1;i++){
            $('tr:last-child').remove();
        }
        array.forEach(function (record){
            var name = record["name"];
            var score = record["score"];
            var row = '<tr>' +
                '<td>' + name + '</td>' +
                '<td>' + score + '</td>' +
                '</tr>';
    
            $('#table').append(row);
        });
        document.getElementById("menu").style.display="none";
        document.getElementById("table").style.display="block"
    }

    function retry(){
        time = 30;
        score = 0;
        dotx=400;
        doty=250;
        started = false;
        stopped = false;
        document.getElementById("time").innerText = time.toFixed(2);
        document.getElementById("menu").style.display="none";
        document.getElementById("table").style.display="none";
    }

    function orb(x,y,color,rad){
        ctx.fillStyle = color;
        ctx.beginPath();
        ctx.arc(x, y, rad, 0, 2 * Math.PI, true);
        ctx.fill();
    }
    let c = document.getElementById("canvas");
    let ctx = c.getContext("2d");
    ctx.fillStyle = "rgb(140,200,140)";
    ctx.fillRect(0, 0, c.width, c.height);
    let dotx=400;
    let doty=250;
    orb(dotx,doty,"rgb(0,0,0)",10);
    let started = false;
    let tapped = [];
    let time = 30.00;
    let score = 0;
    let id = setInterval(() => {
        ctx.fillStyle = "rgb(140,200,140)";
        ctx.fillRect(0, 0, c.width, c.height);
        for (let i=tapped.length-1;i>=0;i--){
            orb(tapped[i][0],tapped[i][1],"rgb("+14*tapped[i][2].toString()+","+20*tapped[i][2].toString()+","+14*tapped[i][2].toString()+")",10+tapped[i][2]);
            tapped[i][2]+=1;
            if (tapped[i][2]==10){
                tapped.splice(i,1);
            }
        }
        orb(dotx,doty,"rgb(0,0,0)",10);
        if (started){
            time-=0.01;
            document.getElementById("time").innerText = time.toFixed(2);
        }
        if (time<=0){
            console.log("end");
            stopped = true;
            document.getElementById("menu").style.display="block";
            document.getElementById("score").innerText = score;
        }
    }, 10);
    c.addEventListener('mousedown', function (e) {
        // Get the target
        const target = e.target;
    
        // Get the bounding rectangle of target
        const rect = target.getBoundingClientRect();
    
        // Mouse position
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;
        console.log(x,y);
        if (!stopped){
            if (dotx<x&&x<dotx+20 && doty<y&&y<doty+20){
                tapped.push([dotx,doty,0]);
                console.log([dotx,doty,0]);
                dotx=10+Math.floor(Math.random()*780);
                doty=10+Math.floor(Math.random()*480);
                score+=1;
                if (!started){
                    started=true;
                }
            }
        }
    });
</script>