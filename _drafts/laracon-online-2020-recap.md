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

11:00AM
Jonathan Reinink - [@reinink](https://twitter.com/reinink) 
"Building Modern Monoliths With Inertia.js"

Jonathan shared how [Inertia.js](https://inertiajs.com/) can be used to convert a typical server-side rendered application into a client-side rendered Single Page Application (SPA). Inertia is not a framework itself, but rather a tool that sits between the front-end (React, Vue, Svelte) and back-end (Laravel, Rails) frameworks to swap out full page reloads with JSON responses.

---  

11:30AM
Jeffrey Way - @jeffrey_way
Bad Is Good

Blade components to simplify re-usable HTML

Blade components with Alpine
	- inline JS
	- very similar syntax to Vue
	- counter with alpine:
		<div x-data="{ count: 0 }">
			<button
				@click="count++"
				x-text="count"
			</button>
		</div>

End-to-end tests
	- tests every piece of app (interacts with browser, etc.)
	- typically very brittle
	- Cypress (there is a laracasts/cypress package) + Laravel Dusk are tools for end-to-end testing

---  


12:00PM - Break

---  


Ad - Vehikl
Agency of developers to be added to a client's team for developing code

---  

12:15PM
Taylor Otwell - @taylorotwell
Exploring Laravel 8.x

September 8th, 2020 release

App\Models directory
	- place models in this directory rather than in \App

Route caching improved
	- closure-based routes can be cached

Event Listening simplified
	- simplified syntax of registering Event Listeners - no repeating of class names

Queuable Anonymous Event Listeners
	- anonymous event listeners can now be queued (previously, only Event Listener classes with ShouldQueue interface could be queued)
	- add queued() to anonymous event listener declaration

Maintenance Mode improvements
	- php artisan down

	New options for maintenance mode!
	--secret
	--render
	--status
	--redirect

	Allow access to some users
	--secret
	- can now use --secret to allow some users to still access app
	- php artisan down --secret=laracon-2020
	- accessible at /laracon-2020 (gives user secret cookie and reloads app)

	Pre-rendered pages
	--render
	php artisan down --render="errors:503"

	Example:
	php artisan down --render="welcome" --redirect=/ --status=200 --secret=laracon-2020

Exception handling for queued anonymous functions
	- catch() method registers a callback for when the job fails


Exponential Backoffs
	- can set timing for re-trying failed jobs

	Example to wait:
		- first try: 1s
		- second try: 5s
	public function backoff()
	{
		return [1, 5];
	}


Job Batching (large feature)
	- robust job batching
	- queue a bunch of jobs that execute at the same time and have callbacks for when they're finished
	- example: chunking CSV import and be notified of various states of completion


Rate-limiting
	- RateLimiter facade
	- Limit::perMinute()

	Example:
		RateLimiter::for('global', fn () => Limit::perMinute(100))

		Middleware:
		Route::middleware(['throttle:global'])...


Schema dumping
	- squash migrations into one .sql file
	- php artisan schema:dump
	- generates raw SQL schema in database\schema directory
	- when migrations are run, the SQL schema will be used
	- php artisan schema:dump --prune
		- will squash migrations into one .sql file and remove the migration files


Model Factory improvements
	- class-based model factories
		- ex: User::factory()->create();
	- has definition() method for defining attributes
	- has() method - can add relationships in model factories much more elegantly 
		- shorthand, magic methods:
			ex: ->hasPosts(), ->hasUsers(), etc.
				- laravel will know that Post, User, etc model are child models
	- for() method - other side of relationships
		- also has shorthand, magic methods:
			ex: ->forPost(), ->forUser, etc.
				- laravel will know that Post, User, etc model are parent models
	-hasAttached() method - many-to-many relationships

	Package:
	github.com/laravel/legacy-factories
		- provides support for model factories in Laravel versions before Laravel 8


Laravel JetStream
	- scaffolding (including auth)
	- built on TailwindUI
	- has user profile, password changing, etc.
	- 2FA
	- recovery codes
	- API tokens
	- teams & team-member roles
	- billing

	Use Blade + Livewire
		- php artisan jetstream:install livewire

	Use Vue + Inertia
		- php artisan jetstream:install inertia


---  

2:00PM
Prosper Otemuyiwa - @unicodeveloper
Supercharing Laravel Apps with Machine Learning

Machine Learning: computer processing and evaluating data beyond programmed algorithms

How to get started with Machine Learning:
Leverage pre-trained ML models accessible via API calls
- Microsoft Azure Cognitive Services, Amazon Machine Learning API, Google Cloud ML API, IBM Watson ML API

Laravel ML Kit
	- driver-based solution for adding ML features in easy-to-use-package
	- ships with Google Cloud integration
	- Amazon, Microsoft, IBM integration coming soon
	- text recognition, face detection, landmark detection, translations, logo detection, label detection, text to speech, sentiment analysis, etc.

	Examples:
		// Fetch text from Image
		ml()->extractTextFromImage($filePath);

		// detect all faces in this image
		ml()->detectFacesInImage($filePath);

		// convert text from one language to another
		ml()->translateTextToAnotherLanguage($text, $targetLanguage);

		// check whether the image contains NSFW content
		ml()->isAdultContentPresentInImage($filePath);

composer require unicodeveloper/laravel-ml
	- released the week after Laravel 8



---  

2:30PM
Colin Decarlo - @colindecarlo
The Importance of Practice

Budget time each day for practice - accomplish what is necessary and then spend time refining skills.





---  

3:00PM
Matt Stauffer - @stauffermatt
Jose Soto - @josecanhelp
Don't Cry When Your Dev Dependencies Die

Valet + Docker
	- Valet may not be suited for managing certain dependencies (MySQL, Redis, Postgres, etc.)
	- use Docker for managing other dependencies

Docker:
	- virtualization - containers
	- ensures parity between local dev machine and production machine

Dockerized Laravel
	- MySQL container
	- PHP container w/ app
	- Nginx Container - serves application and exposes port so that it can be accessed in a browser

Why Docker?
	- keep machine clean
	- multiple versions of dependencies
	- some software is difficult to install

Servers: Pets vs Cattle
	- dont think of servers as pets -> no need to spend time and resources taking care of them

Composer Package: Takeout
composer global require tightenco/takeout (not yet released)
- macOS tool for installing and managing dependencies using Docker
- great for using with Valet
- install Docker, then install Takeout

Use postgresql with Takeout:
	- takeout enable postgresql
	- takeout disable postgresql

takeout list
	- show list of docker containers

github.com/tightenco/takeout

---  

3:30PM
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

4:00PM
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

4:30PM
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

5:00PM
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

5:30PM
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

6:00PM
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

6:30PM
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

7:00PM
Closing Remarks

feedback: bit.ly/laracon2020

5500+ attendees!