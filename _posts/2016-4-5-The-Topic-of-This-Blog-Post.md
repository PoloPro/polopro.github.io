---
layout: post
title: The Topic of This Blog Post
subtitle: Subtitle Goes Here
---

Brian Ferris has written an excellent series of tutorials on using realtime GTFS data:

http://goo.gl/zS7ti
http://goo.gl/y1er7
http://goo.gl/odBJP
http://goo.gl/eTaag
http://goo.gl/onUpw


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


