---
layout: post
title: RSpec and Test-Driven Development
subtitle: MTA Subway Countdown Clock
---

<ul>
<li>What is test-driven development?</li>
<li>What is RSpec?</li>
<li>Getting started, rspec --init</li>
<ol>Basic layout of Rspec
  <li>Describe</li>
  <li>It/do, It/not_do</li>
  <li>Expect (note spacing)</li>
  <li>Nesting describe blocks</li>
</ol>
<ol>More advanced Rspec
  <li>Class v instance methods, naming</li>
  <li>Context blocks v describe blocks</li>
  <li>Before, after, and let</li>
  <li>When to use blocks and when to use parantheses</li>
<li>Running Rspec, --fail-fast and -fd</li>
<li>The style guide</li>
<li>Where to find this MTA project</li>
<li>Helpful resources and credit</li>






Brian Ferris has written an excellent series of tutorials on using realtime GTFS data:

http://goo.gl/zS7ti


{% highlight ruby %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}

More to come.


