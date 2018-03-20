---
layout: post
title:      "Map Shorthand"
date:       2018-03-20 17:41:48 +0000
permalink:  map_shorthand
---


Something I found out about recently due to using Rubocop is a shorthand for a map.

Take the contrived example of our cheesecake instances.

```ruby
class CheeseCake
  attr_accessor :flavour, :make
end

cheesecakes = []
strawberry_cheesecake = CheeseCake.new
strawberry_cheesecake.flavour = 'strawberry'
strawberry_cheesecake.make = 'homemade'
cheesecakes << strawberry_cheesecake

raspberry_cheesecake = CheeseCake.new
raspberry_cheesecake.flavour = 'raspberry'
raspberry_cheesecake.make = 'tesco'
cheesecakes << raspberry_cheesecake

lemon_cheesecake = CheeseCake.new
lemon_cheesecake.flavour = 'lemon'
lemon_cheesecake.make = 'aldi'
cheesecakes << lemon_cheesecake

more_cheesecake = CheeseCake.new
more_cheesecake.flavour = 'lemon'
more_cheesecake.make = 'homemade'
cheesecakes << more_cheesecake
```

We want to look through the cheesecakes array of cheesecake instances to find out all the different cheesecake makes for our cheesecake extraordinaire ruby gem. I would usually have done the below, which would have got the answer fine.

```ruby
cheesecakes.map { |cheesecake| cheesecake.make }.uniq
```

But what if I told you there was a better way to do this common task? There is!

```ruby
cheesecakes.map(&:make).uniq
```

So now, when you're iterating over an array of instances to get an individual attribute, try the above.
