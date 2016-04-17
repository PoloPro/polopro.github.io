---
layout: post
title: Tests are for Day Two
---

<h2>RSpec and Test-Driven Development</h2>

<ul>
<li>Day One</li>
<li>RSpec</li>
<ul>
<li>Getting Started</li>
<li>The Basic Layout of RSpec</li>
</ul>
</ul>


<h2>Day One</h2>

Every programmer knows the feeling of Day One. You may be waiting in line for coffee, or folding laundry, or meeting a friend for dinner. You're thinking about some project from work, maybe just complaining to yourself about an inconvenience. Suddenly, inspiration. A real eureka moment--a lightning bolt of clarity. 

Immediately you pull out your phone and begin writing furiously. Idea after idea flying onto your default notes app, mispellings left as tomorrow's problem. You need to write this down <em>now</em>. Your coffee, wrinkled laundry, or unfortunate friend is long forgotten. When you finally reach your computer, the rest of the day slides into a blur. One hand is diagramming while the other pours out beautiful, natural code. It's easier than speaking. As you stumble into bed, hours later, you're perfectly content with the world. 

That's Day One. Tests are for Day Two. 

You shuffle into work and look at what you wrote the day before. It's sloppy, rambling stuff that ends in the middle of a line. Your diagrams are unreadable scribbles, your notes autocorrected to gibberish. Picking up the pieces of your frenzied coding and laying out a plan forward--that's Day Two. 

<h2>RSpec</h2>

RSpec is, at its core, a testing library for Ruby. It's a Day Two tool. It's doubtless the most popular in use today, providing a rough approximation of natural language for test-writing.

To walk you through RSpec and test-driven development in general, I'll use a project I recently Day One'd: a reimplementation of NYC's subway countdown clocks using the MTA real-time API. 

<h3>Getting Started</h3>
To start writing tests for your project, you'll first need to install the RSpec gem in your working directory.

{% highlight ruby %}
gem install rspec
{% endhighlight %}

Once you have the gem, you'll need a spec file and a spec helper to work on. Rspec provides shell commands to generate the basics for you.

{% highlight ruby %}
rspec --init
{% endhighlight %}

With that command, you'll now have a hidden file named <em>.rspec</em> and a directory containing a default helper file: <em>spec/spec_helper.rb</em>.

Within the <em>spec/</em> directory that RSpec created for you, create a new file to hold your first tests. The typical naming convention is <em>name_spec.rb</em>, where <em>name</em> is the project or class you want to run tests on. Below is an example with my own spec file.

{% highlight ruby %}
.rspec
spec/
    spec_helper.rb
    mta_api_spec.rb
{% endhighlight %}

<h3>The Basic Layout of RSpec</h3>

<h4>Describe</h4>
An RSpec spec (aha) can basically be broken down into a series of <strong>describe</strong> blocks. These blocks model the structure of your program, usually matching your classes and methods. From my <em>mta_api_spec</em>:

{% highlight ruby %}
describe 'MTA Countdown Clock'
    describe 'API Interface'
        describe '#load_realtime_data'
        describe '#load_station_data'
        describe '#create_schedule'
    describe 'Schedule'
        describe '.create'
        describe '#next_n_arrivals' 
    describe 'Trip'
        describe '.create'
        describe '#passes_station?' 
    describe 'Stop'
{% endhighlight %}

 I've omitted some formatting to show the layout more clearly. 

 The first block, <em>MTA Countdown Clock</em>, is the project I'm testing. As your projects get more complicated and you develop more and more tests, you may want to break your testing into multiple spec files. For this relatively straightforward application, I'll limit my tests to a single file. 

 The second layer of describe blocks contain my classes. Although these classes are all interdependent in the final application, we want to isolate them for testing. An error in one test shouldn't carry over into any other test (this is called cascading failure).

 The third layer of describe blocks contain my methods.

{% highlight ruby %}
describe '.create'
describe '#next_n_arrivals'
{% endhighlight %}

Note the naming convention: instead of <em>'the schedule is created properly'</em>, I simply use <em>'.create'</em>. More descriptive test conditions will come in the <strong>it</strong> statements later. 

Also important to note is the prefix. A <strong>period</strong> indicates the method is a <strong>class method</strong>, while a <strong>octothorpe (#)</strong> indicates the method is an <strong>instance method</strong>.

<h4>The Tests</h4>

Describe blocks organize and provide context for your tests, but they don't test any functionality themselves. For that you need to use <strong>it</strong> blocks.

{% highlight ruby %}
describe 'API Interface' do
    describe '#load_realtime_data' do
      it 'loads gtfs data from the server without error' do
      end

      it 'returns an array of hashes' do
      end
    end
end
{% endhighlight %}



<ul>
  <!-- <li>What is RSpec?</li> -->
  <!-- <li>Getting started, rspec --init</li> -->
  <!-- <ol>Basic layout of Rspec -->
    <!-- <li>Describe</li> -->
    <li>It/do, It/not_do</li>
    <li>Expect (note spacing)</li>
    <!-- <li>Nesting describe blocks</li> -->
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


