---
layout: post
title: Google Locations (Find Google Places by Phone Number)
---

Last month one of my projects needed to search for information about businesses, particularly if you only have a phone number. My first thought was that the [Google Places API](https://developers.google.com/places/) might be helpful, but I was disappointed to discover that you can't query directly by phone number. Instead the lookup is broken into several steps, beginning with a generic query for places near a given geocoordinate and ending with several individual follow-up requests for more details about each Google Place, with each of those detail objects containing a business phone number.

I tried to find any Node modules that might be useful. The best option, [google-places](https://www.npmjs.com/package/google-places), hadn't been updated for two years, was just a basic wrapper for existing API endpoints, and even then it required some basic updates after Google introduced breaking changes. I did some updating work and [submitted a pull request](https://github.com/jpowers/node-google-places/pull/13) but the author was unresponsive. (It's kind of a bummer given that his module is downloaded over 1000 times each day!)

Because I wanted to go further and do lookups by phone number or address, I decided to fork the project and create my own Node module: [Google Locations](https://www.npmjs.com/package/google-locations). As far as I can tell, it's the most up-to-date Google Places module on NPM. In addition to re-implementing the Search, Autocomplete, and Details API endpoints, I also created two convenience methods: searchByAddress and searchByPhone. While searchByAddress combines the Google Geocode API with the Google Places API's Search and Details endpoints, searchByPhone has some special sauce that uses Google Places' Text Search endpoint to lookup Google Place details by phone number. (Note: Because each Text Search request is billed as 10 API requests, watch your API usage!)

This lets you search for Google Places via name and address (e.g. "Five Guys" near "Kingman, AZ") without having to know any geocoordinates, or search for any Google Places matching a phone number (e.g. "(305) 682-3300" returns a Google Place details object for the Macy's store at the Aventura Mall in Miami).

Things I'm proud of
* Test coverage is great -- and not just trivial tests to make it look like it's well-covered, there's actually significant testing around edge cases.
* This was my first time using [Vows](http://vowsjs.org/) for asynchronous testing. It was a lot more pleasant than [Mocha]()'s constant `before()` and `beforeEach()` usage for async.
* It was cool to fork someone else's project but significantly expand its functionality. (It was also a lesson in prodding maintainers to update their code via pull requests, albeit unsuccessfully in this case.)

Things I'd like to improve
* Results are less uniform than I'd like. Some methods return the full Google response object (complete with things like Google response status code) while others just return a Google Place detail object.

*[Google Locations](https://www.npmjs.com/package/google-locations) is now available on NPM*