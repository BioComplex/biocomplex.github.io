---
layout: home
title: REU2018 - Team 2
---
  <link rel="stylesheet" href="/css/style.css">
  <!-- Latest compiled and minified CSS -->
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
<!-- Latest compiled and minified CSS -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.3.1/semantic.min.css" integrity="sha256-oDCP2dNW17Y1QhBwQ+u2kLaKxoauWvIGks3a4as9QKs=" crossorigin="anonymous" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.3.1/components/search.min.css" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.3.1/components/dropdown.min.css" />
  <script
  src="https://code.jquery.com/jquery-3.1.1.min.js"
  integrity="sha256-hVVnYaiADRTO2PzUGmuLJr8BLUSjGIZsDYGmIJLv2b8="
  crossorigin="anonymous"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.3.1/semantic.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.3.1/components/search.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.3.1/components/dropdown.min.js"></script>

   <!-- Import Vega 3 & Vega-Lite 2 (does not have to be from CDN)-->
  <script src="https://cdn.jsdelivr.net/npm/vega@3"></script>
  <script src="https://cdn.jsdelivr.net/npm/vega-lite@2"></script>
  <script src="https://cdn.jsdelivr.net/npm/vega-embed@3"></script> 

<div class="container">
  <h1>REU2018 - Team 2</h1>
  <h2>Complex interactions between academic institutions</h2>
  <span id="select-span"></span>
<br>
  <div id="div-year" class="slidecontainer" style="display: none;">
      <input type="range" min="1960" max="2010" value="1995" class="slider" id="yearRange">
      <p>Year: <span id="selectedYear"></span></p>
    </div>
    <div class="row">
        <div class="col-lg-6 col-md-12 col-sm-24" id="id-afill"></div>
        <div class="col-lg-6 col-md-12 col-sm-24" id="id-year"></div>
    </div>
    <h2  class='invisible' id="titleACMclass" alt="Click to visit the ACM website"><a href="https://www.acm.org/publications/computing-classification-system/1998" target="_blank">ACM Classes</a> </h2>
  <br>
  <div class="invisible" id="acm-classes">
    <div class="wrapper">
      <div><span class="label label-default">A. General Literature</span></div>
      <div><span class="label label-info">B. Hardware</span></div>
      <div></div>
      <div><span class="label label-info">C. Computer Systems Organization</span></div>
      <div><span class="label label-default">D. Software</span></div>
      <div><span class="label label-info">E. Data</span></div>
      <div><span class="label label-default">F. Theory of Computation</span></div>
      <div><span class="label label-info">G. Mathematics of Computing</span></div>
      <div><span class="label label-default">H. Information Systems</span></div>
      <div><span class="label label-info">I. Computing Methodologies</span></div>
      <div><span class="label label-default">J. Computer Applications</span></div>
      <div><span class="label label-info">K. Computing Milieux</span></div>
    </div>
  </div>
</div>


<script type="text/javascript">
  var opts = {"renderer": "svg", "actions": {"export": false,"source": false,"editor": false } }
  var defaultSchool = 'Carnegie Mellon University';

  var jStatic = $.getJSON({"url":"/affil_radial_static.json"});
      jYear = $.getJSON({"url":"/affil_radial_year.json"});
      jAffil = $.getJSON({"url":"/data/unique_STDName_icu.json"});

      //create defered objets
      dfjAffil = $.when(jAffil);
      dfjYear = $.when(jYear);
      defjStatic = $.when(jStatic);


      
  // All the other plots rely on the affiliation JSON 
  dfjAffil.done(function() {
    
    var select = '<select multiple="" name="Search" class="ui fluid normal dropdown search" id="search-select">'

    $.each(JSON.parse(jAffil.responseText), function(key,entry) {
      select += '<option value="'+entry.AuthorAffiliation+'">'+entry.AuthorAffiliation+'</option>'
  });
    select += '</select>';

    $('#select-span').append(select);

      // Make the slide year functionality
    $('#selectedYear').text($('#yearRange').val());
    // show div
    $('#div-year').show();

    //creates the search
    $('#search-select').dropdown({
      'maxSelections': 3,
    });

    // sets the default school
    $('#search-select').dropdown('set selected',defaultSchool);
    $('#search-select').dropdown('set value',defaultSchool);
    $('#search-select').dropdown('setting','onAdd',function(addedValue, addedText){
      if(jplt1.signals[3].value == ""){
        jplt1.signals[3].value = addedText;
      } else if(jplt1.signals[4].value == ""){
        jplt1.signals[4].value = addedText;
      } else {
        jplt1.signals[5].value = addedText;
      }
      updatePlot(jplt1,'#id-afill',opts);

    });

    $('#search-select').dropdown('setting','onRemove',function(removedValue, removedText){
      if (jplt1.signals[3].value == removedValue){
        jplt1.signals[3].value = "";
      } else if (jplt1.signals[4].value == removedValue){
          jplt1.signals[4].value = "";
      } else {
        jplt1.signals[5].value = "";
      }
      
      updatePlot(jplt1,'#id-afill',opts);
    });

    // console.log($('#search-select').dropdown('get value'));


  });

    // Start year plot when select is ready
    dfjYear.done(function() {
      // get the initial year defined as 1995
      jplt2 = JSON.parse(jYear.responseText);
      jplt2.signals[2].value = $('#yearRange').val();
      updatePlot(jplt2,'#id-year',opts);
    });

    // finnally run when radial is ready
    defjStatic.done(function() {
      // get JSON
      jplt1 = JSON.parse(jStatic.responseText);
      // update year to correct value
      jplt1.signals[2].value = $('#yearRange').val();
      // update school to correct value
      jplt1.signals[3].value = defaultSchool;
      //updates the plot
      updatePlot(jplt1,'#id-afill',opts);
      $('#titleACMclass').show();
      $('#acm-classes').removeClass();
      $('#titleACMclass').removeClass();
    });

  // on year slide change
  $('#yearRange').on('input propertychange', function (){
    $('#selectedYear').text(
      $('#yearRange').val()),
      // when the year changes, redraw the entire plot
      // jplt1 = JSON.parse(jStatic.responseText);
      // jplt2 = JSON.parse(jYear.responseText);
      //spec_plt1.signals[3].value = $('#yearRange').val();
      jplt2.signals[2].value = $('#yearRange').val();
      jplt1.signals[2].value = $('#yearRange').val();
      //update plots on change
      //updatePlot(spec_plt1,'#id-afill',opts);
      updatePlot(jplt2,'#id-year',opts);
      updatePlot(jplt1,'#id-afill',opts);     
  }); 



  // $.getJSON({"url":"/affil_radial_year.json"}).done(function(result) {
  //   spec_plt2 = JSON.parse(result.responseText);
  //   console.log(spec_plt2);
  // });


  function updatePlot(json,divId,opts){
    vegaEmbed(divId, json, opts).then(function(result) {}).catch(console.error);
  }

</script>