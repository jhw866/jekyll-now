---
title: "Habit Tracker: A Post-Analysis"
layout: post
---

## Introduction
I have only built one Android app before, called Tap-Tap-Tap, which was a final project for my Android class back in college. It was a pretty simplistic app. Like a typing test on a computer, you test how fast you would type on your phone. The app would keep track of all the text your currently typed. The text would highlight red when you got an incorrect letter and green when you got a correct letter. It has a database full of phrases and the ability to add your own. For learning android, it was a pretty fun project.

After the course, I was ready to explore more. Services, Broadcast Receivers, Networks, Android functionality in general interested me. I had one app that never really saw the light of day and wasn’t really exploring what I wanted to explore. Then I came upon the idea for building a habit tracking app.

This was the perfect idea! This would touch all the areas I wanted to learn, plus the idea is rather simple so understanding the project wouldn’t be difficult.

## Top 5 things I learned
### Services
I needed a Services because I needed some way to notify the user from a background thread, or when the app wasn’t open in front of the user. Services are pretty cool because you can do all these kinds of things from the background without having the app running in the foreground (aka, you can be in another app and still have your service running).

I initially started with IntentService. The service would be indirectly called from AlarmManager through a Broadcast Receiver, and would notify the user using the NotificationManager that the a habit needed to be completed. 
I ended finding that this solution wouldn’t work. When the app is in the background I would get an exciting exception stating I could run the service because newer versions of Android needed to use the JobScheduler. This sucked because older versions of Android still used regular services

JobIntentService to the rescue. 

JobIntentService is a backwards compatible class that allowed me to run the required work using the appropriate classes. When I enqueued work via the JobIntentService, newer versions of Android would use the JobScheduler while the older versions use a service.

### Room

If you didn’t know, Room is a persistence library in Android. It basically abstracts most of the SQLite functionality using annotations and interfaces. You can checkout more about Room here (https://developer.android.com/training/data-storage/room/)

The architecture overall is pretty easyt to understand. Saving habits was easy and updating existing tables was a breeze.
Third Party API’s	

The most important thing I learned here was how to add Third-Party libraries to the app. Most of the time all you have to do is update the build.gradle in your module or project and tada! There are times where the SDK needs to be adjust or something about the project needs to be updated but adding these libraries was painless. The only ones I used were Material dialogs for some pop-up dialogs and Butterknife.
Seriously, don’t reinvent the wheel.

### MVP Archtiecture

The android team at my company uses the MVP architecture so I thought it would be a good idea to learn what the architecture is like. Essentially MVP (Model-View-Presenter) is similar to MVC (Model-View-Controller) but MVP the Model is passed to the View via the presenter. The presenter also determines the view and ideally should contain no Android code (AKA no code that relies on the Android SDK).
While it was useful to learn this, I feel that MVP is a little overkill for just developing a fun app. At a minimum for proper MVP, you need to create 4 files for each fragment: 2 interfaces for the view and presenter, and 2 implementations of the interfaces. This adds a lot of overhead if I just want to add a new page.

Don’t get me wrong, this architecture is useful especially in a production environment. You can easily test the presenter to make sure the logic is right and decoupling the determining of views from their creation is great! But let’s just build something.

### Broadcast Receivers

I’ve never really interacted with Broadcast Receivers before and was one of the items I really wanted to try out. For simplicity sake, Broadcast Receivers are Android’s way of notifying your app when some event occurred. This can be helpful for notifying your app that the phone has just turned on so you can start some custom service.

During the development, I learned that Broadcast Receivers run on the UI thread, so any code executed needs to get done quickly. I used Broadcast Receivers in conjunction with the AlarmManager, which would be called via some Pending Intent which, in turn, would call a Service to be executed. I also created another Broadcast Receiver to be notified when there phone finished booting up. AlarmManager removes all the alarms when the phone is rebooted so I needed to know when the phone finished booting to add habits that required notifications to AlarmManager.

## Top 5 thing I could improve

### Setting Deadlines

Over the course of the project, a lot of things happened. I had a job interview with another company, got a puppy, started playing in sports, life in general. I would set deadlines out for myself, but I would always find reasons to let myself go by them or something would happen where my priorities would shift. This made the development of the app take longer and made me feel like I wasn’t getting anything done.

#### How can I improve?
I need to set small goals. Setting small goals, in conjunction with being realistic with my expectations, help me work towards a larger end goal. This means I would need to break down the large goal into smaller ones and be realistic when I will complete it. Additionally, I need to stick to my goals and not just put them backburner when I don’t feel like working.
I also need to set some time out for myself. I can’t always be working on projects, otherwise I am going to get burned out. Having some time to play video games or go on a hike will allow me to develop a better app and have fun doing it.


### Design
At first I didn’t do a bad job of this. The MVP architecture helped to decouple into Views and Presenter, Habits code was reused using inheritance, other task were put into their appropriate packages.

However, in the last few weeks of development, I just hacked away to get the thing done. Lots of Spagetti code and a lot of copy-pasta. This might be the rush to finish the app but I feel like the app could be redesigned better in general using design patterns that I just recently started learning

#### How can I improve:
Start with a general architecture. Understand the different components that are going to make up the app. In this case, I should have drawn out how the app going to notify users see how those

Once the general architecture is built out, start building the small architecture one by one. It is ok if the architecture didn’t work out, just go back to the drawing board.

### Better Tracking of Issues
I am really bad at remembering stuff which means I needed a way to list all the issues that I encounted while in development and also list all the items that needed to be built. 

There are tons of issue trackers out there and in my efforts to be highly productive, I tried using them all to find one that worked for me. That meant that I was using several different issue trackers for a single project. This was obviously an issue

#### How can I improve:

Towards the final days of the project, I decided to use purely Gitlabs issue tracker. It allowed me to easily add screen shots of issues and add labels. I can then sort by the labels to find ones that I needed to work on

### Testing

I only tested by using the app and didn’t even try testing via unit tests. I believe I tried to get something running using Gitlab’s CI but couldn’t get anything up and running.

This led to a few bugs down the road that could have been noticed quicker if I had created some unit tests. 
#### How can I improve:

Write the damn unit tests. In my defense, I was trying to build something for fun rather than building something that was going to be used by everyone.

### Being Realistic with my Expectations

This whole project was rather a large one. I wanted to notify users about habits, I wanted to have a backend component, I wanted to build out some game component to the app, and do all of that in less than 6 months with a little prior knowledge of android and working full-time with other life commitments. Looking back it was a monumental task but I thought I could take it.

#### How can I improve?

I need to be more realistic with my expectations. For only have built one app before, I was going to be trudging through a lot of unfamiliar territory. I was going to read a lot of documentation and I was going to make a lot of mistakes and have to figure out how to deal with errors. On projects, I am going to need to take more stuff like that into account when coming up with deadlines and timelines.

## Conclusion
There is still a lot that can be done on the project. It is definitely not ready to go on the app store. There are a lot of improvements that can be done and bugs that need to be fixed. But the project did what it was meant to do for the most part. I learned a lot Android and I also learned about personal development.
