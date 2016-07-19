Open the following link, then copy and paste the template code into DevTools. This code is the *YourVisual* starter code with the sample table removed. You will gradually add code to this template to build your visual.

https://gist.githubusercontent.com/deldersveld/dbfc69576c0c80bc80cd67ea1593fe10/raw/cd966f988f8c7bbdd1cdd07afd3982737629ad30/PowerBITemplate.ts

Add the following code just above *init*()
```
private svg: D3.Selection;
```
![](https://raw.githubusercontent.com/deldersveld/power-bi-training/master/custom-visuals/assets/001.PNG)

Add the following code within *init*(). This creates the initial SVG element and assigns a class to it for later reference.
```
var svg = this.svg = d3.select(options.element.get(0))
                .append('svg')
            	.attr("class", "tutorial-svg");
```
![](https://raw.githubusercontent.com/deldersveld/power-bi-training/master/custom-visuals/assets/002.PNG)

Click the *Compile and Run* button. Note that nothing visually appears in the preview area to the right. Right-click in the preview area and select *Inspect* or *Inspect Element*, and you should see your SVG element in developer tools.
![](https://raw.githubusercontent.com/deldersveld/power-bi-training/master/custom-visuals/assets/003.PNG)

Add the following code within *update*() just under the viewModel assignment. The first section adds the viewModel to the developer console if you need to look at its structure. This corresponds to the data from *converter*() that contains data from Power BI. The second section assigns the current width and height based on the dimensions of the preview pane.
```
console.log("Update");
console.log(viewModel);
            
var width = options.viewport.width;
var height = options.viewport.height;
```
![](https://raw.githubusercontent.com/deldersveld/power-bi-training/master/custom-visuals/assets/004.PNG)

Add the following code after the previous code to define your D3 scales. Scales in D3 help translate your data values to correspond with the available pixels on screen. Note that the x scale is ordinal while the y scale is linear. For a bar chart, the categories along the x axis are discrete, while the values along the y axis will be continuous.
```
var x = d3.scale.ordinal()
                .rangeRoundBands([0, width], .1);
            
var y = d3.scale.linear()
                .range([height, 0]);
```
![](https://raw.githubusercontent.com/deldersveld/power-bi-training/master/custom-visuals/assets/005.PNG)

Add the following code after the prior code to create your D3 axes. Note that they reference the x and y scales that you previously created. In addition, the x axis will appear on the bottom while the y axis will appear on the left. The y axis will also include 10 ticks.
```
var xAxis = d3.svg.axis()
                .scale(x)
                .orient("bottom");
            
var yAxis = d3.svg.axis()
                .scale(y)
                .orient("left")
                .ticks(10);
```
![](https://raw.githubusercontent.com/deldersveld/power-bi-training/master/custom-visuals/assets/006.PNG)

Add the following code 
```
var svg = this.svg;
svg.append("g")
		      .attr("class", "y axis")
		      .call(yAxis)
.append("text")
		      .attr("transform", "rotate(-90)")
		      .attr("y", 6)
		      .attr("dy", ".71em")
		      .style("text-anchor", "end")
		      .text("My Y Label");
```
![](https://raw.githubusercontent.com/deldersveld/power-bi-training/master/custom-visuals/assets/007.PNG)

Click *Compile + Run* and note that the y axis label now appears in the preview pane. If you Inspect the preview pane (right-click + Inspect), you will see a number of "g" elements when you expand the original svg element. Most of these correspond with the ten ticks you defined for the y axis, but you should also see the axis label and other elements.
![](https://raw.githubusercontent.com/deldersveld/power-bi-training/master/custom-visuals/assets/008.PNG)
