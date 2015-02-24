---
layout:     post
title:      "Popular food cities in India"
subtitle:   "Cities and their Foodies"
date:       2015-02-24 12:00:00
author:     "Raghav Jalan"
header-img: "img/post-bg-01.jpg"
comments: true
---
<p><a href="https://www.zomato.com">Zomato</a>, with its recent acquisition of <a href="http://www.urbanspoon.com/">Urbanspoon</a>
is becoming a goto service for all the foodies in the world. Interestingly enough, I thought how many restaurants have they covered in India? This histogram explains this. 
</p>
<style type="text/css">

.bar {
  fill: #BC212D;
}

.bar:hover {
  fill: #6600CC;
}

.axis {
  font: 10px sans-serif;
}

.axis path,
.axis line {
  fill: none;
  stroke: #000;
  shape-rendering: crispEdges;
}

.x.axis path {
  display: none;
}

</style>

<div id = "example"></div>

<script src="http://d3js.org/d3.v3.min.js"></script>
<script>

var margin = {top: 20, right: 20, bottom: 70, left: 25},
    width = 800 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;

var x = d3.scale.ordinal()
    .rangeRoundBands([0, width], .1);

var y = d3.scale.linear()
    .range([height, 0]);

var xAxis = d3.svg.axis()
    .scale(x)
    .orient("bottom");

var yAxis = d3.svg.axis()
    .scale(y)
    .orient("left")
    .ticks(5, "s");

var svg = d3.select("div#example").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

d3.csv("/viziotherapy/city-count.csv", type, function(error, data) {
  x.domain(data.map(function(d) { return d.city; }));
  y.domain([0, d3.max(data, function(d) { return d.count; })]);

  svg.append("g")
      .attr("class", "x axis")
      .attr("transform", "translate(0," + height + ")")
      .call(xAxis)
      .selectAll("text")  
            .style("text-anchor", "end")
            .attr("dx", "-.8em")
            .attr("dy", ".15em")
            .attr("transform", function(d) {
                return "rotate(-65)" 
                });

  svg.append("g")
      .attr("class", "y axis")
      .call(yAxis)
    .append("text")
      .attr("transform", "rotate(65)")
      .attr("y", 6)
      .attr("dy", ".71em")
      .style("text-anchor", "end")
      .text("Count");

  svg.selectAll(".bar")
      .data(data)
    .enter().append("rect")
      .attr("class", "bar")
      .attr("x", function(d) { return x(d.city); })
      .attr("width", x.rangeBand())
      .attr("y", function(d) { return y(d.count); })
      .attr("height", function(d) { return height - y(d.count); })
      .append("svg:title")
      .text(function(d) {return d.city + " : " + d.count});

});

function type(d) {
  d.count = +d.count;
  return d;
}
</script>

<p>They have covered over 36 cities in India, indexing over 53,607 resataurants in their database. NCR region leads the herd with app. 10k restaurants, with Mumbai coming second with its impressive 9k score.</p>
<p>You can download the file I used for the above graph from <a href="/viziotherapy/city-count.csv">here</a></p>