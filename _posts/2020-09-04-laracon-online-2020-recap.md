---
layout: post
title: Laracon Online 2020 Recap
permalink: "/laracon-2020/"
date: 2020-09-04 16:33 -0400
---
Laracon is the Laravel community's time to get together, listen to talks from a variety of speakers, and get excited about the new features promised in future releases of the framework. Originally planned to be held [earlier this year at Atlanta's Georgia Aquarium](https://twitter.com/taylorotwell/status/1177566503060869121){:target="_blank"}, the conference was cancelled due to COVID-19 and rescheduled for August 26th in a completely virtual setting. Fortunately, a virtual version of Laracon named "Laracon Online" has been held every year since 2017, so the event team was well-equipped with the experience needed to transition from aquarium to Zoom.  

Below is a brief recap of Laracon Online 2020 that I hope will be useful for anyone who was unable to attend or could just use a refresher on all the cool stuff that happened.  

---

10:00AM EST  
Freek Van Der Herten - [@freekmurze](https://twitter.com/freekmurze){:target="_blank"}  
"A Practical Look at Multitenancy in Laravel"

Multitenancy is, basically, when a single app serves several (groups of) users and those users/teams are unable to see the data belonging to other users/teams.  

Freek shared a new `composer` package for implementing multitenancy within a Laravel application called [Laravel Multitenancy](https://spatie.be/docs/laravel-multitenancy){:target="_blank"}.  

---

Tighten - [@tightenco](https://twitter.com/TightenCo){:target="_blank"}  
"onramp Ad"

Although this was an advertisement, [onramp](https://onramp.dev){:target="_blank"} seems like something worth sharing here. While still a work in progress, the website serves as a free (for now?) resource for getting into software development with Laravel.  

---

10:30AM EST  
Jenny Shen - [@jennyshen](https://twitter.com/jennyshen){:target="_blank"}  
"Build Bridges, Not Walls - Design for Users Across Cultures"  

Jenny explained what culture means for designers/developers and why it's no longer acceptable to simply translate an application's text into various languages. Modern UX design must consider the unique cultural characteristics of the intended users.

Tips:
- research culture using the language of the intended audience  
- implement local payment options  
- consider device performance and connectivity speeds  
- don't use Google Translate for text/copy  

---  

11:00AM EST  
Jonathan Reinink - [@reinink](https://twitter.com/reinink){:target="_blank"}  
"Building Modern Monoliths With Inertia.js"

Jonathan shared how [Inertia.js](https://inertiajs.com/){:target="_blank"} can be used to convert a typical server-side rendered application into a client-side rendered Single Page Application (SPA). Inertia is not a framework itself, but rather a tool that sits between the front-end (React, Vue, Svelte) and back-end (Laravel, Rails) frameworks to swap out full page reloads with JSON responses.

---  

11:30AM EST  
Jeffrey Way - [@jeffrey_way](https://twitter.com/jeffrey_way){:target="_blank"}  
"Bad Is Good"

Jeffrey spoke about how using utility CSS and inline JS on reusable HTML components violates many design principles and would be considered "bad code" to a lot of developers, past and present. However, he makes the case for using frameworks such as [Tailwind](https://tailwindcss.com/){:target="_blank"} and [Alpine](https://github.com/alpinejs/alpine){:target="_blank"} to place utility CSS classes and inline JS on [Blade components](https://laravel.com/docs/7.x/blade#components){:target="_blank"} in order to simplify reusable units.

Here's a simple example of a counter component that uses Tailwind + Alpine, showing a utility CSS class and inline JavaScript:
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

Jeffrey finished with a brief talk on the practice of end-to-end testing and how it can be done with [Cypress](https://www.cypress.io/how-it-works/){:target="_blank"} and Laracast's [Laravel Cypress package](https://github.com/laracasts/cypress){:target="_blank"} or by using Laravel's native [Dusk](https://laravel.com/docs/7.x/dusk){:target="_blank"} composer package.  

---  

12:15PM EST  
Taylor Otwell - [@taylorotwell](https://twitter.com/taylorotwell){:target="_blank"}  
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
- `has()` method - add child relationships  
    - shorthand, magic methods:
        ex: `->hasPosts()`, `->hasUsers()`, etc.
            - Laravel will know that Post, User, etc model are child models
- `for()` method - add parent relationships  
    - also has shorthand, magic methods:
        ex: `->forPost()`, `->forUser()`, etc.
            - Laravel will know that Post, User, etc model are parent models
- `hasAttached()` method - many-to-many relationships

- [Backwards Compatability Package](github.com/laravel/legacy-factories){:target="_blank"}
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

Needless to say, there is a lot to look forward to in Laravel 8.  

---  

2:00PM EST  
Prosper Otemuyiwa - [@unicodeveloper](https://twitter.com/unicodeveloper){:target="_blank"}  
"Supercharing Laravel Apps with Machine Learning"  

Prosper provided this basic definition of "Machine Learning": computer processing and evaluating data beyond programmed algorithms. He spoke about how developers can get started with machine learning by leveraging pre-trained models that are accessible via open APIs.

A few of these models include:  
- Microsoft Azure Cognitive Services
- Amazon Machine Learning API
- Google Cloud ML API
- IBM Watson ML API

Prosper also announced his own "Laravel ML Kit" composer package that will become available a week or so after Laravel 8's release. The package includes:
- driver-based solution for adding ML features
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
Colin Decarlo - [@colindecarlo](https://twitter.com/colindecarlo){:target="_blank"}  
"The Importance of Practice"  

Colin shared his thoughts on the importance of having a daily routine for practice. His advice was to budget time each day for refining skills, ideally after accomplishing any tasks that are necessary to doing your job. That way, practice becomes a normal routine and is positioned as an integral part of your daily life.  

---  

3:00PM EST  
Matt Stauffer - [@stauffermatt](https://twitter.com/stauffermatt){:target="_blank"}  
Jose Soto - [@josecanhelp](https://twitter.com/josecanhelp){:target="_blank"}  
"Don't Cry When Your Dev Dependencies Die"  

Matt and Jose both work at Tighten and shared their preferences for configuring a local development environment, which involves a combination of [Laravel Valet](https://laravel.com/docs/7.x/valet){:target="_blank"} and [Docker](https://www.docker.com/why-docker){:target="_blank"}. Valet is not particularly suited for managing certain dependencies like MySQL, Redis, Postgres, etc., so Docker can be great for filling in those gaps.  

To help with some of the complexity of installing and managing dependencies with Docker, Tighten developed [Takeout](https://github.com/tightenco/takeout){:target="_blank"} and made it available to the community. It is intended to be used with the Valet + Docker environment, so it could be a great tool to pull in to your projects if you use that configuration.  
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
Marcel Pociot  - [@marcelpociot](https://twitter.com/marcelpociot){:target="_blank"}  
"Refactoring to Simplicity"  

Marcel's presentation was centered around a core tenant of Laravel: simplicity.  

He stated that simple code is:
- expressive
- readable
- understandable
- self-explanatory
- reasurring
- fun

Simple code is _not_:
- Familiar code: code is not simple just because you wrote it recently and it feels elegant or makes sense to you right now  

Tips on achieving simplicity:
- do code reviews to ensure that code is actually simple for your team to understand and maintain
- refactor code as it grows
- have courage:
   * know what you can skip to make things easy and simple
   * be brave enough to present your code
   * leave behind code that reflects you and your abilities

A couple simple rules for simple code:
- give variables and methods useful names
- `return` early from methods so logic is easily understood

---  

4:00PM EST  
Adam Wathan - [@adamwathan](https://twitter.com/adamwathan){:target="_blank"}  
"Crafting Components with Tailwindcss"

Adam demonstrated a few levels of refactoring code into reusable components (such as Blade, React, or Vue components), with each level becoming increasingly advanced and elegant.  

First level:  
- simply reorganizing/extracting main elements into separate components
- no APIs or ways for components to communicate with each other
- could use "layout" components that separate the individual components from how they are displayed on the page  

Second level:
- extract reusable elements like buttons, using props as an API to configure any details (color, size, etc.)  

Third level:
- create separate, tailored styling for different screen sizes
- handle any duplication concerns with a single source of truth
   * example: a navbar component's links can be saved as data variables
- duplicate markup is OK: no need for breakpoint classes, presentation code is easier to understand, removes struggle of trying to style for one screen size without affecting the others  

Fourth level:  
- pass in components in slots to abstracted parent components that use render functions (Vue) to render the child components
   * example: IconBadge parent component, with BadgeCheckIcon passed in as a slot

Adam also demonstrated how responsive design using breakpoints/media queries can be frustrating when they are responding to the size of the viewport rather than that of their containing elements. He recommends trying to design components to be responsive _without_ using media queries first, if at all possible, so they can better adapt to the context in which they are in. A few approaches to doing this:
- with Flexbox wrapping, use the same margins for all child components and give the parent component the inverse of those margins in order to maintain alignment as flexbox wraps the elements  
   * example:
        ```
        <div class="flex flex-wrap -ml-3 -mt-2">
            <div class="ml-3 mt-3">
            </div>
        </div>
        ``` 
- CSS Grid's 'gap' property (coming soon to Flexbox) does the same thing as the previous example, with simpler code
   * example: `<div class="flex flex-wrap gap-3">`  

Adam finished his talk with a fifth level of refactoring components that extracts layout details and sends them as props from a parent layout component. The child components then need to be able to parse the props and react accordingly. This is more of a work in progress/thought experiment that was demonstrated for fun and to show how complex this type of design can become.  

---  

4:30PM EST  
Jess Archer - [@jessarchercodes](https://twitter.com/jessarchercodes){:target="_blank"}  
"The Laravel Developer's Guide to Vue SPAs - Part 2"  

Jess's talk was an extension of [one she did for Laracon AU last year](https://www.youtube.com/watch?v=Zv4bUXEwl20){:target="_blank"} in regards to creating Single Page Applications using Vue and Laravel.  

The first bit of advice is to embrace the idea of the "mono-repo", which is a codebase that contains both the front-end Vue SPA and the back-end Laravel API in the same repository. Front- and back-end code tends to be tightly coupled, so containing both in the same repository increases collaboration across teams and eliminates the need for interdependent Pull Requests that need to be coordinated in multiple places.  

The `CODEOWNERS` file is a [GitHub](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/about-code-owners){:target="_blank"} / [GitLab](https://docs.gitlab.com/ee/user/project/code_owners.html){:target="_blank"} feature that specifies which users/teams that are responsible for the various parts of the codebase. Using this file, rules can be set up to require code reviews for any PRs that touch code that a user/team "owns" before they can be merged. For mono-repos, the `CODEOWNERS` file can be used to protect each team's codebase and ensure they are made aware of any potential changes.  

Jess also touched on user expectations and the importance of accessibility when developing for the front-end. A few tips that were mentioned:
- select HTML elements based on behavior rather than appearance
   * examples:
      - use `<a>` for hyperlinks even when they are not styled to look like links
      - use `<button>` when it triggers behavior from clicking on it
      - wrap forms with `<form>`  
- give inputs a `name` attribute
- make `type="button"` the default for buttons

For back-end development:
- write controller tests as a contract between your front-end and back-end (example: don't use `withoutMiddleware()` in tests, as your front-end will always use middleware)
- use appropriate HTTP headers

A few general tips:
- for front-end state management, default to local state and use Vuex for global state only
- for form validation, server-side validation is fine by itself and consider using front-end validation for any erroneous inputs where the user may need guidance to correct  
- write code that's easy to delete/refactor; keep all tightly-coupled code together and resist sharing code across features unless it is global or generic
- APIs are UIs for developers, so consider the "Developer Experience"  
- remember the `Back` button when building SPAs
- utility CSS is more fun than scoped styles
- keep in mind why you chose to build an SPA; it should be more like an application than a webpage
- learn the features of any tools you work with  

---  

5:00PM EST  
April Dunford - [@aprildunford](https://twitter.com/aprildunford){:target="_blank"}  
"Power Positioning - How to Harness a Marketing Superpower"  

Laravel has enabled many developers to start their own businesses and side-hustles, so it was great that April gave a non-engineering presentation focused on marketing and product positioning.  

Positioning is not a tag line, a point-of-view, vision, brand, messaging, "marketing", or a go-to-market strategy. Rather, positioning defines how a product is the best at providing something that a well-defined set of customers care a lot about. It's basically context-setting for products or services.  

Some actual components of positioning include:
- market category
- alternatives
- unique attributes
- value
- customer segments

Customers use what they know about the market to understand solutions and market categories help customers decide which solutions to pay attention to. Positioning helps create a context for a product in which the value becomes immediately obvious to its intended customers.

A traditional, marketing school positioning statement is:
- "For (target market), (our offering) is a (market category) which provides (benefit) unlike (competitor)."

Some questions to ask in order to determine the positioning of a product:
- if you didn't exist, what would customers use?
- what features do you have that others don't?
- what value do the attributes enable?
- who cares a lot about that value?
- what context makes the value clear to your target segments?

---  

5:30PM EST  
Caleb Porzio - [@calebporzio](https://twitter.com/calebporzio){:target="_blank"}  
"All the Cool New Things in Livewire & Alpine"  

Caleb created [Laravel Livewire](https://laravel-livewire.com/){:target="_blank"} and [Alpine.js](https://github.com/alpinejs/alpine){:target="_blank"} and now works on both full-time thanks to donors and sponsors. His talk included how to get up and running with Livewire, it's main strengths, and new updates to both Livewire and Alpine.  

Livewire has two main mechanics: syncing data with two-way binding and listening to events.  

How to set up Livewire:
- install with `composer require livewire/livewire`
- in layouts, drop in blade directives:
    ```
    @livewireStyles
    @livewireScripts
    ```

An interesting feature of how Livewire works is that it uses back-end templating (Blade views), eliminating the bundle step that would exist with a front-end framework like Vue.js. As a result, components can be thought of as "full-stack", rather than separating features into front- and back-end code.  

Caleb did some entertaining live-coding that displayed new features of both Livewire and Alpine, and shared some amazing [screencasts](https://laravel-livewire.com/screencasts/installation){:target="_blank"} that were created for visual learners to start working with the framework.  

---  

6:00PM EST  
Tim MacDonald - [@timacdonald87](https://twitter.com/timacdonald87){:target="_blank"}  
"Follow the Eloquent Road"  

Tim's "Wizard of Oz"-themed talk demonstrated his love for [Eloquent](https://laravel.com/docs/7.x/eloquent){:target="_blank"}, Laravel's ORM, and how his relationship with it has evolved as he's gained more experience.  

Tim began with showing a few ways how models can be instantiated from forms by moving validation and creation logic to different parts of the application. Starting with the controller passing the request to a static method on the Model class, such as `Adventure::createFromRequest($request)`, to moving all the logic from the model directly into the controller, to letting the form request itself parse the request and return only what is needed to the controller (for example: `Adventure::create($request->adventure())`).  

To summarize each iteration:  
1. model instantiation via a static method on model, called from the controller
2. all instantiation logic moved directly into the controller
3. cleaning up the controller by letting the form request class parse the necessary fields and pass to Eloquent's default `create()` method within the controller
   * This final iteration pushes validation logic to the form request class, where everything is prepared and nothing unnecessary makes it through to any other parts of the application.  

Next, Tim touched on different ways of authorizing users, such as only allowing admin users to perform certain actions. Starting with checks that exist on the model, like `$user->isAdmin()`, which are used within `if` statements in both front- and back-end code, Tim showed how this can quickly grow out of hand as the number of different user roles starts to grow. Taking role-checking logic out of the model and into [gates](https://laravel.com/docs/7.x/authorization#gates) will clean up long `if` statements into more concise `can()` or `cannot()` statements.  

Moving on, Tim elaborated on the various features of Eloquent:
- [Models](https://laravel.com/docs/7.x/eloquent#defining-models){:target="_blank"}
- Query [Builders](https://laravel.com/docs/7.x/queries){:target="_blank"} and [Scopes](https://laravel.com/docs/7.x/eloquent#query-scopes){:target="_blank"}
- [Collections](https://laravel.com/docs/7.x/eloquent-collections){:target="_blank"}

Creating model-specific Builder and Collection classes (by extending the base classes) can help simplify model classes and leverage the built-in power of Eloquent.  

Tim's presentation was basically about how code can be organized into various parts of the application in order to keep models and controllers clean, leverage existing features, and to create more appropriate domain language and code-readability.  Here's some additional reading from Tim on these subjects:
- [Giving collections a voice](https://timacdonald.me/giving-collections-a-voice){:target="_blank"}
- [Dedicated query builders for Eloquent models](https://timacdonald.me/dedicated-eloquent-model-query-builders){:target="_blank"}

---  

6:30PM EST  
Jack Ellis - [@jackellis](https://twitter.com/jackellis){:target="_blank"}  
"How We Scaled Fathom Analytics to Handle Billions of Requests"  

Jack shared how his website analytics company, Fathom, has been able to adapt and scale as it has grown.

Some takeways:
- don't prematurely optimize and plan for growth before it is necessary
- when scaling becomes an issue, use queues + jobs to lighten the strain on the database
- don't overcomplicate things trying to be clever

Some problems faced at scale, and how they were solved:
- moved from Redis to to ElasticSearch to handle scale of queries
- moved from MySQL/Postgres/etc. to DynamoDB to handle the issue of over-provisioning database resources while still needing infinite scale

---  

7:00PM EST  
Closing Remarks  

Although virtual, or rather as a result of being virtual, Laracon 2020 was [the largest Laravel conference so far with 5,500+ attendees](https://twitter.com/LaraconOnline/status/1298736195187478528){:target="_blank"}. I received a tremendous amount of value from the conference and would recommend it to anyone in the community that is able to go. I'm already looking forward to the next one, and hoping we can all meet in person at the Georgia Aquarium!  