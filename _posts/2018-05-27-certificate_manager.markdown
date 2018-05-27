---
layout: post
title:      "Certificate Manager"
date:       2018-05-27 19:33:21 +0000
permalink:  certificate_manager
---


For my project, I made a website to manage SSL certificates. In my current job, finding out which expiring certificates are hosted on which load balancers is one of the main problems I have.

To do this, I first came up with a sketch for the sign in and certificates page on Balsamiq. You can see the first of these for sign in below.

![Certificate Manager Enter](https://s3-eu-west-1.amazonaws.com/nemene-share/certificate-manager/enter.png)

The second of these was for the certificates page.

![Certificates Index](https://s3-eu-west-1.amazonaws.com/nemene-share/certificate-manager/certificates.png)

From these pages, I then wrote up the feature test for the login page. From here, I then I ran this test, wrote the appropriate controller test to move this feature test forward.

At this point, I needed to start creating the model. Before doing this, I created an entity relationship diagram that you can see below.

![Entity Relationship Diagram](https://s3-eu-west-1.amazonaws.com/nemene-share/certificate-manager/certificate-manager.png)

I followed the ERD with writing model tests and writing migrations to get the controller test passing. Once the functional test passed, I then worked on the styling for the completed pages.

I learnt this methodology from [Obey The Testing Goat](https://www.obeythetestinggoat.com). You can see an overview of this method in the below diagram.

![TDD with Feature and Unit Tests](http://www.obeythetestinggoat.com/book/images/twp2_0404.png)

Depending on the tests, they sometimes require controller tests in between the feature tests and unit (model) tests. One problem that I ran into using functional/feature tests while testing variations in my device manager project is that I ended up having prolonged running tests. They took over 30 seconds to run, which gave a slow feedback loop. I got around this at the time to a certain extent by only running the tests that I was working on at the time. This time, I ended up using feature tests for the happy path, and then I used controller tests for testing different invalid inputs. These controller tests helped speed up my test runs to under 3 seconds, with a maximum of 6 seconds for the tests to load. The disadvantage of this approach was less robust testing of unhappy paths.

Near the end, I went through the different requirements, and I noticed a few different missing pieces. This specification check is where I added the comments section and the new and show nested resources for load balancer certificates.

For a project such as this, where meeting the requirements of demonstrating what I had learnt was the primary objective, it may have been better to set up a board on Trello with the requirements beforehand and work through the different needs. A requirements-driven development like this, however, would have clashed with my secondary condition that this project is useful for me at work. The main advantage of a design-driven development is a focus on the user experience. Perhaps starting with the Trello board, and putting the sketches onto the Trello board would be an improvement.
