---
title: "Introducing Converscope"
author: "Daylen Yang"
date: 2020-04-05T23:42:01.207Z
lastmod: 2024-11-11T21:47:17-08:00

description: ""

subtitle: "A new way to analyze your personal chat history"

image: "/posts/2020-04-05_introducing-converscope/images/1.png" 
images:
 - "/posts/2020-04-05_introducing-converscope/images/1.png"
 - "/posts/2020-04-05_introducing-converscope/images/2.png"
 - "/posts/2020-04-05_introducing-converscope/images/3.png"
 - "/posts/2020-04-05_introducing-converscope/images/4.png"
 - "/posts/2020-04-05_introducing-converscope/images/5.png"
 - "/posts/2020-04-05_introducing-converscope/images/6.png"
 - "/posts/2020-04-05_introducing-converscope/images/7.png"
 - "/posts/2020-04-05_introducing-converscope/images/8.png"
 - "/posts/2020-04-05_introducing-converscope/images/9.png"
 - "/posts/2020-04-05_introducing-converscope/images/10.png"
 - "/posts/2020-04-05_introducing-converscope/images/11.png"


aliases:
    - "/introducing-converscope-fe721b3c5b50"

---

#### A new way to analyze your personal chat history

{{<youtube AhPRAmjeMyk>}}

<!-- converscope teaser trailer -->

In the last decade, I’ve sent and received **over 460,000 messages**—that’s an average of 126 messages per day! For photos, Google and Apple have both introduced a “Memories” features in their respective photo apps, which lets you look back on photos that the almighty machine learning algorithm has deemed meaningful to your life. Why not do the same for messages?

Inspired by [Chandler’s similar project](https://medium.com/hipster-data-science/pretty-colors-5c98907a39f0), for the last couple of months I’ve been working on **an open source tool called Converscope** ([github](https://github.com/daylen/converscope), [interactive preview](https://converscope.daylen.com/)). It pulls data from your Facebook chats and iMessages and shows you **who you message the most**—over the last year, particular time periods of your life, or over all time. You can drill down into a particular thread and see interesting metrics like

*   a **histogram of message counts** per day
*   the **total number of characters sent** per person (a proxy for how talkative each party is)
*   your **longest streak** and when it ended, a la Snapchat
*   the **most used emoji** per person
*   **_Blast From The Past_**™: a random, potentially cringy message from your past
*   the **TF-IDF top tokens** for the person/group chat. TF-IDF stands for _term frequency-inverse document frequency._ It automatically surfaces the topics that you talk about a lot in _this_ chat that you don’t talk about with other people/groups.
![image](/posts/2020-04-05_introducing-converscope/images/1.png#layoutTextWidth)
This is what a Converscope thread report looks like. It appears the hot topic of conversation with this friend is Apple and Apple products.

I’ve anonymized names and released **an interactive preview at** [**converscope.daylen.com**](https://converscope.daylen.com), so you can browse around and see what it looks like. (Certain features such as _Blast From The Past_™ and TF-IDF Top Tokens are not available in the preview, for privacy reasons.) If you’re one of the top 20 people I message the most, you can try to figure out what SHA-1 hash you are 🙃. And as I mentioned, [Converscope is open source](https://github.com/daylen/converscope), so **you can run this analysis on your own data**! Instructions are [in the README](https://github.com/daylen/converscope/blob/master/README.md), and please do let me know ([e.g. via Twitter](https://twitter.com/daylenyang)) if anything doesn’t work.

The rest of this post is split into two sections: _Interesting Findings_, where I talk about some of the interesting things I learned by poking around this data, and _Building Converscope_, which is about the visual and technical design decisions I made.

### Interesting Findings

#### Group chats

If you pop open [converscope.daylen.com](https://converscope.daylen.com) and flip over to the Group Chats tab, the first two chats you see are these mega-chats with over 20K messages each:
![image](/posts/2020-04-05_introducing-converscope/images/2.png#layoutTextWidth)
the birth and death of group chats

An artifact of the pre-Slack era, [_THE H@BMIND_](https://converscope.daylen.com/detail/b641281131a487918c15f2bf0b3d6a6394d6dd69) was the officer chat for Hackers at Berkeley, a club I joined as a wide-eyed freshman. We were primarily an educational club: [we hosted events](https://www.facebook.com/pg/HackersAtBerkeley/events/?ref=page_internal) where we taught eager students things like how to build a web app, or how to use git. The chat was where we did event planning, and the ebb and flow of message counts corresponds to the school seasons—for example, lots of activity in fall 2013 and spring 2014 versus a drop in summer 2014. You can also see that the club never made a recovery from the summer 2015 slump: some of the more senior members graduated or moved onto TAing.

As for [_Laurel Grove crew_](https://converscope.daylen.com/detail/0c30a2747ac7de3b1ff2994e138be332dd78e498)_:_ in the summer of 2015, I interned at Facebook, and this was the group chat for a set of us who lived at corporate housing at Laurel Grove (such creative naming). We were a chatty bunch, and attempts were made at keeping the group alive after the summer ended. But that petered out by the end of 2016. Every group chat eventually dies.
> Every group chat eventually dies.

The predominant flavor of group chat varies based on time period. In my college days, there were the classics like the aptly named [_presentation due sunday 10pm!_](https://converscope.daylen.com/detail/cbf19e177c30ee01ec8e6d933a77ec06d87527eb) (which did in fact send the most number of messages on Sunday, November 27, 2016), [_CS162 project group_](https://converscope.daylen.com/detail/0d139a1209c3b8dbd6c1ba39f6fd8dad1e742373), and [_189 shittrs_](https://converscope.daylen.com/detail/147bb4e24ac9c57f2c8f3ceb09cf272ba50203dc). These group chats shuttered after the respective classes finished.
![image](/posts/2020-04-05_introducing-converscope/images/3.png#layoutTextWidth)
gee, I sure do wonder when the presentation was due

Also in that same time period were several of group chats dedicated to trips: [_Thailand 12/31–1/13_](https://converscope.daylen.com/detail/643703c250b46f11b54f8e079492937c5d828234) __ ([video recap!](https://youtu.be/pbSuCUpyvs8)), [_NYC 8/15–8/21_](https://converscope.daylen.com/detail/b3010b04c42251d97354e11e39666e840e650fd3), and [_Roadest Trip 2017_](https://converscope.daylen.com/detail/e98c205cc1dc0246dd70686100dffcbf508b8fea) (a spring break road trip [across the PNW](https://youtu.be/NHKv9kkmu2o), after [_roader trip 2k16: A Profoundly Religious Experience_](https://converscope.daylen.com/detail/fb1e5d0cecee74140e867cc862a8d1160289c1c0) and the original road trip in 2015, which lacks a group chat). Here, the _Most Used Emoji_ metric comes in handy to distinguish the trips. (In Facebook Messenger, you can set a default emoji for a group, and it turns out people tend to smash that button a lot.)

![image](/posts/2020-04-05_introducing-converscope/images/4.png#layoutTextWidth)
the elephants in Thailand were very cute


![image](/posts/2020-04-05_introducing-converscope/images/5.png#layoutTextWidth)
I was the most prolific user of the car emoji in the Roadest Trip chat, sending it 30 times



After graduating, a larger percentage of group chats shifted to be trip-oriented. It turns out that group chats centered around trips follow a fairly standard formula:

1.  There is an intense planning phase __ several months or weeks prior to the trip
2.  Next, the actual trip __ generates the majority of the messages
3.  Finally, there may be __ one last hurrah where the Splitwise is closed out or photos are shared

Example of this phenomenon: [_new orleans 2019_](https://converscope.daylen.com/detail/6ef48b5f2647da3ca69c00fee3a0c8580106b764), [Ski + Sundance 2019 subgroup](https://converscope.daylen.com/detail/9363c3913d884f32f8bf18cb0c1b66c01a4da53e), [🏠🏠🏠 Sundance2020.house](https://converscope.daylen.com/detail/fd81012b57992c00971e4404a760acffd856d061).
![image](/posts/2020-04-05_introducing-converscope/images/6.png#layoutTextWidth)
the three phases of a trip-oriented group chat: intense planning, the actual trip, and one last hurrah

The _Longest Streak_ metric consistently identifies the actual trip duration. Modeled after Snapchat streaks, _Longest Streak_ requires that at least one message be sent every day for consecutive days. So if I’m looking at the confusingly titled [_You and Taco_](https://converscope.daylen.com/detail/79b7f3c4af01b8bb6aa2378aedf56df41b5b3dee) chat, I can see that perhaps this chat was about the [post-graduation Europe tour](https://www.youtube.com/watch?v=5_3i1m4MJdY) that my college friends took in 2017, along with our diminishing attempts to stay in touch afterwards.

#### Direct messages

Obviously, it’s more exciting when you can actually see the names as opposed to just XXX everywhere. I’ve got a private instance of Converscope with [STRIP_PII (personally identifiable information) set to false](https://github.com/daylen/converscope/blob/master/constants.py#L33), but in this post I’ll refer to everyone by their (salted!) SHA-1 hash.

Using the “Sort by…” dropdown and selecting the College option, it’s interesting to see who from college I’ve kept in touch with and who have fallen by the wayside. Just contrast the histograms for my longtime friend [414129b](https://converscope.daylen.com/detail/414129b63cf610545df173939c801232706c3172) (with whom I have a 59 day streak!) to [368db00](https://converscope.daylen.com/detail/368db0084b5587e0ecde3575c76c3c7f186ca579).
![image](/posts/2020-04-05_introducing-converscope/images/7.png#layoutTextWidth)
a friendship that blossomed and faded

Equally interesting is seeing the new friends I’ve made _post-_college. Turns out, there are a lot! [b01f615](https://converscope.daylen.com/detail/b01f6152b4bc9dcfd10f16816ff3280d86a599c8), [bd417c1](https://converscope.daylen.com/detail/bd417c1475c0addf0fcc45388051ced83bd7a940), and [8b20864](https://converscope.daylen.com/detail/8b208640130641e7da7ff43902fa3d2023952039), just to name a few. Here, the _TF-IDF Top Tokens_ feature shows how most of these friendships are oriented around specific activities like cycling and photography:
![image](/posts/2020-04-05_introducing-converscope/images/8.png#layoutTextWidth)
“fcc” stands for [fatcake club](https://www.instagram.com/fatcakeclub/)


![image](/posts/2020-04-05_introducing-converscope/images/9.png#layoutTextWidth)
we both used to climb


![image](/posts/2020-04-05_introducing-converscope/images/10.png#layoutTextWidth)
film, leica, SF. what a mood

And finally, to close out the _Interesting Findings_ section, here’s a fun _Blast From The Past_™ to when I [faceplanted when riding my Boosted Board](https://medium.com/@daylenyang/ten-months-with-the-boosted-board-570013146a00):
![image](/posts/2020-04-05_introducing-converscope/images/11.png#layoutTextWidth)
### Notes on building Converscope

#### Frontend

The [Converscope frontend](https://github.com/daylen/converscope/tree/master/converscope-react) is a React app and is pretty much just a fancy JSON viewer. I’ll just highlight two of my favorite features. First up is the hover animation for the card design, inspired by [the card design in Apple TV](https://youtu.be/0qwALOOvUik?t=3627). On hover, the card gains a drop shadow and also subtly grows in size.




mmm, drop shadows



Second, I’m pretty proud of the dark mode theme, which automatically kicks in if you switch to dark mode on your iOS or macOS device. (This is thanks to the prefers-color-scheme CSS Media Query!)




mmm, dark mode



You can try these for yourself at [converscope.daylen.com](https://converscope.daylen.com/).

#### Backend

There’s two parts to the Converscope backend: [there’s a script](https://github.com/daylen/converscope/blob/master/data_merge.py) that parses and merges the Facebook and iMessage data formats into easy-to-understand [Inbox, Conversation, and Message protos](https://github.com/daylen/converscope/blob/master/chat.proto). From there, computing the metrics to display is a matter of [filling a dictionary](https://github.com/daylen/converscope/blob/master/server.py#L188-L205).

The code to parse through iMessage data is probably the ugliest. Just look at [this SQL query](https://github.com/daylen/converscope/blob/master/data_reader_imessage.py#L51-L55) which has both an inner join and a left join, plus some fun [inline date parsing](https://twitter.com/daylenyang/status/1201016223464509440)!

A special thanks to [Kevin Chen](https://twitter.com/kevinchen) who pointed out that I should add a [salt before hashing the sender name](https://github.com/daylen/converscope/blob/master/data_merge.py#L33) when generating conversation IDs—otherwise it would be trivial to deanonymize the preview website by running a dictionary attack on an easily obtainable friend list (e.g. Facebook).

For TF-IDF, setting [maximum document frequency to 0.2](https://github.com/daylen/converscope/blob/master/analysis.py#L41) really boosted the quality of the displayed tokens. That means that if a token appears in more than 20% of chats, it is discarded. This helps eliminate common words like “the” and “and.”

In a similar vein, to ensure good quality for _Blast From The Past_™, I require that a message be [at least 30 characters](https://github.com/daylen/converscope/blob/master/server.py#L130) (otherwise you just get a lot of “lol,” “haha,” and “yeah”).Wow, you made it this far! Converscope has been a long time in the making and I’m happy to finally put it out there. I encourage you to [check out the interactive preview](https://converscope.daylen.com/) (flip over to “Group Chats” so you don’t see XXX everywhere) and if you’re handy with the command line, to [run it on your own data](https://github.com/daylen/converscope).
