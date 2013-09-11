---
layout: post
title: "PHPUnit: Mocking the System Under Test"
date: 2013-09-11 17:55
comments: true
categories: 
---

# PHPUnit: Mocking the System Under Test

When unit testing a class, at one point you'll have to check if some functionality in a dependency is triggered. Usually this is done by replacing the dependency with a mock object. A well designed system let's you inject dependencies in your objects, thus allowing for easier unit testing.

{% gist 6527929 %}

The example above tests if the mailer service is triggered when the registerUser() method is called. The System Under Test (SUT) here is the UserService, and the mock object is the MailerService.

But what if you wanted to mock one or more methods in the SUT itself? Like the example below:

{% gist 6527951 %}

This implementation of the UserService has 2 separate methods: one for saving a User object and a convenience method registerUser which calls saveUser and then sends out a mail.

I've sometimes wondered how you can make sure that the registerUser method calls the other method without actually covering the implementation of it. Let me rephrase that: "How can you mock a method in the SUT?"

Of course the answer was quite simple, but it only hit me after a while: Let the SUT become the mock!

In PHPUnit, it's possible to create a *partial mock*. That's a mock with not all its methods mocked. When you invoke a method of a partial mock that wasn't mocked, then the original method is called. Here's an example in a unit test:

{% gist 6527967 %}

So first the SUT is mocked, with expectations for our saveUser method. Then on that mock we call the registerUser method, which is not mocked.

If you generate a code coverage report from that test, then only the registerUser method will be covered, which is exactly what I wanted.