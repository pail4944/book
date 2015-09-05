{% import '../../hackathons/classmates/data.html' as data %}

# Report

There are {{ data.comments.length }} students who gave a self-introduction. As a
class, we brainstormed and came up with a long list of further questions we can
ask based on this data. Our team chose to tackle on the following:

# How many Applied Math Majors are there?

{% lodash %}
var students = _.filter(data.comments, function(comment){
	var string = comment.body.toLowerCase()
	return _.includes(string, "applied math")
})
return _.size(students)
{% endlodash %}
The answer is {{result}}.

# How many people submitted after August 24th? (The first day of class

{% lodash %}
var students = _.filter(data.comments, function(comment){
	var string = comment.created_at
	string = string.split('T')[0].split('-')[2]
	if (string > 24){
	return true;
	}
})
return _.size(students)
{% endlodash %}
The answer is {{result}}.

# Who was the first person to say their favorite food was Mexican?

{% lodash %}
var student = _.find(data.comments, function(comment){
	var string = comment.body.toLowerCase()
	return _.includes(string, "mexican")
})
return student.user.login
{% endlodash %}
The name is {{result}}.

# How many people submitted before noon?

{% lodash %}
var students = _.filter(data.comments, function(comment){
	var string = comment.created_at
	string = string.split('T')[1].split(':')[0]
	if (string < 12){
	return true;
	}
})
return _.size(students)
{% endlodash %}
The answer is {{result}}.
