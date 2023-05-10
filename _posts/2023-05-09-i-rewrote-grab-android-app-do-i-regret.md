---
layout: post
title:  "I re-wrote Grab Android App. Do I regret?"
date:   2023-05-09 21:00:00 -0500
categories: blog post
---
Back in 2017, I had a great chance to rewrite a popular Android application that had over 30 million users.  Of course that was a tremendous effort of the great team of engineers. But I had an opportunity to influence business decisions and then design architecture, and define tech that was fundamental for the future growth of the mobile application. But these were one of the hardest years in my career so far.

In 2017 I just joined the Android team at Grab - one of the biggest ride-hailing services in South East Asia that beat Goliath (Uber) in Singapore and across SEA (South East Asia) back in 2018 and now it’s the biggest Everyday Everything Southeast Asia's leading superapp. But in 2017 it was a mess at least the Android part of the service. It needed some love. 

In short: the whole app consisted of 2 Activities with 4k lines of code each and communicated through global variables, I think that says it’s all. You fix a price tag appearance and that breaks the taxi-booking button. So what would a normal engineering suggests? yes… careful refactoring. So we started.

![before_rewrite](/assets/images/before_rewrite.png)

Split into smaller components, move code around, run, it works, repeat. Except for "run → it works" transition was not always a thing. Highly coupled code and absence of clear requirements for features which is normal for a scrappy startup that has grown up fast is a recipe for a “life in not like a book” type of situation. So while refactoring you end up rewriting the whole activity and another one since those are coupled as I mentioned, and it would not pass any manual Quality Assurance, unit tests were out of the question with highly coupled code and integration tests were not yet as affordable as today. 

2015-2017 were years of MVW (Model View Whatever) architectures rise, Kotlin was around and was close to become an official language for Android development (Google IO May 2017), Daggerization of the Android world (not Hiltization yet) was a thing already, and Reactive programming was all about RxJava and memoization (pun intended) of how combine() works internally and on which thread emits results (no LiveData, Flow or anything else). Oh, how deep I was on that hype train looking back now. I wanted Everything Everywhere All at Once. So do other teammates - cause we all learn by doing, and we all wanted to learn.

So if anyway we are rewriting the app while doing refactoring why not do a proper re-write? Is it logical? Except the business have agreed on refactoring not a rewrite. So how do you usually work around this? Yes, let’s do refactoring, build new features and do rewrite all at the same time. Is THIS logical now? Nah, we were not the first we won’t be the last. Business was happy on that promise, we got our rewrite, all happy except for the “people/hours” metric, but it’s a startup right, they call it a hustle for a reason. 

So the team continued building the product and 3 engineers including me seated in a room brainstorming ideas. What are the problems we are trying to solve, what tools do we have/want, and what do we anticipate the app will do? The main product idea at that time was seamless multimodal transportation - in simple words taxi-bike-bus-scooter trips. I still hear the echo of some product manager pitching this idea to management in meeting room again. So we created whole new architecture that composited MVP or MVVM (choose and pick) for features and Booking Core at the heart of taxi experience.

![booking_core](/assets/images/booking_core.png)

What went wrong? Uber released blog posts on RIBs or RIBlets architecture and gave an influx of bad ideas with Dagger at the heart of your Multimodal disaster with Kotlin and kapt. We read it like this: beautiful multimodal architecture that solves all your problems for feature teams structured development, isolation, quick builds, team collaboration… mmm perfect. Except for an unspecified mountain of problems with a learning curve, build times and “Do you want 1000 modules by the end of the year” developer life.

But young and brave we were. Small protype worked and in 2018, sliglty over 7-8 months in we were almost done. A couple of new features were built twice (refactored into new architecture), and old code was rewritten from scratch… from scratch, I repeat (that’s the definition of re-write). Next phase is testing. QA gets the build - we are waiting with temptation. 

Today I hear engineers often say why the f* they need that many developers there are only 2 screens and a map I can build it in a week/month/3 months. I was there too, and you know I did not like to learn it the hard way. Important note here is that I joined in 2017 and a bunch of people who were rewriting joined after and there were no product specs or docs, so you get what you see. 

QA lead showed me their TestRails (all use cases to test) and showed me rewritten app on a screen of an Oppo phone. “No surprise there is an issue, for sure some weird custom OS sh* again”. “Hello, world!”, Half of the screens like error handling, and flows with secondary business features were just missing. Cause we never heard, knew or thought about those. +6 months of intensive burn-down charts, polishing and literally restoring features. Ah, and building other new features on the go - the main one was app redesign. And finally, we incremented Major Version Digit. Hello, new world - it was!

That was THE experience I would say. New design, new architecture, all new bells and whistles, bleeding edge technology (at least back then). We did it and that was a relief. Until the next product idea Everyday Everywhere Superapp.

Do I regret it? I don’t know. I gained experience and skills, I could have tried a roller coaster instead but that would not be that scary. I learned a lot in working with stakeholders, leading an effort, working till midnights :), and tickling nerves cause we could have lost a market because of decisions that I had made or influenced. After 6 years it seems like a big win for my personal growth and the personal growth of each team member. 

Was it beneficial to the business? Probably yes, I was there while it became a leading Superapp in the region and building blocks were put back then. It has 180 million users today.   

Could that have been done the other way? Definitely yes, but life is not pre-calculated, and it had what it had. 

Thanks for reading it till the end. If you are at that moment thinking to rewrite, think twice or better seven times and if that’s still yes - go ahead and enjoy that experience! And you know what… check your product before rewriting, who knows what you might miss?

Would you rather rewrite or refactor?
