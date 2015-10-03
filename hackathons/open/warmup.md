# Warmup

Complete this warmup exercise to get an idea how to put all the different pieces
together to generate an end-to-end data analysis viz report.

<a name="top"/>
<div id="autonav"></div>

{% data src="../fcq/fcq.clean.json" %}
{% enddata %}

{% viz %}

{% title %}

What is the distribution of courses across colleges?

{% solution %}

var groups = _.groupBy(data, function(d){
    return d['CrsPBAColl']
})
var counts = _.sortBy(_.pairs(_.mapValues(groups, function(d){
	return d.length
	})), function(n){
		return n[1]
	}).reverse()
	
console.log(counts)

// TODO: modify the code below to produce a nice vertical bar charts

function computeX(d, i) {
    return i*60
}

function computeHeight(d, i) {
    return d[1]/10
}

function computeWidth(d, i) {
    return 60
}

function computeY(d, i) {
    return 400 - d[1]/10
}

function computeColor(d, i) {
    return 'red'
}
function computeLabel1(d,i){
	return d[0]
}
function computeLabel2(d,i){
	return d[1]
}

var viz = _.map(counts, function(d, i){
            return {
                x: computeX(d, i),
                y: computeY(d, i),
                height: computeHeight(d, i),
                width: computeWidth(d, i),
                color: computeColor(d, i),
				label1: computeLabel1(d,i),
				label2: computeLabel2(d,i)
			}
         })
console.log(viz)

var result = _.map(viz, function(d){
         // invoke the compiled template function on each viz data
         return template({d: d})
     })
return result.join('\n')

{% template %}

<g transform="translate(${d.x} ${d.y})">
    <rect         
         width="${d.width}"
         height="${d.height}"
         style="fill:${d.color};
                stroke-width:3;
                stroke:rgb(0,0,0)" />
<text transform="translate(20 -5)">
        ${d.label1}
    </text>
	<text transform="translate(20 -20)">
        ${d.label2}
    </text>
</g>


{% endviz %}
