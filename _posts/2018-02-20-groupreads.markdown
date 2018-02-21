---
layout: post
title:      "Groupreads"
date:       2018-02-20 12:19:02 -0500
permalink:  groupreads
---


My CLI gem is called [groupreads](https://www.github.com/safuya/groupreads). It uses the Goodreads API. It gets the users list of groups, all of the currently read books in those groups and compares them against the users to read and read lists. It then outputs a list of books to add to their to-read list.

## Layout
The project directories and files included are all based on Bundler's [gem creation guide](http://bundler.io/v1.16/guides/creating_gem.html).

### bin
This directory includes the console file, for use in development. It allows calling all of the methods outside of the program itself. I did not use this file, instead always using the spec files along with binding.pry for debugging. The setup file is a bash script for running the install of all the required gems.

### exe
groupreads is the file that loads the CLI from the lib folder.

### feature
The feature directory contains support/setup.rb, which pulls in the Aruba requirement for the feature scenarios. The file groupreads.feature contains the tests for the different command line options.

### lib
Lib contains the main code.

Groupreads pulls together everything into one module, allowing use through one require statement.

The group contains methods for the books that are currently being read by that group.

Reader involves the majority of the code. Firstly, reader allows viewing what is on a readers shelf by supplying the name of the shelf. It then contains methods based on this to feedback the read and to_read books for the user. A method is available for listing groups, along with a method for listing the currently reading books for all of those groups. Finally, there is also a new books method for listing out the new books, which is the primary purpose of this application.

The CLI class uses Thor to create the command line arguments for this gem.

### spec
The spec directory contains all of the tests for this code. These specs contain all that will be useful going forward, to make sure that new features do not break old features, and that refactoring of the application allows it to carry on working. These tests were also fundamental in helping with the design of the application.

### base directory
The base directory contains a .gitignore file, which does the usual ignoring of files for git. The .rspec file contains some options for rspec. .travis.yml contains options for pushing the code through a [Travis CI](https://travis-ci.org) pipeline. There is a code of conduct document directly from [contributor covenant](http://contributor-covenant.org). The gem file points to the gem spec. The gem spec contains the project details, deals with pulling in all of the requirements and building out the development commands. The license chosen for this project was the MIT license. This license allows anyone to use, modify or sell my gem, as long as they don't blame me, and as long as they include this license in all copies or code that is made up of a substantial portion of this gem. The Rakefile contains a different way of running the specs. The readme contains details on the gem, how to use it and how to contribute to it.

## Development Method
The gem was developed using test-driven development. Each test was written one at a time to provide the next piece of the puzzle that I required. I then ran the new test to make sure that it was failing. I then fixed the error messages provided by the test one by one until the test passed. I then refactored this test. Rspec was used to test the methods, Aruba and Cucumber were used to test the CLI. Pry was used to create breakpoints while solving issues. I created the develop branch while working on the code. When I completed development, I merged this branch back into master.

## Future improvements
There are some future enhancements that I'd like to make to the gem. I have provided a list of these below.
* Guide the user through setting up their environment with the setup file.
* Extend the CLI to include all of the different methods.
* Provide a help command on the CLI.
* Improve the feature tests to check the output of the newbooks option.
* Refactor the addition of the key-value pairs for the HTTP GETs into a method.
* Create docs.
