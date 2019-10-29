---
layout: post
title: Responding to Model Events with Laravel
permalink: "/responding-to-model-events-with-laravel/"
date: 2019-10-29 12:32 -0400
---
There may be times when you want to respond to something that has happened in your app; maybe a user purchased an item and you want to send them a message saying that their order was received. Instead of adding the logic to send the order confirmation alongside the code that handles the purchase, you can use events and event listeners to separate the two actions.

[Laravel](https://laravel.com/) is a framework for creating web applications using PHP, and this post will go over how to respond to changes in your Laravel app's models by using model events with Event and Listener classes.

---
## Model Events

Eloquent models fire different events when they are manipulated, and these events make it easy to write code that should be executed depending on what has happened to the model.  

Imagine needing to send a message to admins whenever a new User successfully registers - you could hook into the User model's ```created``` event to do this.  

The different model events are:
* ```retrieved```
* ```creating```
* ```created```
* ```updating```
* ```updated```
* ```saving```
* ```saved```
* ```deleting```
* ```deleted```
* ```restoring```
* ```restored```



## Events

Event classes are stored in ```app/Events``` and will correspond to a model event. These classes act as a container for the Model instance that fired the event and typically save the Model instance as a property.



## Listeners
Listener classes are stored in ```app/Listeners``` and will correspond to an Event. These classes will receive an instance of the Event class and have a ```handle()``` method that contains code that should be executed as a result of the model event. Listener classes can access the Model instance since it is a property of the Event instance.



## Listening for Model Events
To listen for a model event, you need to:
1. register the necessary Event and Listener classes within the ```$listen``` property of ```EventServiceProvider```
2. generate the Event and Listener classes with an ```artisan``` command
3. register the Event class and the model event within the Model's ```$dispatchesEvents``` property
4. add the code that should execute within the Listener class' ```handle()``` method


### Register the Event and Listener within EventServiceProvider

```EventServiceProvider``` (```app/Providers/EventServiceProvider.php```) will have a ```$listen``` array that contains the mappings for Events and their Listeners. Add the new pair that you will need to this array.

For example:
```
protected $listen = [
    '\App\Events\NewUserRegistered::class' => [
        '\App\Listeners\NotifyAdminNewUserRegistration::class',
    ],
];
```
The Event-Listener pair is added here first - before they actually exist - because you can use an ```artisan``` command to generate them automatically from the ```$listen``` array.


### Generate Event and Listener Classes

```php artisan event:generate``` will generate the necessary classes for the Events and Listeners that are defined within ```EventServiceProvider```'s ```$listen``` property.

For example:
```
php artisan event:generate
```
would generate a ```NewUserRegistered``` Event class and a ```NotifyAdminNewUserRegistration``` Listener class.

The ```event:generate``` command looks at ```EventServiceProvider```'s ```$listen``` property and runs the ```make:event``` and ```make:listener``` commands for each pair. So, if you wanted to generate the Event and Listener classes manually without using ```event:generate```, you could execute:
```
php artisan make:event NewUserRegistered
php artisan make:listener NotifyAdminNewUserRegistration --event=NewUserRegistered
```


### Register the Event Class with the Model

Now that the Event and Listener classes have been created and then registered within ```EventServiceProvider```, the Event class should be mapped to its corresponding model event in the Model class.

To do this, add a ```$dispatchesEvents``` property on the Model and define the mapping.

For example:
```
class User extends Authenticatable
{
    protected $dispatchesEvents = [
        'created' => \App\Events\NewUserRegistered::class,
    ];
}
```


### Add Code to the Listener Class

Now that the model event has been connected to the Event class, and the Event class has been connected to the Listener class, it's time to write the actual code that should execute when the model event occurs.

This code should be added to the ```handle()``` method, which will receive an instance of the Event it is responding to.

For example:
```
class NotifyAdminNewUserRegistration
{
    public function handle(NewUserRegistered $event)
    {
        // write code to notify admin of new user registration here
        // can get the new user that was registered with $event->user
    }
}
```


---

## Summary of the Model-Event-Listener Setup

To summarize:
1. The Model's ```$dispatchesEvents``` property links the model event to the Event class  
    * The Event class receives an instance of the Model in its ```__construct()``` method and saves it as a property
2. ```EventServiceProvider```'s ```$listen``` property links the Event class to the Listener class
    * The Listener class receives an instance of the Event class in its ```handle()``` method
3. The Listener class' ```handle()``` method will execute the code that should run when the model event occurs
    * The ```handle()``` method can access the Model instance as a property on the Event instance