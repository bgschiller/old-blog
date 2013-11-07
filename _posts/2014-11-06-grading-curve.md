---
layout: post
title: Grading on a Curve
description: "Everyone's happy to hear that a test will be curved, but what does that even mean?"
category: blog
tags: [math, fun, teaching, grading]
image:
  feature: hogwarts-express.jpeg
---

When you curve a test, how do the scores actually change? You might decide that the top scorer (with 89/100, say) should get 100%, but do you add 11 points to everyone's score, or change the denominator to be 89? Maybe you want to drop a particularly hard question, but doesn't that hurt people who got it right?

I was all geared up to explore these questions, but as it happens someone [beat me to it](http://divisbyzero.com/2008/12/22/how-to-curve-an-exam-and-assign-grades/)! 

<script src="http://code.highcharts.com/highcharts.js"></script>
<script src="http://code.highcharts.com/modules/exporting.js"></script>

<div id="container" style="min-width: 310px; height: 400px; margin: 0 auto"></div>

<script type="text/javascript">

// Generate an Irwin–Hall distribution of 10 random variables.
var grades;
$.getJSON("/extras/grading_curve_scores.json", function(data) {
    grades = data;
    $(function () {
        $('#container').highcharts({
            chart: {
                type: 'area'
            },
            title: {
                text: 'Grading distributions before and after applying a curve'
            },
            xAxis: {
                labels: {
                    formatter: function() {
                        return this.value; // clean, unformatted number for year
                    }
                }
            },
            yAxis: {
                title: {
                    text: 'Number of Students'
                },
                labels: {
                    formatter: function() {
                        return this.value;
                    }
                }
            },
            tooltip: {
                pointFormat: 'Curving with {series.name}, <b>{point.y:,.0f}</b><br/>students received a grade of {point.x}%'
            },
            plotOptions: {
                area: {
                    pointStart: 0,
                    marker: {
                        enabled: false,
                        symbol: 'circle',
                        radius: 2,
                        states: {
                            hover: {
                                enabled: true
                            }
                        }
                    }
                }
            },
            series: [{
                name: '(uncurved)',
                data: [null, null, null, null, null, 6 , 11, 32, 110, 235, 369, 640,
                    1005, 1436, 2063, 3057, 4618, 6444, 9822, 15468, 20434, 24126,
                    27387, 29459, 31056, 31982, 32040, 31233, 29224, 27342, 26662,
                    26956, 27912, 28999, 28965, 27826, 25579, 25722, 24826, 24605,
                    24304, 23464, 23708, 24099, 24357, 24237, 24401, 24344, 23586,
                    22380, 21004, 17287, 14747, 13076, 12555, 12144, 11009, 10950,
                    10871, 10824, 10577, 10527, 10475, 10421, 10358, 10295, 10104 ]
            }, {
                name: 'Add to numerator',
                data: [null, null, null, null, null, null, null , null , null ,null,
                5, 25, 50, 120, 150, 200, 426, 660, 869, 1060, 1605, 2471, 3322,
                4238, 5221, 6129, 7089, 8339, 9399, 10538, 11643, 13092, 14478,
                15915, 17385, 19055, 21205, 23044, 25393, 27935, 30062, 32049,
                33952, 35804, 37431, 39197, 45000, 43000, 41000, 39000, 37000,
                35000, 33000, 31000, 29000, 27000, 25000, 24000, 23000, 22000,
                21000, 20000, 19000, 18000, 18000, 17000, 16000]
            }]
        });
    });
    

}
</script>