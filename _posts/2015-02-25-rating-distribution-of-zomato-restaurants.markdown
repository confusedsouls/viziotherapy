---
layout:     post
title:      "Ratings of Restaurants around India"
subtitle:   "as if they matter."
date:       2015-02-25 12:00:00
author:     "Raghav Jalan"
header-img: "img/post-bg-01.jpg"
comments: true
---
<style type="text/css">

.bar {
  fill: #BC212D;
}

.bar:hover {
  fill: brown;
}
.heading {
  font: 40px Arial;
}
.axis {
  font: 10px Arial;
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

.d3-tip {
  line-height: 1;
  font-weight: bold;
  padding: 12px;
  background: rgba(0, 0, 0, 0.8);
  color: #fff;
  border-radius: 2px;
}

/* Creates a small triangle extender for the tooltip */
.d3-tip:after {
  box-sizing: border-box;
  display: inline;
  font-size: 10px;
  width: 100%;
  line-height: 1;
  color: rgba(0, 0, 0, 0.8);
  content: "\25BC";
  position: absolute;
  text-align: center;
}

/* Style northward tooltips differently */
.d3-tip.n:after {
  margin: -1px 0 0 0;
  top: 100%;
  left: 0;
}

</style>
<p>In my <a href="/viziotherapy/2015/02/24/best-food-cities-in-india">earlier post</a>, we saw how restaurants are distributed across our food-obsessed nation. In this post, we will see how <a href="https://www.zomato.com">zomato</a> users gave their reviews.</p>
<div id = "example"></div>

<script>

var margin = {top: 40, right: 20, bottom: 70, left: 25},
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

var tip = d3.tip()
    .attr('class', 'd3-tip')
    .offset([-10, 0])
    .html(function(d) {
      return "<strong>Share:</strong> <span style='color:red'>" + Math.round(d.share*100)/100  + " %</span>";
    });


var svg = d3.select("div#example").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

svg.call(tip);

d3.csv("/viziotherapy/data/rating-count.csv", type, function(error, data) {
  x.domain(data.map(function(d) { return d.rating; }));
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
      .style("fill", function(d) { return d.color;})
      .attr("x", function(d) { return x(d.rating); })
      .attr("width", x.rangeBand())
      .attr("y", function(d) { return y(d.count); })
      .attr("height", function(d) { return height - y(d.count); })
      .on('mouseover', tip.show)
      .on('mouseout', tip.hide)

});

  svg.append("text")
      .attr("x", 500)
      .attr("y", 20)
      .style("font-size", "40px")
      .style("fill", "#CB202D")
      .text("Total: 53,607");

function type(d) {
  d.count = +d.count;
  return d;
}
</script>
<p>I'm sure you noticed the huge red line in the leftmost corner with zero reviews. It means that 40% of zomato restaurants are not rated yet. This is a huge number. Most restaurants, around 50% lie in the range of 3 - 4. The count starts increasing in 2.2 and reaches the peak at 3.1, then it moves to a downhill path.</p>
<p>You can download the file I used for the above graph from <a href="/viziotherapy/data/rating-count.csv">here</a></p>