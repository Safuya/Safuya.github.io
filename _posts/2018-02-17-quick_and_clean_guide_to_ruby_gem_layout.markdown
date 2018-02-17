---
layout: post
title:      "Quick and Clean Guide to Ruby Gem Layout"
date:       2018-02-17 11:30:35 +0000
permalink:  quick_and_clean_guide_to_ruby_gem_layout
---


This morning, I've been working on my Gem, and I ran into the problem of understanding the layout that [bundle recommends](http://bundler.io/v1.16/guides/creating_gem.html). Previously I was setting up my code so that all of my classes were in separate files (right), but were in not within the overall module (wrong).

So, how do you get started quickly? Using my gem named groupreads as an example, I will show you.

```
gem update bundle
bundle gem groupreads
```

Where do you start coding? Create your class file inside lib/groupreads/readers.rb.
```
module Groupreads
  class Readers
      attr_accessor :name
        
        def initialize(name)
          self.name = name
        end
        
    end
end
```

Add your new file as a requirement in lib/groupreads.rb
```
require 'groupreads/version'
require 'groupreads/readers'

module Groupreads
end
```

I'm still not sure if I'm organising my rspec files in the right way, so more will follow.
