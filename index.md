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

# REU2018 - Team 2
## Complex interactions between academic institutions

<div class="slidecontainer">
	<p>Year: <span id="selectedYear"></span></p>
  <input type="range" min="1960" max="2010" value="1995" class="slider" id="yearRange">
</div>

<div id="vis">
</div>

<script type="text/javascript">
  // get local json
  var json = $.getJSON({"url":"/affil_radial_static.json",'async': false});
  var spec = JSON.parse(json.responseText);

  // renders the initial plot
  vegaEmbed('#vis', spec, {
                "renderer": "svg",
                "actions": {
                "export": false,
                "source": false,
                "editor": false
              } }).then(function(result) {


              }).catch(console.error);
	
  // selected year functionality
   $('#selectedYear').text($('#yearRange').val());
                $('#yearRange').on('input propertychange', function (){
                  $('#selectedYear').text(
                  $('#yearRange').val()
                ),
                  // when the year changes, redraw the entire plot
            spec.signals[3].value = $('#yearRange').val();
               vegaEmbed('#vis', spec, {
                "renderer": "svg",
                "actions": {
                "export": false,
                "source": false,
                "editor": false
              } }).then(function(result) {


              }).catch(console.error);
            
  });


	

	// JSON change functionality

    // access view as result.view
</script>