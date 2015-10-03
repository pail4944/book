
# Report

# Authors

This report is prepared by
* [Parker Illig](www.github.com/pail4944)
* [Andrew Krodinger](www.github.com/drewdinger)


<a name="top"/>
<div id="autonav"></div>



{% data src="../fcq/fcq.clean.json" %}
{% enddata %}

{% viz %}

{% title %}

# What class has the highest ratio of work per week to credit hours? (Parker Illig)

{% solution %}

var clean = _.filter(data, function(d){
	return d.Workload.Raw != ""
})
var result = _.groupBy(clean, function(d){
	var dept = d.Subject
	var num = d.Course
	var combine = dept +  " " + num	
	return combine
})
var classes = _.mapValues(result, function(d){
	var wk = _.pluck(d, 'Workload.Raw')
	var hr = _.sum(_.pluck(d, 'Hours'))/d.length
	return (_.sum(wk)/d.length)/ hr
})
classes = _.sortBy(_.pairs(classes), function(d){
	return d[1]
}).reverse()
classes = _.dropRightWhile(classes, function(d){
	return d[1] < 2.513
})

console.log(classes)

// TODO: modify the code below to produce a nice vertical bar charts

function computeX(d, i) {
    return 0
}

function computeHeight(d, i) {
    return 20
}

function computeWidth(d, i) {
    return d[1]*60
}

function computeY(d, i) {
    return i*20
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
var viz = _.map(classes, function(d, i){
            return {
                x: computeX(d, i),
                y: computeY(d, i),
                height: computeHeight(d, i),
                width: computeWidth(d, i),
                color: computeColor(d, i),
				label1: computeLabel1(d,i),
				label2: computeLabel2(d,i),
				width2: (computeWidth(d,i)+10)
			}
         })
console.log(viz)

var result = _.map(viz, function(d){
         // invoke the compiled template function on each viz data
         return template({d: d})
     })
return result.join('\n')

{% template %}

<g transform="translate(0 ${d.y})">
    <rect         
         width="${d.width}"
         height="20"
         style="fill:${d.color};
                stroke-width:3;
                stroke:rgb(0,0,0)" />
<text transform="translate(10 15)">
        ${d.label1}
    </text>
	<text transform="translate(${d.width2} 15)">
        ${d.label2}
    </text>
</g>

{% endviz %}

#What are the classes that have the most students barely pass with a C? (Andrew Krodinger)

{% lodash %}
var groups = _.groupBy(data, function(college){
    return college['CourseTitle']
})
var courseEnroll = _.mapValues(groups, function(a){
    return _.map(a, function(b){
        return b['PCT']['C']
    })
})
var enroll = _.mapValues(courseEnroll, function(enrolls){
    return _.sum(enrolls)
})
result = _.slice(_.sortByOrder(_.pairs(enroll),function(d){return d[1]},'desc'), 0, 10)
return result
{% endlodash %}

<table>
{% for n in result %}
    <tr>
        <td>{{n[0]}}</td>
        <td>{{n[1]}}</td>
    </tr>
{% endfor %}
</table>