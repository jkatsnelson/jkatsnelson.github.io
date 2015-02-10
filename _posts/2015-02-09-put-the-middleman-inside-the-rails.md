---
layout: post
title: How to Put the Middleman Inside the Rails
feature-img: "img/sample_feature_img.png"
---

Let me tell you how to put the Middleman inside of the Rails.

##Middleman 3:

Go to your middleman repo and build the static site.

{% highlight bash %}
cd middleman/
middleman build
{% endhighlight %}

Take your dist folder and move it into /public in your rails app.

{% highlight bash %}
mv middleman/dist rails/public
{% endhighlight %}

Add this to your routes.rb in Rails:

{% highlight ruby %}

require 'rack/contrib'
app = lambda {|env| [404, {'Content-type' => 'text/plain'}, ['Not found']]}

Rails.application.routes.draw do
  mount Rack::TryStatic.new(app, root: 'public/target', urls: %w(/), try: %w(.html index.html /index.html)) => '/'
end

{% endhighlight %}

Run the Rails.

{% highlight bash %}
rails server
{% endhighlight %}