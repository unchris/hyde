The Terrible Twos - Part 2: Your App Needs to Make Friends
=====================
Chris Cameron, Spiria

# Background

In the previous article, we discussed a phenomenon in young web applications that I'm terming 'The Terrible Twos' for its resemblance to young children beginning to assert their independence while still learning the skills needed to be independent. We talked about how we have rising expectations for a young web application, which can lead to resource exhaustion, misbehaviour, and "tantrums."

Today let's take a look at some of the strategies we can use to mitigate the challenges raised in the first article. In particular, we're going to look at giving your app some friends that can share some of the burden and improve behaviour and resource usage.

# Task Queues

A very popular method for off-loading work from your app is to use what is known as a Task, or Job Queue. A simple way to think of this is that it's a separate application that maintains a todo list of functions and their arguments.

This Task Queue application's sole purpose is to tick off todo items one at a time by loading up just enough of your web application's state/environment to execute a particular function as if it were being done by your web application, but detached from the request/response process. Some Task queues don't need any of your application state to run at all, which is ideal but not always achievable this early in your application's life cycle.

Common task queues for web applications include [Sidekiq](https://sidekiq.org/) for Rails, [Celery](http://www.celeryproject.org/) for Python, and [Exq](https://github.com/akira/exq) for Elixir. These task queues usually depend on a message broker application such as Redis or RabbitMQ to store the tasks and provide for many of the advanced options you can find, such as setting different priorities for different queues or retry options when a task fails.

Task Queues typically support some concurrency, which allows more than one task to be executed at the same time. This can be done by having worker processes which receive Tasks from a Task Queue daemon application, or by having separate Task Queue applications which have different queues to pull jobs from.

# Daemons

Another popular method for off-loading work from your app is to use one or more Daemons. If you have done any systems programming you probably are already familiar with some common daemons already running on typical OS implementations, such as the printer or notifications daemon. Even casual users of operating systems may be aware that applications like Dropbox work because they have a synchronization daemon running at all times.

For web applications, you can think of a daemon as a highly focused application which listens for incoming events, processes them, and then goes to sleep waiting for more events. How this is implemented depends on design requirements; daemons can listen to anything from TCP sockets to RabbitMQ messages to local filesystem events.

A common pattern in Microservice architectures is to use a message broker (e.g., Redis, RabbitMQ) that the web application sends messages to and the daemon listens to, typically on specific queues or exchanges so that each daemon receives the appropriate message.

Another common pattern is a daemon which listens for new files in a special disk folder, generating a thumbnail image or scraping metadata and storing it in a database.

# Cron Jobs / Scheduled Tasks

Cron jobs are similar to daemons in that they perform a specific well-defined function, but in this case instead of being queued up somewhere, they are run on a specific time schedule. The most popular way to do this is *probably* `cron` which comes with most Linux distributions but there is also support for scheduled tasks baked in everywhere in the stack these days including the operating system, your web framework, Task Queues, and Kubernetes. Even cloud providers such as Heroku and AWS have special services for running scheduled tasks.

# Use Cases

So what are some of the ways we can use these strategies to help our web app grow up?

A quick question first: do you have any application features that are time-consuming? For example, uploading/downloading files, generating PDFs, performing searches, imports/exports?

These features should probably be off-loaded to a Task Queue. In Python you might use Celery, in Rails you might use Sidekiq. You could also spin up a RabbitMQ (or other message queue) and send task descriptions as messages to daemons that wait for jobs yourself (popular in microservice architectures). In a couple of instances you might need to do something more specific.

For example, in the case of uploading/downloading files, perhaps you should invest in using Amazon S3 which has features that allow you to give your users the perception that they are uploading/downloading files to/from your application, but actually they are interacting directly with S3. Some of the cloud providers even have options to allow batch downloads in a ZIP file, further offloading the work from your application. This can be useful for importing as well, since you can send the import file(s) to S3 and then get a background job to process the import.

For generating PDFs or performing exports, you could return immediately to the user with a way for the user (or your web client) to get status on the progress, while the job is completed in the background. Instead of the user waiting around for a download link or clicking refresh 1000 times you could promise to e-mail them a link to the completed file later. The HTTP code 202 Accepted was created for this class of response.

A great use for cron jobs is to:

- perform maintenance tasks such as deleting files or database records that have been marked for deletion.
- creating reports for financial purposes, such as end-of-day totals, or calculating discrepancies between AR and AP
-

# Next

In this article we.

In Part 3, we'll discuss ways to optimize code and configuration to speed up the main work, cache common requests, and add some basic concurrency (horizontal scaling) to the mix.
