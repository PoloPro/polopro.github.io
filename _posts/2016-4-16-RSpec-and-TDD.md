---
layout: post
title: Tests are for Day Two
---

<h2>RSpec and Test-Driven Development</h2>
<hr>
<ul>
  <li><a href="#dayone">Day One</a></li>
  <li><a href="#rspec">RSpec</a></li>
  <ul>
    <li><a href="#started">Getting Started</a></li>
    <li><a href="#layout">The Basic Layout of RSpec</a></li>
    <li><a href="#tests">The Tests</a></li>
  </ul>
  <li><a href="#tdd">Test-Driven Development</a></li>
  <li><a href="#daytwo">Day Two</a></li>
  <li><a href="#links">Links</a></li>
</ul>

<h2 id="dayone">Day One</h2>
<hr>
Every programmer knows the feeling of Day One. You may be waiting in line for coffee, or folding laundry, or meeting a friend for dinner. You're thinking about some project from work, maybe just complaining to yourself about an inconvenience. Suddenly, inspiration. A real eureka moment--a lightning bolt of clarity. 

Immediately you pull out your phone and begin writing furiously. Idea after idea flying onto your default notes app, misspellings left as tomorrow's problem. You need to write this down <em>now</em>. Your coffee, wrinkled laundry, or unfortunate friend is long forgotten. When you finally reach your computer, the rest of the day slides into a blur. One hand is diagramming while the other pours out beautiful, natural code. It's easier than speaking. As you stumble into bed, hours later, you're perfectly content with the world. 

That's Day One. Tests are for Day Two. 

You shuffle into work and look at what you wrote the day before. It's sloppy, rambling stuff that ends in the middle of a line. Your diagrams are unreadable scribbles, your notes autocorrected to gibberish. Picking up the pieces of your frenzied coding and laying out a plan forward--that's Day Two.


<h2 id="rspec">RSpec</h2>
<hr>
RSpec is, at its core, a testing library for Ruby. It's a Day Two tool. It's doubtless the most popular in use today, providing a rough approximation of natural language for test-writing.

To walk you through RSpec and test-driven development in general, I'll use a project I recently Day One'd: a reimplementation of NYC's subway countdown clocks using the MTA real-time API. 

<h3 id="started">Getting Started</h3>
<hr>
To start writing tests for your project, you'll first need to install the RSpec gem in your working directory.

{% highlight ruby %}
gem install rspec
{% endhighlight %}

Once you have the gem, you'll need a spec file and a spec helper to work on. Rspec provides shell commands to generate the basics for you.

{% highlight ruby %}
rspec --init
{% endhighlight %}

With that command, you'll now have a hidden file named <em>.rspec</em> and a directory containing a default helper file: <em>spec/spec_helper.rb</em>.

Within the <em>spec/</em> directory that RSpec created for you, you can create a new file to hold your first tests. The typical naming convention is <em>name_spec.rb</em>, where <em>name</em> is the project or class you want to run tests on. Below is an example with my own spec file.

{% highlight ruby %}
.rspec
spec/
    spec_helper.rb
    mta_api_spec.rb
{% endhighlight %}

From here you can start pulling together your Day One work into a coherent project. However, that requires some basic knowledge of how to organize your RSpec.

<h3 id="layout">The Basic Layout of RSpec</h3>
<hr>
<h4 id="describe">Describe</h4>
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

 I've omitted some formatting here to show the layout more clearly (the full test file is available with the rest of the project on Github).

 The first block, <strong>'MTA Countdown Clock'</strong>, is the project I'm testing. As your projects get more complicated and you develop more and more tests, you may want to break your testing into multiple spec files. For this relatively straightforward application, I'll limit my tests to a single file. 

 The second layer of describe blocks contain my classes. Although these classes are all interdependent in the final application, we want to isolate them for testing. An error in one test shouldn't carry over into any other test (this is called cascading failure).

 The third layer of describe blocks contain my methods.

{% highlight ruby %}
describe '.create'
describe '#next_n_arrivals'
{% endhighlight %}

Note the naming convention: instead of <strong>'the schedule is created properly'</strong>, I simply use <strong>'.create'</strong>. More descriptive test conditions will come in the <strong>it</strong> statements later. 

Also important to note is the prefix. A <strong>period</strong> indicates the method is a <strong>class method</strong>, while a <strong>octothorpe (#)</strong> indicates the method is an <strong>instance method</strong>.

<h4 id="tests">The Tests</h4>

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

<strong>#load_realtime_data</strong> is the first method my program calls, so it's a good place to start testing. It blocks are where you'll list the expected behavior of your method or class, and they typically have longer, more natural descriptors. Let's take a look at the first block.

{% highlight ruby %}
it 'loads gtfs data from the server without error' do
    url = "http://www.info.mta.com"
    interface = MtaApiInterface.new(url)
    expect{ interface.load_realtime_data }.to_not raise_error
end
{% endhighlight %}

Ah, finally we've reached a test! Within the it block, we define a <strong>url</strong> variable, instantiate a <strong>MtaApiInterface</strong> object, and then provide an expectation for the test. The first two lines are just Ruby code, but the <strong>expect</strong> is specific to RSpec.

<strong>Expect</strong> statements resemble natural language, and can be interpreted fairly easily. For example, in the code above, we expect the block <strong>{ interface.load_realtime_data }</strong> to not raise an error. 

So, does <strong>expect</strong> always take a block as an argument?

{% highlight ruby %}
it 'returns an array of hashes' do
    url = "http://www.info.mta.com"
    interface = MtaApiInterface.new(url)
    expect(interface.load_realtime_data.sample).to be_a(Hash)
end
{% endhighlight %}

Two of our lines are unchanged (we'll discuss how we can stop that repetition with <strong>let</strong> statements), but <strong>expect</strong> now takes in a value argument instead of a block! Why?

When writing an expectation, use parentheses if you want to test the VALUE returned from the statement. In this case, we want to test if a random element of the <strong>#load_realtime_data</strong> return array is a Hash. In the previous example, the return value of the method wasn't really our concern. We wanted to see if the method itself raised an error. In that case, we use a block.

There is <a href="http://www.rubydoc.info/github/rspec/rspec-expectations/RSpec/Matchers" target="_blank">excellent documentation</a> available for both <strong>expect</strong> and its matchers, but there is a common error that you may run into with spacing. Usually Ruby is very forgiving with whitespace, and--style guide aside--organization is up to the programmer. When writing an expectation, however, make sure that there is no space between <strong>expect</strong> and the following argument!

{% highlight ruby %}
# bad
expect { apples_and_oranges }.to raise_error

# good
expect{ apples_and_oranges }.to change(basket.count).by(1)
{% endhighlight %}

This is because <strong>expect</strong> has the method <strong>.to</strong>, not the block or value itself. Don't let the natural language feel of RSpec trick you into putting a space after expect!

<h3 id="running">Running RSpec</h3>
<hr>
Now that you have some tests, it's time to run them. Thankfully, running RSpec tests involves just a single command.

{% highlight ruby %}
rspec
# or specify a filename
rspec mta_api_spec.rb
{% endhighlight %}

That's it! Run <strong>rspec</strong> in your project directory (the one containing the hidden <em>.rspec</em> file), and RSpec will run all the spec files in your <em>spec/</em> directory. (Note: if you want to run only the tests in a specific spec file, use the filename as an argument.)

Now, what does this output?


That's not very descriptive, beyond telling us that all of our tests are passing. Let's try it again with the <strong>-fd</strong> flag.

{% highlight ruby %}
rspec -fd
# or
rspec --format documentation
{% endhighlight %}


That's much better! We can even save this fully formatted output to a file, if we'd like. 

{% highlight ruby %}
rspec spec --format documentation --out rspec.txt
{% endhighlight %}

But with all the tests green, there's not much information to help us debug. Let's remove some code and see if we can get some tests to fail.


Now we can look at the error thrown during the failing test. Evidently there isn't a method called <strong>.id</strong> on that <strong>Stop</strong> object. But why is there a Stop object interfering with our <strong>#next_n_arrivals</strong> test? We only want to know if the behavior "<strong>returns an array of hashes</strong>" is true, the methods and functionality of the other object shouldn't be part of our test! This will be partially rectified with the RSpec concepts of <strong>before</strong>, <strong>after</strong>, and <strong>let</strong>. But, in general, you should "mock up" information that is generated externally. If you need a Stop object to test your method, manually create a new one with explicit values. You simultaneously partition your tests and have a single object state to work with.

Finally, if you have a large number of tests failing and want to view just a single failure for debugging, use the <strong>--fail-fast</strong> flag.

{% highlight ruby %}
rspec -fd --fail-fast
{% endhighlight %}

We'll now have a single, detailed error to debug, rather than a Terminal screen filled with red.

<h3 id="tdd">Test-Driven Development</h3>
<hr>
Basic RSpec firmly under your belt, you're well on the way to taming that unruly Day One code. With the ability to write tests you can now begin test-driven development, or TDD, which is the heart of Day Two (and every day after). Compared to the creative rambling of your first day, TDD is a precision instrument. It involves five major steps:

<ol>
<li><strong>Write a small test</strong></li>
<li><strong>Run the test and fail it</strong></li>
<li><strong>Code!</strong></li>
<li><strong>Run the test and pass it</strong></li>
<li><strong>Make your code pretty</strong></li>
</ol>

Each step is simple but necessary.

<ol>
<li><strong>Write a small test</strong></li>
  Write a test that covers just a single method or a dozen lines of code. You'll be repeating this list many times, and you want to slowly build functionality rather than rush headlong.
<li><strong>Run the test and fail it</strong></li>
  This is an important step! Your test shouldn't pass before you've coded a solution to it. If it does, your test is independent of your code, which is useless. If you're writing tests for legacy code (even if it's your own from the day before), continue adding tests, one small group at a time, until you reach the 'failing' edge of your code. This breadcrumb trail of tests will provide valuable information if later features are difficult to debug.
<li><strong>Code!</strong></li>
  Code a solution to the small test you've written. Don't worry about elegance or run-time yet, just write the most direct code to get that test green.
<li><strong>Run the test and pass it</strong></li>
  Everyone's favorite part! Run your test and pass it. Bask in the warmth of RSpec's beautiful green 'passing' tests. If you're failing at this step, go back to your code and fix it until your test passes!
<li><strong>Make your code pretty</strong></li>
  Now, with the test passing, go back to your solution. Is it well-structured? Is it efficient and cleanly formatted? Does it duplicate any previous functionality? Can it be abstracted? Refactor it until you're proud of it.
</ol>

The final step, perhaps the most important one: <strong>repeat the process</strong>! 

Test-driven development is built on iterative design, slowly building towards your final goal. These five steps aren't necessarily fun, nor are they particularly fast or beautiful. Instead, they require you to keep "what is my project supposed to <em>do</em>?"" in your mind at all times. If a branch of your code doesn't work towards your final product, you'll find yourself asking "why am I writing tests for this?" TDD encourages straightforward, almost single-minded design. It's not a solution for every project or every pitfall, but it does provide a process for developing sturdy, practical code. It's for Day Two.

<h3 id="advanced">Advanced RSpec</h3>
<hr>
Even in RSpec, there's always room for the Ruby mantra "don't repeat yourself." With the simple method of writing tests, you'll be repeating environment setup for every test that uses a common object or interface. <strong>Before</strong>, <strong>after</strong>, and <strong>let</strong> are RSpec blocks that help trim that repetitive code.

<strong>Before</strong> blocks are the most common. Within a describe block, you may have a set of variables or objects that need to be instantiated before every test. 

{% highlight ruby %}
describe 'API Interface' do
    before(:all) do
        @realtime_url = "http://www.realtime_url.com"
        @stations_url = "http://www.stations_url.com"
    end
    before(:each) do
        station = ["Grand Central", "Times Square"].sample
    end
{% endhighlight %}

Here we have examples of <strong>before(:all)</strong> and <strong>before(:each)</strong>. Again, RSpec's natural language design helps us interpret the functionality here. <strong>Before(:all)</strong> will run the block a single time before any of the tests (before all of the other code). Any instance variables defined here will be available in the test blocks. 

<strong>Before(:each)</strong> will instead run the block once before each test (before each of them). If there is both an <strong>(:all)</strong> and an <strong>(:each)</strong> block, the <strong>(:all)</strong> block will run first, followed by the <strong>(:each)</strong> block.

<strong>After</strong> functions much the same way, with reverse priority. <strong>After(:each)</strong> will run first, followed by <strong>after(:all). <strong>After</strong> blocks are usually used for manually clearing objects created by the test or in a <strong>before</strong> block.

The final pre-code method is <strong>let</strong>.

{% highlight ruby %}
describe 'API Interface' do
    before(:all) do
        @realtime_url = "http://www.realtime_url.com"
        @stations_url = "http://www.stations_url.com"
    end
    let(:interface) { MtaApiInterface.new(@realtime_url, @stations_url) }
{% endhighlight %}

<strong>Let</strong> is lazily evaluated, which means the code in the block isn't run until a test calls it. Once it's called within a spec, however, it's cached and doesn't need to be recreated for the next test. <strong>Let</strong> is typically used for expensive methods or object creation, where you don't want to rerun the code many times for each test. You would call a let block using the keyword you used to define it.

{% highlight ruby %}
interface.load_realtime_data
{% endhighlight %}

When looking through other RSpec tests, you may see blocks labeled <strong>context</strong>.

{% highlight ruby %}
describe 'API Interface' do
    context 'realtime data is available' do
        it '#load_realtime_data loads successfully' do
        end
    end
    context 'realtime data is not available' do
        it '#load_realtime_data throws an error' do
        end
    end
end
{% endhighlight %}

<strong>Context</strong> is an alias for <strong>describe</strong>. The functionality is precisely the same, but context is used in a different... context. Within a describe block, you may have two states that you wish to test your method's behavior under. In the above example, the GTFS realtime data may be unavailable due to server maintenance. In that case, I use a <strong>context</strong> block pair to define the expected behavior of my <strong>#load_realtime_data</strong> method under each condition. 

For detailed examples of these advanced Ruby techniques and proper styling of your spec, check out the <a href="https://github.com/howaboutwe/rspec-style-guide" target="_blank">RSpec Style Guide</a>.

<h2 id="daytwo">Day Two</h2>
<hr>
With RSpec, Day Two isn't so bad. 

Sure, you're deeply regretting some of your Day One choices--scrawling useless notes to yourself, neglecting your laundry, writing meandering volumes of unnecessary code. But Day One has its place. It's creative, volatile, and deeply rewarding stuff, perhaps the closest programmers ever get to artistic fervor. 

Day Two is about frameworks, diagrams, low-level tests, and isolating functionality. It's not as glamorous, it's not what you dreamed of when you chose to become a programmer. But Day Two has its rewards, too. The satisfaction of wresting a persistent bug from its hiding place. The deep contentment of scrolling through your refactored code. 

The simple pleasure of a green test at the end of a long day. That's Day Two.

<hr id="links">
To view my MTA countdown clock (with live updates for the 1/2/3/4/5/6/S subway lines), check out my public <a href="https://github.com/PoloPro/mta-countdown-clock" target="_blank">Github repository</a>.

Also check out <a href="http://rspec.info/" target="_blank">RSpec's documentation and videos</a> for a more detailed walkthrough of their testing framework.





