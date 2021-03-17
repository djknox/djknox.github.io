---
layout: post
title: Laracon Online Winter 2021 Recap
permalink: "/laracon-winter-2021/"
date: 2021-03-17 18:41 -0400
---
Laracon Winter 2021 was held virtually this year, continuing the shift from in-person events to Zoom-based presentations. The Laracon team has plenty of previous experience delivering web-based events with Laracon Online, so this year's Winter conference was another success.

The format consisted of eight speakers giving hour-long presentations, which allowed the speakers to deep-dive on their various topics. As a result, a lot of code-reading and live-programming was done so I would highly recommend watching the videos when they are available. Here, I'll provide my notes that might be helpful for selecting which videos/presentations to look into more closely.

---

10:00AM EST  
Mohamed Said - [@themsaid](https://twitter.com/themsaid){:target="_blank"}  
"Diving the Queue"  

Queues are used to move time-intensive tasks out of the normal request loop and asynchronously process them in order to speed up responses.  

`config/queue.php` contains a `connections` configuration array that defines the connections to backend services like Redis, Beanstalk, Amazon SQS, etc.  
- any given connection may have multiple queues  

`app/Jobs` contains all queueable jobs
- generate a new job class with `php artisan make:job NameOfJob`
- the generated class will implement `Illuminate\Contracts\Queue\ShouldQueue` interface

Enqueueing  
- converts a task/job to string form
- sends to storage system (database, Redis, Amazon SQS, etc.)

Dequeuing  
- start worker, gets jobs from queue store
- resolve back into object instance
- execute `handle()` on job

Dispatching:  
- single jobs
- chains of jobs
    - one after the other
- batches of jobs
    - execute in parallel

Starting a worker:  
```
php artisan queue:work --queue=high,low
                       --timeout=60
                       --memory=128
```
- `--queue=high,low`
   * start a worker that verifies that all of the `high` queue jobs are processed before continuing to any jobs on the `low` queue
- `--timeout=60`
   * exit with an error if a job is processing for longer than `60` seconds
- `--memory=128`
   * exit with an error if a job reaches the memory limit of `128` megabytes


---  

11:00AM EST  
Christoph Rumpel - [@christophrumpel](https://twitter.com/christophrumpel){:target="_blank"}  
"The final Laravel Service Container talk"

The Service Container:
- manages class dependencies  
- performs dependency injection
   * dependencies are put into the class via the constructor/"setter" methods  

Advantages:  
- better dependency handling
- less responsibilities
- no duplicated code
- change control flow (Inversion of Control/"IOC")

How:
- auto-resolving
- explicit binding
- facades

Automatic dependency injection and facades allow for rarely needing to manually bind/resolve anything from the container


---  

12:20PM EST  
Bobby Bouwmann - [@bobbybouwmann](https://twitter.com/bobbybouwmann){:target="_blank"}  
"Routing Laravel"

Agenda:
- basic routing
- routing cycle
- route model binding explained
- another use case for the router
- basic practices

Route files:
- `web.php`
- `api.php`
- `channels.php`
- `console.php`

Ways to define a route:  
- basic controller
- invokable controller (single action controller)
- callback (closure-based)

Viewing a list of routes:  
- `php artisan route:list --compact`  
- `php artisan route:list --columns=method,name,action,middleware`

Fallback routes:  
`Route::fallback` method defines a route that will be executed when no other routes match the request
- should always be the last route registered

`App\Http\Kernel.php`: central location that all requests flow through  
- `$middleware`
   * middleware run during every request
- `$middlewareGroups`
   * route middleware groups
- `$routeMiddleware`
   * route middleware that can be assigned to groups/used individually

Route-Model Binding  
- can add `where` clauses to routes that use route-model binding
   * `where()`
   * `whereNumber()`
   * `whereAlpha()`
   * etc.
- `missing()`
   * if no model is found, supply a callback to execute


---  

1:20PM EST  
Taylor Otwell - [@taylorotwell](https://twitter.com/taylorotwell){:target="_blank"}  
"Laravel Update"

Release cycle change - one major release every year (typically towards the end of the year)
- Laravel 9 coming in September 2021

Laravel Sail (Docker environment for Laravel)  
- can now choose which services to install rather than getting all during install
- can now use Meilisearch as a service

Laravel Breeze
- lightweight authentication system (login, regisration, password reset, etc.)
- starter/lighter version of Laravel Jetstream
- also offers an Initia.js frontend implementation (powered by Vue)

Metered Billing support added to Laravel Cashier

Testing improved for JSON
- `Illuminate\Testing\Fluent\AssertableJson`
- `$response->assertJson(function (AssertableJson $json) {...`

Parallel Testing
- `php artisan test --parallel`

New `ThrottlesExceptions` middleware for throttling job exceptions
- throttle the number of exceptions that will be thrown during a certain time period
- example: a third-party API goes down, can throttle the inevitable exceptions that will be thrown from job(s)

Laravel Octane  
- goal: speed up Laravel
- context:
   * with PHP, the entire app is rebooted for every request
   * could speed up PHP apps by booting app once and feeding it requests
      - downside: memory leaks, etc.
- Swool and RoadRunner are two tools that do this for PHP applications
- Laravel Octane is a package that supports tools like Swool/Roadrunner and configures what happens in the app during the request lifecycle
- `php artisan octane:start --workers={number of workers}`
- `php artisan octane:reload` - will need to restart server when making code changes
- `--watch` will do hot-reloading for restarting server after code changes


---  

2:40PM EST  
Marcel Pociot  - [@marcelpociot](https://twitter.com/marcelpociot){:target="_blank"}  
"Understanding Laravel broadcasting"  

What is broadcasting?
- making an event accessible outside of app

`BROADCAST_DRIVER` configurations available: `log`, `pusher`, `ably`, `null`, `redis`


---  

3:40PM EST  
Miguel Piedrafita - [@m1guelpf](https://twitter.com/m1guelpf){:target="_blank"}  
"Understanding Foundation: What ties everything together"  

Deep-dive into Laravel foundation codebase


---  

5:00PM EST  
Caleb Porzio - [@calebporzio](https://twitter.com/calebporzio){:target="_blank"}  
"Doing small things with Livewire & Alpine"  

Examples:
- form fields that depend on other form fields
- simple buttons that submit forms and change according to the response
- delay loading of expensive charts until the rest of the page loads
- dispatching jobs with a button and showing progress

Note: this presentation focused solely on Livewire  


---  

6:00PM EST  
Nuno Maduro - [@enunomaduro](https://twitter.com/enunomaduro){:target="_blank"}  
"Laravel's Artisan Console component"  

`Illuminate\Console`

Live-coding presentation on rebuilding Laravel Artisan from scratch
- Laravel Zero, PHP Insights, and Pest are all tools built on top of Artisan Console

