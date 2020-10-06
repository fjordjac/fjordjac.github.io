<html>
<head>
	<meta charset="utf-8">
	<title>DataVis Project01</title>
	<link type="text/css" rel="stylesheet" href="css/stylesheet.css"/>
	<style>
	</style>
	<script>
	document.querySelectorAll('style,link[rel="stylesheet"]').forEach(e => e.remove());
	</script>
</head>
<body>
	<div>
		<div class="title">Project 1: John Snow - Cholera Outbreak</div>
		<br/>
		<br/>
         <div> <a href="proj1_vis.html">Go to the Visulization</a></div>
        <br/>
		<br/>
		<div class="subtitle">About:</div>
		<div>
			<p>
				The purpose of this project is to replicate and possibly enhance the work of Dr. John Snow using JavaScript and D3. Dr. John Snow was a Physician interested in understanding the Broad Street Cholera Outbreak in London in 1854; Using the same data collected by Dr Snow I have recreated visualization using modern visualization techniques
                <br><br>
				More informationabout Dr. Snow's work during the 1854 Cholera Outbreak is found <a href="https://en.wikipedia.org/wiki/1854_Broad_Street_cholera_outbreak"> on Wikipedia</a>.
                <br>
                <img src="https://upload.wikimedia.org/wikipedia/commons/2/27/Snow-cholera-map-1.jpg"
					alt="Cholera Map"
					style="width:500px"
				/>
			</p>
				
		</div>
		<div class="subtitle">My Design Process</div>
		<div>
			<p>
                When designing my project i was very interested in keeping the interface simple and free of flashy animation and using consitent colors. My first idea was to have a large map with all the infections with a small graph underneath showing the death by day that when hovering over would update the map above.
                <br>
                <br>
                Realizing that the screen would be better formatted for deskop view, instead of a vertical view I decided the best format would be a side-by-side design with both visualizations being the same size. This made more sense, considering the data in the line graph would be just as important and interesting as the map.
                <br>
                <br>
                In the last itteration of my designs I settled on changing how i would display deaths on the map and the shape of pump houses. I didn't like how having larger circles would crowd eachother and eventually it would be impossible to see anything. So I made a change and used small dots the represented the dead.

            </p>
		</div>
        
        		<div class="subtitle">My Design Choices</div>
		<div>
            <p>
            Rationale of your design choices: This should be a rigorous explanation of the design choices you made. For example, why did you use color to encode a particular variable? Why did you arrange your charts in a particular way?
            </p>
	    
		</div>
	<div class="subtitle">Video Demo</div>
	<div>
	<br>
		        <video controls width="720">
  <source src="resource/vis_demo.mp4" type="video/mp4">
<div style="border: 1px solid black ; padding: 8px ;">
Sorry, your browser does not support the &lt;video&gt; tag used in this demo.
</div>
</video>
<br>
<div
        		<div class="subtitle">Questions and Conclusions</div>
		<div>
			<p>
            THe question I was curious about is the difference between male and female. Since the known issue was the infected water in the pumphouses, would gender play a role in fatality. 19th century England being a male dominated society i felt it would have made sense that more women would have been infected (since they would be home gathering water, using it to cook and clean, etc
            </p>
		</div>
	</div>
</body>


</html>
