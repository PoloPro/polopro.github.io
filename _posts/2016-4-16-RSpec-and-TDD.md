---
layout: post
title: Tests are for Day Two
subtitle: RSpec and Test-Driven Development
---

Every programmer knows the feeling of a Day One. You may be waiting in line for coffee, or folding laundry, or meeting a friend for dinner. You're thinking about some project from work, maybe just complaining to yourself about an inconvenience. Suddenly, inspiration. A real eureka moment--a lightning bolt of clarity. 

Immediately you pull out your phone and begin writing furiously. Idea after idea flying onto your default notes app, mispellings left as tomorrow's problem. You need to write this down <em>now</em>. Your coffee, wrinkled laundry, or unfortunate friend is long forgotten. When you finally reach your computer, the rest of the day slides into a blur. One hand is diagramming while the other pours out beautiful, natural code. It's easier than speaking. When you finally stumble into bed, hours later, you're perfectly content with the world. 

That's Day One.

Tests are for Day Two. 

When you shuffle into work and look at what you wrote the day before--sloppy, rambling stuff that ends in the middle of a line. Your diagrams are unreadable scribbles, your notes autocorrected to gibberish. 



<ul>
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
  </ol>
  <li>Running Rspec, --fail-fast and -fd</li>
  <li>The style guide</li>
  <li>Where to find this MTA project</li>
  <li>Helpful resources and credit</li>
</ul>






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


