---
title: "Journey 2016: Postmortem, featuring the Traveling Salesman Problem"
author: "Daylen Yang"
date: 2016-10-14T19:19:55.043Z
lastmod: 2024-11-11T21:47:01-08:00

description: ""

subtitle: "Running is hard when there are people in your way"

image: "/posts/2016-10-14_journey-2016-postmortem-featuring-the-traveling-salesman-problem/images/1.jpeg" 
images:
 - "/posts/2016-10-14_journey-2016-postmortem-featuring-the-traveling-salesman-problem/images/1.jpeg"
 - "/posts/2016-10-14_journey-2016-postmortem-featuring-the-traveling-salesman-problem/images/2.jpeg"
 - "/posts/2016-10-14_journey-2016-postmortem-featuring-the-traveling-salesman-problem/images/3.jpeg"
 - "/posts/2016-10-14_journey-2016-postmortem-featuring-the-traveling-salesman-problem/images/4.jpeg"


aliases:
    - "/journey-2016-postmortem-featuring-the-traveling-salesman-problem-a94adb32edc8"

---

#### Running is hard when there are people in your way

Last weekend a couple friends and I played in [Journey to the End of the Night (San Francisco edition)](https://www.facebook.com/events/1778922165714289/) for the first time. For those who aren’t familiar with it, Journey is a city-wide game of virus tag, in which you try to visit as many checkpoints as possible without getting tagged. (If you get tagged, you become a chaser.) Here’s what went down.

Since we were cheapskates, we didn’t pay for priority map access and got our maps right before the game started. The map looked like this:
![image](/posts/2016-10-14_journey-2016-postmortem-featuring-the-traveling-salesman-problem/images/1.jpeg#layoutTextWidth)
There are 6 checkpoints of varying point values that have safe zones around them, which protect you from chasers. There are also 9 checkpoints that don’t have safe zones, worth 1 point each. We decided that those weren’t worth the trouble, so this boiled down to solving an instance of the TSP (traveling salesman problem) through the 6 safe zones. This is the plan we came up with:
![image](/posts/2016-10-14_journey-2016-postmortem-featuring-the-traveling-salesman-problem/images/2.jpeg#layoutTextWidth)
The Plan™

Looks pretty solid, right? Here’s what we actually did:
![image](/posts/2016-10-14_journey-2016-postmortem-featuring-the-traveling-salesman-problem/images/3.jpeg#layoutTextWidth)
Uh, we took a few detours

Whoops, that looks nothing like the plan! A couple chasers outside the Fort Mason safe zone caused us to change course and end up at the 2 point checkpoint in the middle of the map. That pretty much trashed our nice minimum cost TSP solution. We tried to pick up the pieces by finishing up the west side of the map and bussing over to the east side. Unfortunately, the bus stopped two blocks from the 5 point safe zone on the east side, and we were ambushed at an intersection before we could make it in. Sad!Looking back, what did us in was the constraint of playing in a busy city with tons of people on the sidewalks and tons of moving cars in the streets. It’s much nicer to be pursued down a relatively empty side street, so you don’t have to weave around people or be that concerned about cars smashing into you when you cross intersections. It’s even better you’re being chased up a steep hill and you’re more fit than your pursuers, which is what happened earlier in the day. By contrast, the sidewalks at Columbus and Broadway (where we got tagged) were packed with people, which made it more difficult to maneuver. In action movies, the crowd usually parts for the main character to make his/her great escape; to my dismay, that doesn’t happen in real life.

Hey, at least we got some nice views of the city. And for Journey 2017, we’ll be bringing our A-game.
![image](/posts/2016-10-14_journey-2016-postmortem-featuring-the-traveling-salesman-problem/images/4.jpeg#layoutTextWidth)
View from Alta Plaza Park. Shot on iPhone 7.
