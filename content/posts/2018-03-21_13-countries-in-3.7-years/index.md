---
title: "13 Countries in 3.7 Years"
author: "Daylen Yang"
date: 2018-03-21T15:31:01.947Z
lastmod: 2024-11-11T21:47:07-08:00

description: ""

subtitle: "I was scrolling through Twitter the other day and I came across this tweet:"

image: "/posts/2018-03-21_13-countries-in-3.7-years/images/1.png" 
images:
 - "/posts/2018-03-21_13-countries-in-3.7-years/images/1.png"
 - "/posts/2018-03-21_13-countries-in-3.7-years/images/2.png"
 - "/posts/2018-03-21_13-countries-in-3.7-years/images/3.png"
 - "/posts/2018-03-21_13-countries-in-3.7-years/images/4.png"
 - "/posts/2018-03-21_13-countries-in-3.7-years/images/5.png"
 - "/posts/2018-03-21_13-countries-in-3.7-years/images/6.png"


aliases:
    - "/13-countries-in-3-7-years-abc5b8e22a3c"

---

I was scrolling through Twitter the other day and I came across this tweet:

> [](https://twitter.com/dcurtis/status/966871883462320128)


Conquer Earth looked pretty cool, but as Dustin himself said, to get it to work you actually need to _manually input every place you’ve been to_. Sad!

Since I’m a huge dork, I check-in to every restaurant, shop, park, etc. I visit using an app called Swarm (by Foursquare), and I’ve been doing this for years. And guess what? Swarm/Foursquare has an API to fetch every place you’ve ever checked in to. So I decided to build my own map-with-dots-indicating-where-I’ve-been, and in a couple of hours, I was able to whip up this:
![image](/posts/2018-03-21_13-countries-in-3.7-years/images/1.png#layoutTextWidth)
Wow, a map with dots! Amaze

You can check it out live here: [https://daylen.com/map/](https://daylen.com/map/)

### Behind the scenes

The MVP: display the lat/long points of the places I’d checked in to, with no additional bells and whistles. My front-end was just one large embedded Mapbox map. To access the places I had checked in to from the front-end, I created the [/api/venuehistory](https://daylen.com/api/venuehistory) endpoint on my node.js/Express backend. Eventually I would implement caching and background refresh of the Foursquare endpoint, but to start I manually performed the OAuth token exchange, downloaded the JSON response from the Foursquare API, and stuck it in the root directory of my server to play with. The first iteration looked like this:
![image](/posts/2018-03-21_13-countries-in-3.7-years/images/2.png#layoutTextWidth)
I call this theme “bee”, as in it stung my eyes so hard it died quickly

Yuck! Those yellow points really blend in with each other. The fix here is to vary the point color based on the place category. And after tweaking the map colors a bit, it looked more like this:
![image](/posts/2018-03-21_13-countries-in-3.7-years/images/3.png#layoutTextWidth)
Mmmm, purple

Definitely looks better, but the dot color doesn’t have any semantic meaning. This is because although they technically correspond to Foursquare’s [venue categories](https://developer.foursquare.com/docs/resources/categories), the categories are actually too detailed—a “burger joint” and “fast food restaurant” are apparently not the same thing, and thus would get different colors. Luckily, Foursquare provides a category hierarchy, so we can look up the parent category for each place and then assign one of six semantic colors to the dot.

![image](/posts/2018-03-21_13-countries-in-3.7-years/images/4.png#layoutTextWidth)
All the colors of the rainbow



Next up was the sidebar. Here, I took, err, significant inspiration from [Gyroscope](http://life.daylen.com), which is a pretty neat quantified-self data visualization website.
![image](/posts/2018-03-21_13-countries-in-3.7-years/images/5.png#layoutTextWidth)
I’m not sure why we get different numbers on the same data

Looking at a bunch of dots on a map is cool, but what’s even cooler is being able to hover over dots to see the place names. Luckily, the Foursquare API provides place names and categories. My friend [Kevin](https://kevinchen.co) pointed me to the CSS backdrop-filter property, which which lets me apply a subtle blur to the popover. Mmmm.

![image](/posts/2018-03-21_13-countries-in-3.7-years/images/6.png#layoutTextWidth)
Blur courtesy of the CSS backdrop-filter property



The final step was to swap out my static JSON file with background crawling of the Foursquare API. When I visit a new place, I want that to show up automatically on the map without having to go download the JSON manually and stick it in the repo. But the Foursquare API is a bit slow to return the list of all the places you’ve been to when you ask for it. So I pull from the Foursquare API every 5 minutes and store the result in Redis, and for my own API I serve responses from the Redis cache.And that’s about it! You can check it out for yourself here: [https://daylen.com/map/](https://daylen.com/map/) Be sure to zoom out to see the world view!
