// 1. Define the Southeast Asia region using the correct LSIB dataset path
var sea_region = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017')
  .filter(ee.Filter.eq('country_na', 'Philippines'));

// 2. Read in the Image Collection
var era5_heat_ic = ee.ImageCollection('projects/climate-engine-pro/assets/ce-era5-heat');

// 3. Define the visualization parameters (UTCI Celsius range typical for PH summer)
var temp_palette = ["#2166ac", "#67a9cf", "#d1e5f0", "#f7f7f7", "#fddbc7", "#ef8a62", "#b2182b"];
var vis_params = {min: 25, max: 42, palette: temp_palette};

// 4. Create a server-side list of years for the statistical trend
var yearsList = ee.List([1940, 1950, 1960, 1970, 1980, 1990, 2000, 2010, 2020]);
// JavaScript array used strictly for generating the UI map layers sequentially
var yearsArray = [1940, 1950, 1960, 1970, 1980, 1990, 2000, 2010, 2020];

var threshold = 32 + 273.15; // 32°C threshold in Kelvin

// --- SECTION A: CALCULATE STATS & GENERATE CHART ---
var decadalStats = yearsList.map(function(year) {
  var startDate = ee.Date.fromYMD(ee.Number(year), 3, 1);
  var endDate = startDate.advance(3, 'month');
  var summerCol = era5_heat_ic.filterBounds(sea_region).filterDate(startDate, endDate);
  
  var heatDays = summerCol.select('utci_mean')
    .map(function(img) { return img.gt(threshold); })
    .reduce(ee.Reducer.sum())
    .rename('summer_days_over_26');

  var stats = heatDays.reduceRegions({
    collection: sea_region,
    reducer: ee.Reducer.mean(),
    scale: 27830
  });

  return stats.map(function(f) {
    return f.set('year', year);
  });
});

var chartData = ee.FeatureCollection(decadalStats).flatten();

var chart = ui.Chart.feature.byFeature({
  features: chartData,
  xProperty: 'year',
  yProperties: ['mean']
}).setOptions({
  title: 'Decadal Trend: Avg Summer Days (Mar-May) UTCI > 26°C in the Philippines',
  hAxis: {title: 'Year', format: '####'},
  vAxis: {title: 'Avg Number of Summer Days'},
  lineWidth: 2,
  pointSize: 4,
  series: {0: {color: '#b2182b', label: 'Philippines'}}
});

print(chart);

// --- SECTION B: MULTI-DECADAL MAP VISUALIZATION ---
// Center the map on the Philippines
Map.centerObject(sea_region, 6);

// Use a JavaScript client-side loop to cleanly overlay map layers for every decade
yearsArray.forEach(function(year) {
  var startDate = ee.Date.fromYMD(year, 3, 1);
  var endDate = startDate.advance(3, 'month');
  
  // Calculate average daily maximum UTCI for that summer season
  var summerMean = era5_heat_ic.filterBounds(sea_region)
    .filterDate(startDate, endDate)
    .select('utci_max')
    .mean()
    .subtract(273.15) // Convert Kelvin to Celsius
    .clip(sea_region);
  
  // Add as a separate map layer (Unchecked/hidden by default to avoid lag, except 2020)
  var shownByDefault = (year === 2020); 
  Map.addLayer(
    summerMean, 
    vis_params, 
    'Mean Summer UTCI Max (' + year + ')', 
    shownByDefault
  );
});
