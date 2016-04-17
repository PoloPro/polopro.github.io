---
layout: post
title: Tests are for Day Two
subtitle: RSpec and Test-Driven Development
---

<h3>Day One</h3>

Every programmer knows the feeling of Day One. You may be waiting in line for coffee, or folding laundry, or meeting a friend for dinner. You're thinking about some project from work, maybe just complaining to yourself about an inconvenience. Suddenly, inspiration. A real eureka moment--a lightning bolt of clarity. 

Immediately you pull out your phone and begin writing furiously. Idea after idea flying onto your default notes app, mispellings left as tomorrow's problem. You need to write this down <em>now</em>. Your coffee, wrinkled laundry, or unfortunate friend is long forgotten. When you finally reach your computer, the rest of the day slides into a blur. One hand is diagramming while the other pours out beautiful, natural code. It's easier than speaking. When you finally stumble into bed, hours later, you're perfectly content with the world. 

That's Day One. Tests are for Day Two. 

You shuffle into work and look at what you wrote the day before. It's sloppy, rambling stuff that ends in the middle of a line. Your diagrams are unreadable scribbles, your notes autocorrected to gibberish. Picking up the pieces of your frenzied coding and laying out a plan forward--that's Day Two. 

<h3>RSpec</h3>

RSpec is, at its core, a testing library for Ruby. It's doubtless the most popular in use today, providing a rough approximation of natural language for test-writing.

To walk you through RSpec and test-driven development in general, I'll use a project I recently Day One'd: a reimplementation of NYC's subway countdown clocks using the MTA real-time API. 

<h4>Getting Started</h4>
To start writing tests for your project, you'll first need to install the RSpec gem in your working directory.

{% highlight ruby %}
gem install rspec
{% endhighlight %}

Once you have the gem, you'll need a spec file and a spec helper to work on. Rspec provides shell commands to generate the basics for you.

{% highlight ruby %}
rspec --init
{% endhighlight %}

With that command, you'll now have a hidden file named '.rspec' and a 'spec/' directory containing a default 'spec_helper.rb' file.

Within the 'spec/' directory that RSpec created for you, create a new file to hold your first tests. The typical naming convention is 'name_spec.rb', where 'name' is the project or class you want to run tests on.

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

Ten lines of code, test. Ten lines of code, test. Refactor. A few small steps at a time, always with the end goal in mind. That's Day Two and every day after.





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


