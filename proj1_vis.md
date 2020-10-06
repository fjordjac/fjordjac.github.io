
<!DOCTYPE html>
<html>
<head>
	<title>DataVis Project01</title>
        <script type="text/javascript" src="https://d3js.org/d3.v3.min.js"></script>
        <script type="text/javascript" src="js/javascript.js"></script>
        <link type="text/css" rel="stylesheet" href="css/stylesheet.css"/>
</head>
<body>
    <div class="pagetitle">Project 1: John Snow - Cholera Outbreak</div>
    <div> <a href="index.html">About the Project</a></div>
		<br/>
		<br/>
	<div>
		<svg id="main_map" width="500px" viewBox="0 0 500 500">
            <text id='label' x='5' y='20'>Street View</text>
        </svg>
		<svg id="linegraph" width="500px" viewBox="0 0 500 500">
			<text id='label' x='5' y='20'>New Deaths</text>
		</svg>
	</div>

	
	<script type="text/javascript">
        var main_map = d3.select("#main_map");
        var linegraph = d3.select("#linegraph");
		var m_size = 500;
		var axis_space = 30;
		var mm_xscale = d3.scale.linear();
		var mm_yscale = d3.scale.linear();
		var lg_xscale = d3.scale.linear();
		var lg_yscale = d3.scale.linear();
		var streets = [];
		var pumps = [];
		var death_days = [];
		var deaths = [];
        var pumpSize = 8;


		var lineFunction = d3.svg.line().x(function(d) { return mm_xscale(d.x); }).y(function(d) { return mm_yscale(d.y); }).interpolate("linear");
		

		d3.json("resource/streets.json", function(data) {
			streets = data;
		
			mm_xscale.domain([3,20]).range([0, m_size]);
			mm_yscale.domain([3,20]).range([m_size, 0]);
			
			for (var i=0; i < streets.length; i++) {
				main_map.append("path").attr("d", lineFunction(streets[i])).attr("class", "street");
            }
        d3.csv("resource/pumps.csv", function(data) {
            for (var i=0; i < data.length; i++) {
                pumps.push({x: data[i].x, y: data[i].y});
            }

            main_map.selectAll('.pump')
                .data(pumps)
                .enter()
                .append('rect')
                .attr('fill', 'green')
                .attr('width', pumpSize)
                .attr('height', pumpSize)
                .attr('x', function(d) { return mm_xscale(d.x) - pumpSize / 2; })
                .attr('y', function(d) { return mm_yscale(d.y) - pumpSize / 2; })             
                .append("title")
                .text("Pump");
			});
			d3.csv("resource/deaths_age_sex.csv", function(data) {
				for (var i=0; i < data.length; i++) {
					deaths.push(
						{
							x: data[i].x,
							y: data[i].y,
							age: +data[i].age,
							gender: +data[i].gender==1 ? "female" : "male"
						}
					);
				}
				d3.csv("resource/deathdays.csv", function(data) {
					var person = 0;
					for (var day = 0; day < data.length; day++) {
					
						var tot_count = +data[day].deaths;
						var male_count = 0;
						var female_count = 0;
						
						
						for (var i=0; i < tot_count; i++) {
							deaths[person].deathday = day;
							deaths[person].deathdate = data[day].date;
							if (deaths[person].gender == "female") {
								female_count++;
							} else {
								male_count++;
							}							
							person++;
						}
						
						death_days.push ({
							day: day,
							deathdate: data[day].date,
							male: male_count,
							female: female_count,
						});
						
					}

					lg_xscale.domain([0, death_days.length-1]).range([axis_space, m_size - axis_space]);
					lg_yscale.domain([0, 100]).range([m_size - axis_space, axis_space]);
						
					main_map.selectAll('.death')
                        .data(deaths)
                        .enter()
                        .append('circle')
                        .attr('class', function(d) {return "death " + d.gender + " " + "deathday" + d.deathday;})
                        .attr('cx', function(d) { return mm_xscale(d.x); })
                        .attr('cy', function(d) { return mm_yscale(d.y); })
                        .append("title")
                        .text(function(d) {return "Death: " + d.deathdate;});

					var male_line = d3.svg.line().x(function(d) { return lg_xscale(d.day); }).y(function(d) { return lg_yscale(d.male); });
                    var female_line = d3.svg.line().x(function(d) { return lg_xscale(d.day); }).y(function(d) { return lg_yscale(d.female); });
            
			        linegraph.append('path').attr('id', 'graphlinegendermale').attr('fill', 'none').attr('d', male_line(death_days));
			        linegraph.append('path').attr('id', 'graphlinegenderfemale').attr('fill', 'none').attr('d', female_line(death_days));
                    linegraph.append("circle").attr("cx",400).attr("cy",75).attr("r", 6).style("fill", "blue")
                    linegraph.append("circle").attr("cx",400).attr("cy",100).attr("r", 6).style("fill", "red")
                    linegraph.append("rect").attr("x",394).attr("y",119).attr("width", '12px').attr('height','12px').attr("fill", "green")
                    
                    linegraph.append("text").attr("x", 420).attr("y", 75).text("Male").style("font-size", "15px").attr("alignment-baseline","middle")
                    linegraph.append("text").attr("x", 420).attr("y", 100).text("Female").style("font-size", "15px").attr("alignment-baseline","middle")
                    linegraph.append("text").attr("x", 420).attr("y", 125).text("Pump House").style("font-size", "15px").attr("alignment-baseline","middle")
					var hover_width = m_size / (death_days.length - 1);

                    linegraph.selectAll("rect.userhover")
                        .data(death_days)
                        .enter()
                        .append("rect")
                        .attr("x", function(d, i) {return lg_xscale(i) - hover_width / 2;})
                        .attr("y", axis_space)
                        .attr('width', hover_width)
                        .attr("height", m_size)
                        .attr("fill", "black")
                        .attr("fill-opacity", "0")
                        .attr("stroke", "none")
                        .attr("stroke-width", "0")
                        .attr("id", function(d) { return d.deathdate; })
                        .attr("class", "userhover")
                        .on("mouseover", function(d) {
                            d3.selectAll('.death')
                                .filter(function(d2) {
                                    return d2.deathday > d.day;
                                })
                                .attr('visibility','hidden');
                        })
                        .on("mouseout", function(d) {
                            main_map.selectAll(".death").attr("visibility","visible");
                        });

                    var xAxis = d3.svg.axis().scale(lg_xscale).orient('bottom').tickFormat(function(d) {return death_days[d].deathdate;});
                    var yAxis = d3.svg.axis().scale(lg_yscale).orient('left');

                    linegraph.append('g').attr('class', 'axis').attr('transform', 'translate(0,' + (m_size - axis_space) + ')').call(xAxis);
                    linegraph.append('g').attr('class', 'axis').attr('transform', 'translate(' + axis_space + ',0)').call(yAxis);	
					main_map.selectAll('.death').attr('fill','none').attr('fill-opacity', 1).attr('r', '4');			
                    main_map.selectAll('.male').attr('fill','blue').attr('opacity','25%');
                    main_map.selectAll('.female').attr('fill','red').attr('opacity','25%');
                    main_map.selectAll('.death').attr('visibility','visible');

                    linegraph.select('#graphlinegendermale').attr('stroke','blue');
                    linegraph.select('#graphlinegenderfemale').attr('stroke','red');
				});
				
			});
		});
				
	</script>
</body>


</html>
