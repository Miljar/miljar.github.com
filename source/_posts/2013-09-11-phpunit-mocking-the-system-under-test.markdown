---
layout: post
title: "PHPUnit: Mocking the System Under Test"
date: 2013-09-11 17:55
comments: true
categories: 
---

When unit testing a class, at one point you'll have to check if some functionality in a dependency is triggered. Usually this is done by replacing the dependency with a mock object. A well designed system let's you inject dependencies in your objects, thus allowing for easier unit testing.

``` php Mocking dependencies
<?php
class UserService
{
  proteced $mailerService;
	
	public function getMailerService()
	{
		if (empty($this->mailerService)) {
			throw new \RuntimeException('No mailer service set');
		}
		
		return $this->mailerService;
	}
	
	public function setMailerService(MailerInterface $service)
	{
		$this->mailerService = $service;
		
		return $this;
	}
	
	public function registerUser($userObject)
	{
		// …
		
		$mailerService = $this->getMailerService();
		$mailerService->sendMail('registration', $userObject);
	}
}


class UserServiceTest extends PHPUnit_Framework_TestCase
{

	public function testRegisteringUserTriggersMail()
	{
		$userObject = new User('Tom', 'tom@example.com');
	
		$mailerMock = $this->getMock('My\Namespace\MailerService');
		$mailerMock->expects($this->once())
			->method('sendMail')
			->with(
				$this->equalTo('registration'),
				$this->equalTo($userObject)
			);
			
		$userService = new UserService();
		$userService->setMailerService($mailerMock);
		
		$userService->registerUser($userObject);
	}

}
```

The example above tests if the mailer service is triggered when the registerUser() method is called. The System Under Test (SUT) here is the UserService, and the mock object is the MailerService.

But what if you wanted to mock one or more methods in the SUT itself? Like the example below:

``` php Example with internal dependencies
<?php
class UserService
{
    // ...
	
	public function registerUser($userObject)
	{
		$this->saveUser($userObject);
		
		$mailerService = $this->getMailerService();
		$mailerService->sendMail('registration', $userObject);
	}
	
	public function saveUser($userObject)
	{
		// ...
	}
}
```

This implementation of the UserService has 2 separate methods: one for saving a User object and a convenience method registerUser which calls saveUser and then sends out a mail.

I've sometimes wondered how you can make sure that the registerUser method calls the other method without actually covering the implementation of it. Let me rephrase that: "How can you mock a method in the SUT?"

Of course the answer was quite simple, but it only hit me after a while: Let the SUT become the mock!

In PHPUnit, it's possible to create a *partial mock*. That's a mock with not all its methods mocked. When you invoke a method of a partial mock that wasn't mocked, then the original method is called. Here's an example in a unit test:

``` php Mocking the SUT
<?php
class UserServiceTest extends PHPUnit_Framework_TestCase
{

    public function testRegisteringUserCallsSaveUserMethod()
	{
		$userObject = new User('Tom', 'tom@example.com');
		
		$mock = $this->getMock('UserService', array('saveUser'));
		$mock->expects($this->once())
			->method('saveUser')
			->with($this->equalTo($userObject));
			
		$mock->registerUser($userObject);
	}
}
```

So first the SUT is mocked, with expectations for our saveUser method. Then on that mock we call the registerUser method, which is not mocked.

If you generate a code coverage report from that test, then only the registerUser method will be covered, which is exactly what I wanted.