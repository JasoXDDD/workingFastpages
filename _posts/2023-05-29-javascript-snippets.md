<h1 style="color:white; text-align: center;">Table</h1>
<style>
  #sample_style{
    width: 100%;
    color:white;
    border: 2px solid #009614;
  }
</style>
<table id="table" style="width: 100%; color:white; border: 2px solid #009614;">
  <tr>
    <th>Name</th>
    <th>Score</th>
  </tr>
  <tbody id="get">
  </tbody>
</table>

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
            if (L[i] <= R[j]) {
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

    var labelsRow = '<tr>' +
        '<th>Name</th>' +
        '<th>Score</th>' +
        '</tr>';
    $('#recentGames').append(labelsRow);

    let array = [{"name":"Jason","score":1000},
            {"name":"Jaso","score":1200}];
    mergeSort(array,0,array.length)
    console.log(array)

    array.forEach(function (record){
        var name = record.name;
        var score = record.score;
        var row = '<tr>' +
            '<td>' + name + '</td>' +
            '<td>' + score + '</td>' +
            '</tr>';

        $('#recentGames').append(row);
    });
</script>