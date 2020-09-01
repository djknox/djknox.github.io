---
layout: post
title: Laracon Online 2020 Recap
permalink: "/laracon-2020/"
---

Laracon is the Laravel community's time to get together, listen to talks from a variety of speakers, and get excited about the new features promised in future releases of the framework. Originally planned to be held [earlier this year at Atlanta's Georgia Aquarium](https://twitter.com/taylorotwell/status/1177566503060869121), the conference was cancelled due to COVID-19 and rescheduled for August 26th in a completely virtual setting. Fortunately, a virtual version of Laracon named "Laracon Online" has been held every year since 2017, so the event team was well-equipped with the experience needed to transition from aquarium to Zoom.  

Below is a brief recap of Laracon Online 2020 that I hope will be useful for anyone who was unable to attend or could just use a refresher on all the cool stuff that happened.  

---

10:00AM EST  
Freek Van Der Herten - [@freekmurze](https://twitter.com/freekmurze)  
"A Practical Look at Multitenancy in Laravel"

Multitenancy is, basically, when a single app serves several (groups of) users and those users/teams are unable to see the data belonging to other users/teams.  

Freek shared a new `composer` package for implementing multitenancy within a Laravel application called [Laravel Multitenancy](spatie.be/docs/laravel-multitenancy).  

---

Tighten - [@tightenco](https://twitter.com/TightenCo)  
"onramp Ad"

Although this was an advertisement, [onramp](onramp.dev) seems like something worth sharing here. While still a work in progress, the website serves as a free (for now?) resource for getting into software development with Laravel.  

---

10:30AM EST  
Jenny Shen - [@jennyshen](https://twitter.com/jennyshen)  
"Build Bridges, Not Walls - Design for Users Across Cultures"  

Jenny explained what culture means for designers/developers and why it's no longer acceptable to simply translate an application's text into various languages. Modern UX design must consider the unique cultural characteristics of the intended users.

Tips:
- research culture using the language of the intended audience  
- implement local payment options  
- consider device performance and connectivity speeds  
- don't use Google Translate for text/copy  

---  

11:00AM EST  
Jonathan Reinink - [@reinink](https://twitter.com/reinink)  
"Building Modern Monoliths With Inertia.js"

Jonathan shared how [Inertia.js](https://inertiajs.com/) can be used to convert a typical server-side rendered application into a client-side rendered Single Page Application (SPA). Inertia is not a framework itself, but rather a tool that sits between the front-end (React, Vue, Svelte) and back-end (Laravel, Rails) frameworks to swap out full page reloads with JSON responses.

---  

11:30AM EST  
Jeffrey Way - [@jeffrey_way](https://twitter.com/jeffrey_way)  
"Bad Is Good"

Jeffrey spoke about how using utility CSS and inline JS on reusable HTML components violates many design principles and would be considered "bad code" to a lot of developers, past and present. However, he makes the case for using frameworks such as [Tailwind](https://tailwindcss.com/) and [Alpine](https://github.com/alpinejs/alpine) to place utility CSS classes and inline JS on [Blade components](https://laravel.com/docs/7.x/blade#components) in order to simplify reusable units.

Here's an example of a counter component that uses Tailwind + Alpine:
```
<div
    class="mx-auto"
    x-data="{ count: 0 }">
    <button
        @click="count++"
        x-text="count"
    </button>
</div>
```

Jeffrey finished with a brief talk on the practice of end-to-end testing and how it can be done with [Cypress](https://www.cypress.io/how-it-works/) and Laracast's [Laravel Cypress package](https://github.com/laracasts/cypress) or using Laravel's native [Dusk](https://laravel.com/docs/7.x/dusk) composer package.  

---  

12:15PM EST  
Taylor Otwell - [@taylorotwell](https://twitter.com/taylorotwell)
"Exploring Laravel 8.x"

Laravel 8 will be released on September 8th, 2020 and Taylor shared a lot of the new features that will be available on that day. This is always an exciting part of Laracon, so I'll list some of the new features/improvements that were shared.


`App\Models` directory added to organize Model classes  

Route caching improved  
- closure-based routes can be cached

Event Listening simplified  
- simplified syntax of registering Event Listeners - no repeating of class names

Queuable Anonymous Event Listeners  
- anonymous event listeners can now be queued (previously, only Event Listener classes with ShouldQueue interface could be queued)

Maintenance Mode improvements (`php artisan down`)  

- New options for maintenance mode:  
        `--secret`  
        `--render`  
        `--status`  
        `--redirect`  

- Allow access to some users with `--secret`  
        Example:
        - `php artisan down --secret=laracon-2020`
        - App is now only accessible at `/laracon-2020`
        - user receives secret cookie and the app is reloaded  

- Pre-rendered pages with `--render`  
        Example:  
	    - `php artisan down --render="errors:503"`

- Example of multiple options combined:  
        - `php artisan down --render="welcome" --redirect=/ --status=200 --secret=laracon-2020`  

Exception handling for queued anonymous functions  
- `catch()` method registers a callback for when the job fails


Exponential Backoffs  
- setting timing options for re-trying failed jobs  
- new `App\Jobs\BackOffJob` class has `backoff()` method

   * Example of exponential backoffs:  
    - first try: wait 1s to retry
    - second try: wait 5s
    - third try: wait 10s

```
public function backoff()
{
    return [1, 5];
}
```


Job Batching
- queue a batch of jobs that execute at the same time and have callbacks for when they're finished  
- Example: chunking a CSV import and being notified at the various states of completion


Rate-limiting  
- `RateLimiter` facade  
- `Limit::perMinute()`

Example:
```
// RouteServiceProvider.php
RateLimiter::for('global', fn () => Limit::perMinute(100))

// routes\web.php
Route::middleware(['throttle:global'])...
```

Schema dumping  
- squash migrations into one .sql file with `php artisan schema:dump`
- generates raw SQL schema in `database\schema` directory
- when migrations are run, the SQL schema will be used
- `php artisan schema:dump --prune`
    - will squash migrations into one .sql file and remove the migration files


Model Factory improvements  
- class-based model factories
    - example: `User::factory()->create();`
- `definition()` method for defining attributes
- `has()` method - add relationships in model factories much more elegantly 
    - shorthand, magic methods:
        ex: `->hasPosts()`, `->hasUsers()`, etc.
            - Laravel will know that Post, User, etc model are child models
- `for()` method - other side of relationships
    - also has shorthand, magic methods:
        ex: `->forPost()`, `->forUser()`, etc.
            - Laravel will know that Post, User, etc model are parent models
- `hasAttached()` method - many-to-many relationships

- [Backwards Compatability Package](github.com/laravel/legacy-factories)
    - provides support for model factories in Laravel versions before Laravel 8

Laravel JetStream  
- UI scaffolding (including auth)
- built on TailwindUI
- has user profile, password changing, etc.
- 2FA
- recovery codes
- API tokens
- teams & team-member roles
- billing  
- install with Blade + Livewire: `php artisan jetstream:install livewire`
- install with Vue + Inertia: `php artisan jetstream:install inertia`  

---  

2:00PM EST  
Prosper Otemuyiwa - [@unicodeveloper](https://twitter.com/unicodeveloper)  
"Supercharing Laravel Apps with Machine Learning"  

Prosper provided this basic definition of "Machine Learning": computer processing and evaluating data beyond programmed algorithms. He spoke about how developers can get started with machine learning by leveraging pre-trained models that are accessible via API calls.

A few of these models include:
- Microsoft Azure Cognitive Services
- Amazon Machine Learning API
- Google Cloud ML API
- IBM Watson ML API

Prosper also announced his own "Laravel ML Kit" composer package that will become available a week or so after Laravel 8's release. The package includes:
- driver-based solution for adding ML features in easy-to-use-package
- ships with Google Cloud integration
- Amazon, Microsoft, IBM integration coming soon
- text recognition, face detection, landmark detection, translations, logo detection, label detection, text to speech, sentiment analysis, etc.

Examples:
```
// fetch text from Image
ml()->extractTextFromImage($filePath);

// detect all faces in this image
ml()->detectFacesInImage($filePath);

// convert text from one language to another
ml()->translateTextToAnotherLanguage($text, $targetLanguage);

// check whether the image contains NSFW content
ml()->isAdultContentPresentInImage($filePath);
```

When the package is released, it can be installed with `composer require unicodeveloper/laravel-ml`.

---  

2:30PM EST  
Colin Decarlo - [@colindecarlo](https://twitter.com/colindecarlo)  
"The Importance of Practice"  

Colin shared his thoughts on the importance of having a daily routine for practice. His advice was to budget time each day for refining skills, ideally after accomplishing any tasks that are necessary to doing your job. That way, practice becomes a normal routine and is positioned as an integral part of your daily life.  

---  

3:00PM EST  
Matt Stauffer - [@stauffermatt](https://twitter.com/stauffermatt)  
Jose Soto - [@josecanhelp](https://twitter.com/josecanhelp)  
"Don't Cry When Your Dev Dependencies Die"  

Matt and Jose both work at Tighten and shared their preferences for configuring a local development environment, which involves a combination of [Laravel Valet](https://laravel.com/docs/7.x/valet) and [Docker](https://www.docker.com/why-docker). Valet is not particularly suited for managing certain dependencies like MySQL, Redis, Postgres, etc., so Docker can be great for filling in those gaps.  

To help with some of the complexity of installing and managing dependencies with Docker, Tighten developed [Takeout](https://github.com/tightenco/takeout) and made it available to the community. It is intended to be used with the Valet + Docker environment, so it could be a great tool to pull in to your projects if you use the same setup.  
Be sure to install Docker first, and then install Takeout with `composer global require tightenco/takeout`.  

Some examples of `takeout` commands:
```
// show list of docker containers
takeout list

// install Postgresql
takeout enable postgresql

// remove Postgresql
takeout disable postgresql
```

---  

3:30PM EST  
Marcel Pociot
Refactoring to Simplicity

Simple code is:
	- expressive
	- readable
	- understandable
	- self-explanatory
	- reasurring
	- fun

Familiar code != Simple code

Do code reviews to ensure that code is actually simple for your team to understand and maintain

Refactor code as it grows

Simple rules for simple code:
	- naming variables
	- useful method names
	- return early

Simplicity takes courage
	- know what you can skip to make things easy and simple
	- be brave enough to present your code
	- leave behind code that reflects you and your abilities


---  

4:00PM EST  
Adam Wathan - @adamwathan
Building a Component Library with Tailwind CSS
"Crafting Components with tailwindcss"

Design layouts and components so that components can be swapped in and out (if you remove a component, the layout is unchanged)

In vue components, Adam uses setup(props) to set up the component based on the prop(s), slot(s), etc. passed to it
	- setup()
		* https://composition-api.vuejs.org/api.html#setup

Duplicated markup
	- fine to have separate markup for mobile/desktop styling - makes it easier for tailoring UX to screen sizes

Try to make components responsive WITHOUT using media queries
	Adam demo'd:
		- 'gap' (in cssgrid, but coming to flexbox)
		- copying margins of child components to their parents

---  

4:30PM EST  
Jessica Archer - @jessarchercodes
The Laravel Developer's Guide to Vue SPAs - Part 2

Embrace the mono-repo by default
	- Vue app and Laravel API in same repository

`CODEOWNERS` file
	- specify users/groups that are responsible for different parts of the codebase

Vue.js Style Guide:
	- eslint.vuejs.org

Slots before props!

Select HTML elements based on behavior, not appearance (accessibility and user expectations)
	- examples:
		- use <a> even when it doesn't LOOK like a link
		- use <button> when it is something that triggers behavior from clicking on it
		- use <form @submit.prevent> with forms

Give inputs a name attribute

Make type="button" the default for buttons

Create <BaseIcon> component for any icons
	- pass the icon name as a prop
	- use computed property to create the icon based on the prop passed to the component


Back-end tips
	- Think of your controller tests as a contract between your front-end and back-end (example: don't use withoutMiddleware() in tests, as your front-end will always use middleware)
	- use appropriate HTTP headers

State Management
	- prefer local state
	- use Vuex for global state only

Form Validation
	- server-side form validation is fine, just pass errors/success messages to front-end to display (re-creating front-end form validation is not really necessary)

Write code that's easy to delete and refactor

APIs are UIs for developers - consider the "Developer Experience"

Remember the BACK button

Utility CSS is way more enjoyable

Don't lose sight of why you're building an SPA
	- more like an app than a webpage
	- enjoyable!

Learn the features of tools you work with

---  

5:00PM EST  
April Dunford - @aprildunford
Power Positioning - How to Harness a Marketing Superpower
Powerful Product Positioning

Positioning is not:
	- a tag line
	- a POV
	- vision
	- brand
	- messaging
	- "marketing"
	- go-to-market strategy

Positioning defines:
	- how the product is the best at providing something that a well-defined set of customers care a lot about
	- context-setting for products

Market categories help customers decide what to pay attention to
Customers use what they know about the market to understand solutions

Traditional Positioning Statement:
	- For (target market), (our offering) is a (market category) which provides (benefit) unlike (competitor).

Components of Positioning:
	- market category
	- alternatives
	- unique attributes
	- value
	- customer segments

Customer-centric Positioning
	- if you didn't exist, what would customers use?
	- what features do you have that others don't?
	- what value do the attributes enable?
	- who cares a lot about that value?
	- what context makes the value obvious to your target segments?


---  

5:30PM EST  
Caleb Porzio - @calebporzio
All the Cool New Things in Livewire & Alpine

Livewire v2.0

How to set up Livewire:
	composer require livewire/livewire

	Drop in blade directives:
	@livewireStyles
	@livewireScripts

Two main strengths:
	Sync data with two-way binding
	Listen to events

Uses blade views, so no bundle step like with Vue.js

Can create "full-stack" components, rather than separate back-end and front-end components



Alpine.js
@entangle to synchronize properties between Livewire and Alpine

---  

6:00PM EST  
Tim Macdonald - @timacdonald87
Follow the Eloquent Road

Eloquent:
	* Builders
	* Models
	* Collections

Eloquent Collections
timacdonald.me/giving-collections-a-voice

Eloquent Builders
- move Eloquent Scopes from the model class to a builder class
Witch::whereWicked()
	 ->whereFromTheWest();
timacdonald.me/dedicated-eloquent-model-query-builders


---  

6:30PM EST  
Jack Ellis - @jackellis
How We Scaled Fathom Analytics to Handle Billions of Requests
usefathom.com 

Started on Heroku - didn't prematurely optimize

Use queues + jobs to lighten strain on database

Don't over-complicate things trying to be clever

Infrastructure
	- Laravel Vapor
	- Redis

Problems at Scale
	- moving from Redis to to ElasticSearch to handle scale of queries
	- moving from MySQL/Postgres/etc. to DynamoDB in order to handle issue of over-provisioning database resources while still needing infinite scale -> DynamoDBforLaravel.com


---  

7:00PM EST  
Closing Remarks

feedback: bit.ly/laracon2020

5500+ attendees!