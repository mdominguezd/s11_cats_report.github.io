<div>
  <label for="website-select">Get items from this collection:</label>
  <select id="website-select" onchange="updateIframe()">
    <option value="">All collections</option>
    <option value="collections=FBLs">FBLs</option>
    <option value="collections=Elevation data">Elevation data</option>
  </select>
</div>

<div>
  <label for="AOI">Get items in this AOI:</label>
  <select id="AOI" onchange="updateIframe()">
    <option value="">Any</option>
    <option value="&bbox=0,0,20,20">Africa</option>
    <option value="&bbox=100,-1,103,2.5">SEA</option>
    <option value="&bbox=-110,-50,34,25">LAC</option>
  </select>
</div>

<iframe id="content-iframe" src="https://eoapi.satelligence.com/stac/search" width="100%" height="450px"></iframe>

<script>
  function updateIframe() {
    var selectBox = document.getElementById("website-select");
    var selectAOI = document.getElementById("AOI");
    var iframe = document.getElementById("content-iframe");
    iframe.src = "https://eoapi.satelligence.com/stac/search?" + selectBox.value + selectAOI.value;
  }
</script>