---
layout: default
---

![](assets/boxplots.png)

# Tevfik Umut Dincer

Hi, I'm Tev. I'm a PhD student at UCLA in the Bioinformatics program, member of [Ernst lab](http://www.biolchem.ucla.edu/labs/ernst/). Check out my publications [here](http://www.ncbi.nlm.nih.gov/pubmed/?term=umut+dincer) and some projects I'm working on [here](https://github.com/udincer).

- email: <span style="unicode-bidi:bidi-override; direction: rtl;"> ude.alcu<span style="display:none">hello@there.com</span>@recnid </span>
- github: udincer

<div id="hello"></div>

<style>

    body {
        font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
    }

    .box {
        font: 0px sans-serif;
    }

    .box line,
    .box rect,
    .box circle {
        fill: #c9c9ff;
        stroke: #6666ff;
        stroke-width: 1.5px;
        opacity: 0.4;
    }

    .box .center {
        stroke-dasharray: 3,3;
    }

    .box .outlier {
        fill: none;
        stroke: #ccc;
    }

</style>

<script src="https://d3js.org/d3.v3.min.js"></script>
<script src="assets/box.js"></script>

<script>

    var margin = {top: 10, right: 10, bottom: 20, left: 10},
        width = 30 - margin.left - margin.right,
        height = 360 - margin.top - margin.bottom;

    var min = Infinity,
        max = -Infinity;

    var chart = d3.box()
        .whiskers(iqr(1.5))
        .width(width)
        .height(height);

    d3.csv("assets/data.csv", function(error, csv) {
        if (error) throw error;

        var data = [];

        csv.forEach(function(x) {
            var e = Math.floor(x.variable),
                s = Math.floor(x.value),
                d = data[e];
            if (!d) d = data[e] = [s];
            else d.push(s);
            if (s > max) max = s;
            if (s < min) min = s;
        });

        chart.domain([min, max]);

        var svg = d3.select("div#hello").selectAll("svg")
            .data(data)
            .enter().append("svg")
            .attr("class", "box")
            .attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.bottom + margin.top)
            .append("g")
            .attr("transform", "translate(" + margin.left + "," + margin.top + ")")
            .call(chart);

        setInterval(function() {
            svg.datum(randomize).call(chart.duration(1000));
        }, 2000);
    });

    function randomize(d) {
        if (!d.randomizer) d.randomizer = randomizer(d);
        return d.map(d.randomizer);
    }

    function randomizer(d) {
        var k = d3.max(d) * .1;
        return function(d) {
            return Math.max(min, Math.min(max, d + k * (Math.random() - .5)));
        };
    }

    // Returns a function to compute the interquartile range.
    function iqr(k) {
        return function(d, i) {
            var q1 = d.quartiles[0],
                q3 = d.quartiles[2],
                iqr = (q3 - q1) * k,
                i = -1,
                j = d.length;
            while (d[++i] < q1 - iqr);
            while (d[--j] > q3 + iqr);
            return [i, j];
        };
    }

</script>

<div id="hello"></div>

aaa