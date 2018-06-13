---
layout: home
title: REU2018 - Team 2
---
 <!-- Import Vega 3 & Vega-Lite 2 (does not have to be from CDN) -->
  <script src="https://cdn.jsdelivr.net/npm/vega@3"></script>
  <script src="https://cdn.jsdelivr.net/npm/vega-lite@2"></script>
  <!-- Import vega-embed -->
  <script src="https://cdn.jsdelivr.net/npm/vega-embed@3"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>

  <link rel="stylesheet" href="/css/style.css">
  <!-- Latest compiled and minified CSS -->
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">


<div class="container">
  <h1>REU2018 - Team 2</h1>
  <h2>Complex interactions between academic institutions</h2>
    <div class="slidecontainer">
      <p>Year: <span id="selectedYear"></span></p>
      <input type="range" min="1960" max="2010" value="1995" class="slider" id="yearRange">
    </div>
    <div class="row">
        <div class="col-xs-7" id="plot1"></div>
        <div class="col-xs-3" id="plot2"></div>
    </div>
</div>


<script type="text/javascript">
  // get local json
  var jsonplt1 = $.getJSON({"url":"/affil_radial_static.json",'async': false});
  var jsonplt2 = $.getJSON({"url":"/affil_radial_year.json",'async': false});

  var spec_plt1 = JSON.parse(jsonplt1.responseText);
  var spec_plt2 = JSON.parse(jsonplt2.responseText);

  // renders the initial plot
    opts = {"renderer": "svg", "actions": {"export": false,"source": false,"editor": false } }
  vegaEmbed('#plot1', spec_plt1, opts).then(function(result) {}).catch(console.error);
  vegaEmbed('#plot2', spec_plt2, opts).then(function(result) {}).catch(console.error);

  // selected year functionality
   $('#selectedYear').text($('#yearRange').val());
                $('#yearRange').on('input propertychange', function (){
                  $('#selectedYear').text(
                  $('#yearRange').val()
                ),
                  // when the year changes, redraw the entire plot
            spec_plt1.signals[3].value = $('#yearRange').val();
            spec_plt2.signals[2].value = $('#yearRange').val();
            vegaEmbed('#plot1', spec_plt1, opts).then(function(result) {}).catch(console.error);
            vegaEmbed('#plot2', spec_plt2, opts).then(function(result) {}).catch(console.error);           
  });

</script>
