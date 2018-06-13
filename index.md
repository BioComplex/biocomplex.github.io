---
title: REU2018 - Team 2
---

 <!-- Import Vega 3 & Vega-Lite 2 (does not have to be from CDN) -->
  <script src="https://cdn.jsdelivr.net/npm/vega@3"></script>
  <script src="https://cdn.jsdelivr.net/npm/vega-lite@2"></script>
  <!-- Import vega-embed -->
  <script src="https://cdn.jsdelivr.net/npm/vega-embed@3"></script>

## REU2018 - Team 2
# Complex interactions between academic institutions

<div id="vis"></div>

<script type="text/javascript">
  var spec = "/affil_radial_static.json";
        vegaEmbed('#vis', spec, ["embed": {
                "renderer": "svg",
                "actions": {
                "export": false,
                "source": false,
                "editor": false
              }
</script>