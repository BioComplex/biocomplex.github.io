---
title: REU2018 - Team 2
---
 <!-- Import Vega 3 & Vega-Lite 2 (does not have to be from CDN) -->
  <script src="https://cdn.jsdelivr.net/npm/vega@3"></script>
  <script src="https://cdn.jsdelivr.net/npm/vega-lite@2"></script>
  <!-- Import vega-embed -->
  <script src="https://cdn.jsdelivr.net/npm/vega-embed@3"></script>

  <link rel="stylesheet" href="/css/style.css">

## REU2018 - Team 2
# Complex interactions between academic institutions

<div class="slidecontainer">
  <input type="range" min="1960" max="2010" value="2005" class="slider" id="myRange">
</div>

<div id="vis">
	<div class="slidecontainer">
  <input type="range" min="1960" max="2010" value="2005" class="slider" id="myRange">
</div>
</div>

<script type="text/javascript">
  var spec = "/affil_radial_static.json";
  vegaEmbed('#vis', spec, {
                "renderer": "svg",
                "actions": {
                "export": false,
                "source": false,
                "editor": false
              } }).then(function(result) {
    // access view as result.view
  }).catch(console.error);
</script>