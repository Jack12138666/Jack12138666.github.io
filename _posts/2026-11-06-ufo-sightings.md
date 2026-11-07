---
layout: post
title: UFO Sightings Visualizations
description: Two interactive Altair visualizations of NUFORC UFO sighting reports
---

<script src="https://cdn.jsdelivr.net/npm/vega@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@6"></script>

<a href="https://github.com/UIUC-iSchool-DataViz/is445_data/blob/main/ufo-scrubbed-geocoded-time-standardized-00.csv" class="btn">The Data</a>
<a href="https://github.com/Jack12138666/Jack12138666.github.io/blob/main/python_notebooks/ufo_viz.ipynb" class="btn">The Analysis</a>

## Plot 1: UFO sightings over time and by reported shape

<div id="vis1"></div>
<script>
  vegaEmbed('#vis1', '{{ site.baseurl }}/assets/json/ufo_trend_by_shape.json');
</script>

This plot shows how many UFO sightings were reported to NUFORC each year, and which shapes people reported most often. The top chart is an area chart with year encoded as a quantitative (`Q`) variable on the x-axis (I used quantitative rather than temporal because the data was already aggregated to whole years) and the number of sightings (a quantitative aggregate, `sum(sightings)`) on the y-axis; an area mark makes the overall volume of reports over time easy to read. The bottom chart is a horizontal bar chart with the reported shape as a nominal (`N`) variable on the y-axis, sorted by count, and the number of sightings as a quantitative variable on the x-axis. I used a single blue color for both charts instead of a color scheme, because shape is already shown by position on the y-axis, so coloring by shape would repeat the same information. In the notebook, I parsed the date strings into Pandas timestamps (dropping a small number of rows with unparseable times such as "24:00"), kept reports from 1950 onward because earlier years have very few reports, grouped all but the 8 most common named shapes into an "other" category, and counted sightings per year and shape so the chart only embeds a small aggregated table.

## Plot 2: UFO sightings by US state

<div id="vis2"></div>
<script>
  vegaEmbed('#vis2', '{{ site.baseurl }}/assets/json/ufo_map_by_state.json');
</script>

This map shows the total number of UFO sightings reported in each US state. Each state is drawn with a geoshape mark (Albers USA projection), where the state boundaries are encoded with the geojson (`G`) encoding type, and the number of sightings is encoded as a quantitative (`Q`) variable mapped to color. The state name in the tooltip is a nominal (`N`) variable. I used the sequential "viridis" color scheme because the variable is an ordered count, viridis is perceptually uniform, and it remains readable for people with color vision deficiencies. I applied a square-root scale to the color, since a few very populous states have far more reports than the rest, and a linear scale would make most states look the same. States with no matching reports are shown in light gray. In the notebook, I kept only reports from the US, counted sightings per state and shape group, and added each state's FIPS code so the counts could be joined to the US states TopoJSON map. Note that these are raw counts, not adjusted for population, so states with more people naturally tend to have more reports.

## Interactivity

Both plots are interactive. In Plot 1, you can drag across the timeline to select a range of years, and the shape bar chart below updates to show only sightings from those years. This makes it possible to see whether the most commonly reported shapes changed over time, which is hard to see in a single static chart. In Plot 2, a dropdown menu lets you filter the map to one reported shape (or all shapes), and hovering over a state shows its name and exact count. This helps compare where different kinds of sightings are concentrated without crowding many maps onto the page.
