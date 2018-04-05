---
layout: post
title:      "Device Manager"
date:       2018-04-05 07:25:39 +0000
permalink:  device_manager
---


For my Sinatra project, I am making a device manager website. This is for managing consumer routers and set top boxes in our test environment. The website meets the below requirements:
* Be an MVC website.
* Use ActiveRecord with Sinatra.
* Have a devices, groups and users table.
* Devices belong to a group. A device must have a serial number and model. It may also have a firmware version, last contact and last activation.
* Groups can have many devices and many users. A group must have a name and a type.
* Users can have many groups. A user must have a username, name, email and password. They may also have a group, and they may not do anything until they have a group.
* Only admins can add devices.
* Admins must approve new user signups before they can do anything.
* Users may view devices if the device belongs to their group and their group is of type read.
* Users may delete devices and edit the firmware version, last contact and last activation of the devices that belong to their group if their group is of type write. They also have all permissions granted through read.
* Admins may view, delete, edit and add new devices. All devices will be a member of the admin group.
* User emails and usernames must be unique and not blank.
* Passwords must be 8 characters or longer, must contain 3 of 4 of either lowercase, uppercase, symbols and numbers.
* Group names must be unique.
* Device serial numbers must be unique.

The database and model for my project has four main tables that can most easily be seen in the diagram below.

![entity relationship diagram](https://s3-eu-west-1.amazonaws.com/nemene-share/device-manager-erd.png)

The project was developed using test driven development. The first part of the application that was developed was the database and the model. This provided a basis for the rest of the website. The tests for this section were provided in spec/models and used rspec. Once the model was completed, functional tests were written for individual sections, followed by implementation of the features to pass these tests. These were written in rspec and capybara. The final piece of testing that was carried out was manual testing of the website. Layout and the small pieces of javascript functionality were tested using this method.
