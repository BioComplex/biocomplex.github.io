---
layout: post
title: Affiliation Geolocation
---
 <!-- Latest compiled and minified CSS -->
<script
  src="https://code.jquery.com/jquery-3.1.1.min.js"
  integrity="sha256-hVVnYaiADRTO2PzUGmuLJr8BLUSjGIZsDYGmIJLv2b8="
  crossorigin="anonymous"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.0.0-beta.2.rc.2/leaflet.css"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.0.0-beta.2.rc.2/leaflet.js"></script>
<!-- <meta content='user-scalable=no, initial-scale=1, width=device-width' id='viewport' name='viewport'> -->
<link rel="stylesheet" href="css/examples.css"/>
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css" integrity="sha384-WskhaSGFgHYWDcbwN70/dfYBj47jz9qbsMId/iRN3ewGhXQFZCSftd1LZCfmhktB" crossorigin="anonymous">


<script src="js/PruneCluster.js"></script>
<script src="data/map500k.js"></script>
<div class="btn-toolbar" role="toolbar">
	<div class="btn-group mr-2" role="group">
		<button id="world" type="button" class="btn btn-info">World</button>
	</div>
	<div class="btn-group mr-2" role="group">
		<button id="boston" type="button" class="btn btn-info">Boston</button>
	</div>
	<div class="btn-group mr-2" role="group">
		<button id="london" type="button" class="btn btn-info">London</button>
	</div> 
	<div class="btn-group mr-2" role="group">
		<button id="tokyo" type="button" class="btn btn-info">Tokyo</button>
	</div>
	<div id="map"></div>
</div>
<script>
// viewport
// var viewPortScale = 1 / window.devicePixelRatio;
// $('#viewport').attr('content', 'user-scalable=no, initial-scale='+viewPortScale+', width=device-width');
var map = L.map("map", {
	attributionControl: false,
	zoomControl: false
}).setView(L.latLng(0.0, 0.0), 2);

$('#london').click(function(){
	map.setView(L.latLng(51.509, -0.118), 12);
});

$('#boston').click(function(){
	map.setView(L.latLng(42.36, -71.059), 12);
});

$('#tokyo').click(function(){
	map.setView(L.latLng(35.652, 139.835), 12);
});

$('#world').click(function(){
	map.setView(L.latLng(35.652, 139.835), 12);
});

L.tileLayer('http://{s}.tile.osm.org/{z}/{x}/{y}.png', {
        detectRetina: true,
        maxNativeZoom: 17
}).addTo(map);

var leafletView = new PruneClusterForLeaflet();
for (var i = 0, l = mapdata.length; i < l; ++i) {
	leafletView.RegisterMarker(new PruneCluster.Marker(mapdata[i][0],
		mapdata[i][1], {title: mapdata[i][2]}));
}

leafletView.PrepareLeafletMarker = function (marker, data) {
	if (marker.getPopup()) {
		marker.setPopupContent(data.title);
	} else {
		marker.bindPopup(data.title);
	}
};
map.addLayer(leafletView);
</script>