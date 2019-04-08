The Terrible Twos - Part 3: Your App Needs Some Discipline
=====================
Chris Cameron, Spiria

# Background


# Improvements to your code and configuration (TODO: this section needs breaking down better)

Long request times can be caused by myriad things, but some common causes include:

- serving very large assets to clients directly (e.g., huge or many image files),
- not enabling http compression (e.g., with Gzip),
- "N+1 query" situations (common among database ORMs when joining tables together),
- the server you're running on is too slow (i.e., the $1/month plan isn't cutting it),
- classic problems like poorly performing nested loops or loading huge structures into memory

Some of these can be solved with configuration changes (slow servers, compression, image optimization), while others will involve improving the code. For the N+1 query problem there are usually libraries in each framework which can detect these (e.g., `bullet` for Rails), or you can use SaaS services like NewRelic to see which requests hit the database too many times.

One common optimization is to have something like NGINX sit in front of your application server, so it can serve your cacheable assets (CSS, JS, images, fonts, etc) leaving your application request queue open for regular requests. Alternatively, you could serve these from a Content Delivery Network (CDN) although this will cost you some additional money.

For searches, you are probably writing some custom hairball of `ILIKE` queries in postgres or a similar database. Where you go next will largely depend on what kind of complexity you would like to add. Some databases like postgres have extensions that let you do text document search more efficiently (and featurefully too), but will require you to invest in your database skills. Other solutions could involve learning about Elastic search, but this will require more learning on your operations side as you'll have new applications to keep running. Or alternatively you'll have to host your database in one of the big managed providers and rely on their proprietary search capabilities. Each option has its own pros and cons and you should do your research before making a choice.

# Make use of more caching levels

Another common solution to dealing with request times is to cache objects that you would typically fetch from disk or database into an in-memory store such as Redis or memcached. For instance, many web applications have a bit of boilerplate that needs to be fetched on each request, typically from the backing database, such as user credentials (for auth) name, email, associated records, etc. This is often done to help the web client (and 3rd parties) receive enough basic information without having to constantly hit your server.

Although the HTTP cache will help for a lot of these requests, actually building the response in order to know whether to return HTTP 304 still takes time. At the application level, you can cache these frequently accessed objects in Redis or memcached, and then implement a fetching procedure that checks cache first and falls back to the database second. With some caching libraries, this can even be done transparently for you, by reusing your ORM's interface to do the cache-check-and-fallback procedure.

Of course this will introduce a new dependency that will need to be managed as well. And just like your web application your cache can develop its own terrible twos including memory exhaustion and too many requests etc. Make sure you are monitoring things in your cache, whether you deploy it yourself or make use of a hosted solution such as RedisLabs.

# Horizontally Scale (run more than one application)

If you're satisfied with the work you've done to reduce request times in the two previous categories, then you may also be ready to simply add more application servers to the mix. Each application server will run in its own process space, so this works well for frameworks that don't implement threading.

How you do this is going to depend on how you run your server and unfortunately out of scope for this article. If you're using something simple like Heroku you can drag a slider to pay for more "Dynos" which run your application, but if you're on something more complex you may need to consult your local ops people to make a change to Amazon Cloud Formation, or perhaps a Terraform script.

No matter how you do it, just know that this is not some kind of last resort - it's a very common and accepted practice, especially with applications built on frameworks like Rails or Flask et. al. since threading support is not as good in their underlying languages (Ruby and Python). Some companies avoid this by using JVM-backed versions of these languages but that's not my preference.

# Next

Future work, other reading.
