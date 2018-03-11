---
layout: post
title:      "Getting started on your CLI Gem"
date:       2018-03-11 09:43:59 +0000
permalink:  getting_started_on_your_cli_gem
---

This is a step by step tutorial on getting started with your CLI gem, and how to do that using test-driven development.

## How?
The concept of test driven development is straightforward. You write out tests before you start writing your code. You then write code to get your tests passing. Let's start building out our gem. I want to make a gem that shows my books compared to my book clubs books and adds them for me. Let's start by creating a structure for the gem using bundler.

```
// ♥ bundle gem groupreads
Creating gem 'groupreads'...
MIT License enabled in config
Code of conduct enabled in config
      create  groupreads/Gemfile
      create  groupreads/lib/groupreads.rb
      create  groupreads/lib/groupreads/version.rb
      create  groupreads/groupreads.gemspec
      create  groupreads/Rakefile
      create  groupreads/README.md
      create  groupreads/bin/console
      create  groupreads/bin/setup
      create  groupreads/.gitignore
      create  groupreads/.travis.yml
      create  groupreads/.rspec
      create  groupreads/spec/spec_helper.rb
      create  groupreads/spec/groupreads_spec.rb
      create  groupreads/LICENSE.txt
      create  groupreads/CODE_OF_CONDUCT.md
Initializing git repo in /Users/roberthughes/src/sandbox/groupreads
[07:47:36] sandbox
// ♥  cd groupreads
```

Great. Oh, there's already a spec file. Let's try running our tests.

```
[07:50:57] groupreads
// ♥ rspec
/Users/roberthughes/.rvm/rubies/ruby-2.3.3/lib/ruby/site_ruby/2.3.0/bundler/rubygems_integration.rb:65:in `rescue in validate': The gemspec at /Users/roberthughes/src/sandbox/groupreads/groupreads.gemspec is not valid. Please fix this gemspec. (Gem::InvalidSpecificationException)
The validation error was '"FIXME" or "TODO" is not a description'
```

So, I need to edit the gemspec. It looks like it's on line 12 and 13.

```
  spec.summary       = %q{Compare your books to your group books.}
  spec.description   = %q{Compare your books to your book clubs books.}
```

Let's try that again.

```
// ♥ bundle exec rspec

Groupreads
  has a version number
  does something useful (FAILED - 1)

Failures:

  1) Groupreads does something useful
     Failure/Error: expect(false).to eq(true)

       expected: true
            got: false

       (compared using ==)
     # ./spec/groupreads_spec.rb:7:in `block (2 levels) in <top (required)>'

Finished in 0.0286 seconds (files took 0.14683 seconds to load)
2 examples, 1 failure

Failed examples:

rspec ./spec/groupreads_spec.rb:6 # Groupreads does something useful

[08:38:13] (master) groupreads
```

Awesome! Let's follow their suggestion. So, what do we want to do next? Let's start by making a group, starting with our test of course.

```
RSpec.describe Group do

  describe '#name' do
    it 'can be read and written to' do
      test_group = Group.new
      test_group.name = "Sword and Laser"
      expect(test_group.name).to eql("Sword and Laser")
    end
  end

end
```

So, how do you decide what goes in this test? The answer is simple. If I was using this code, how would I want to use it? What would I want to run, and what do I expect back from this test?

Run the tests.
```
// ♥ bundle exec rspec

An error occurred while loading ./spec/group_spec.rb.
Failure/Error:
  RSpec.describe Group do

    describe '#name' do
      it 'can be read and written to' do
        test_group = Group.new
        test_group.name = "Sword and Laser"
        expect(test_group.name).to eql("Sword and Laser")
      end
    end


NameError:
  uninitialized constant Group
# ./spec/group_spec.rb:1:in `<top (required)>'

Finished in 0.00027 seconds (files took 0.13815 seconds to load)
0 examples, 0 failures, 1 error occurred outside of examples
```
Great. Now we're getting on to the sort of thing you'll be seeing in the Learn labs. Let's make lib/group.rb.

```
class Group
end
```

And try running our tests.

```
// ♥ bundle exec rspec

An error occurred while loading ./spec/group_spec.rb.
Failure/Error:
  RSpec.describe Group do

    describe '#name' do
      it 'can be read and written to' do
        test_group = Group.new
        test_group.name = "Sword and Laser"
        expect(test_group.name).to eql("Sword and Laser")
      end
    end


NameError:
  uninitialized constant Group
# ./spec/group_spec.rb:1:in `<top (required)>'

Finished in 0.00024 seconds (files took 0.13006 seconds to load)
0 examples, 0 failures, 1 error occurred outside of examples
```

Oops. It looks like I missed the requiring the file. Let's add "require" to the rspec.

```
require 'group'

RSpec.describe Group do
...
```

And try again.

```
// ♥ bundle exec rspec

An error occurred while loading ./spec/group_spec.rb.
Failure/Error:
  RSpec.describe Group do

    describe '#name' do
      it 'can be read and written to' do
        test_group = Group.new
        test_group.name = "Sword and Laser"
        expect(test_group.name).to eql("Sword and Laser")
      end
    end


NameError:
  uninitialized constant Group
# ./spec/group_spec.rb:1:in `<top (required)>'

Finished in 0.00024 seconds (files took 0.13006 seconds to load)
0 examples, 0 failures, 1 error occurred outside of examples
```

That's better. Let's add the attribute accessor to lib/group.rb.

```
class Group
  attr_accessor :name
  
end
```

And test.

```
// ♥ bundle exec rspec

Group
  #name
    can be read and written to

Groupreads
  has a version number

Finished in 0.00503 seconds (files took 0.11312 seconds to load)
2 examples, 0 failures
```

Awesome. Time to add your next feature. What do you want your groups to do? How do you want to call that functionality? What output do you expect back?

## Why
So why do you want to develop like this? Isn't it just slowing me down? I could have written the above example in half the time without the test.

Further down the road, you may want to edit the functionality. You have the documentation provided by the tests to remind you of what you thought your requirements were at the time. You can use this documentation along with the increased understanding you have of your app to update the requirements, edit what you currently have to make it meet your new needs, and check that all the features you've added in the meantime haven't broken, or fix them if they have.

It helps keep you focused on what you need. I've gone through writing features for an application, then I've gone back later and realised that I never ended up using the functionality that I created. With test driven development, it helps keep me focused on what needs developing, and why that feature requires developing.

## Summary and other resources
A couple of excellent resources for test-driven development are:
Effective Testing with RSpec 3 - Made by the current developers of RSpec. It goes into a lot of the extra features.
[Bundler: How to create a Ruby gem](http://bundler.io/v1.16/guides/creating_gem.html)

Even if you don't use test-driven development now, keep it in mind, especially when you're at those painful moments where you feel completely lost in an error that seems to go all the way through your app. Test-driven development could save your sanity!
