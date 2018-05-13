---
layout: post
title:      "The Root of Absolutely Relatively Awesome Links"
date:       2018-05-13 07:58:07 +0000
permalink:  the_root_of_absolutely_relatively_awesome_links
---


There have been some questions in the Slack in the past on broken links. This post is going to try to give a succinct explanation of different link types, and how they relate to each other for your Sinatra project and going forward with none magic frameworks such as Rails. There are three different main types of links:
* Absolute
* Root
* Relative
So now, hopefully, this blog posts title makes sense.
## Absolute
Absolute links are the easiest to understand. You give the entire link. The below shows this in a pretty diagram.

![Absolute Links](https://s3-eu-west-1.amazonaws.com/nemene-share/absolute-path.png)

If you give the full link, you can be sure that in your specific circumstances, it's going to work right now. The problem is, as soon as you host it, the link may break. For example, http://localhost:3000/posts/23 is not the same as https://safuya.info/posts/23. Because of this, you should only use absolute links for links external to your site. A typical example of this is for externally hosted CSS frameworks.

```
<link rel="stylesheet" href="http://www.awesomestyles.com/style.css">
```
## Root
Root links are links that work from the main folder in your project. To use this, you need to put a / on the front of your link.
```
<a href="/posts/23">Click Bait!</a>
```
Assuming you're hosting this on https://www.safuya.info, you would then link to https://www.safuya.info/posts/23. These links are not too confusing, as they won't change the location as you move around the site.
A caveat with Sinatra and Rails though is that the root directory includes directories that you may not expect.
### Sinatra Root
Sinatra adds a default location to your root directory. The /public folder, which you would expect to be /public/amazing-image.png is accessible via /amazing-image.png. You can change the default for this if you would like in /app/controllers/application_controller.rb. The below example shows a typical application controller. set :public_folder, 'public' is setting the public folder to be in the root directory, and to allow routing to it. You can then place static files in there, such as CSS, javascript and images.
```
class ApplicationController < Sinatra::Base
  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
    enable :sessions
    set :session_secret, ENV['session_secret']
  end

  helpers Flash, Authorization
end
```
### Rails Root
In Rails, it's a bit more complicated. The main place to add your static files, such as CSS, javascript and images is the asset pipeline, located in /app/assets. There will be images, javascripts and stylesheets by default. If you want to add additional folders, such as fonts, you can do it in /config/initializers/assets.rb.
```
Rails.application.config.assets.paths << Rails.root.join('app', 'assets', 'fonts')
```
Rails will then put everything in the assets into a folder. When you use the below, which rails generates automatically for you:
```
<%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
```
It will give these assets a random identifier such as the below:
```
<link rel="stylesheet" media="all" href="/assets/application.self-0b34aa4a80f4198fbe85c7f3af8b8f79cee409ca96c3de76dd28dc08a75b0f73.css?body=1" data-turbolinks-track="reload" />
```
There are a few reasons Rails does this, but that's for another blog post.
## Relative Links
Relative links can be useful, but they are where people end up getting confused, especially with the above changes in Sinatra and Rails folder layouts with obfuscated configuration files. Relative links work based on your current location. If we have the below:
```
<a href="22">Last</a><a href="24">Next</a>
```
If you are hosting this from https://www.safuya.info/posts/23 it will link to https://www.safuya.info/posts/22 and https://www.safuya.info/posts/24. In this situation, this works pretty well. You don't need to type in the full path. It keeps your code shorter, and it's descriptive with the text.
The problem comes if you're using relative links for CSS in your main template. You're working on a Sinatra project, and you are working on your root page. You link to your stylesheet link so:
```
<link type="stylesheet" href="style.css">
```
In your root page, this correctly links to your style.css with the magic of relative links.
```
<link type="stylesheet" href="http://localhost:9393/style.css">
```
You then continue and build out a model, controller and view for posts. Your link then stops working. Why? Because it's now doing the below:
```
<link type="stylesheet" href="http://localhost:9393/posts/style.css">
```
You haven't got your style.css in your posts folder so that it won't load your styles.
To fix this, change to using a root link.
```
<link type="stylesheet" href="/style.css">
```
It will then always render correctly.
