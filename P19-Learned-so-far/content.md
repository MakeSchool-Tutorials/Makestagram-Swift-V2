---
title: "What You Have Learned So Far"
slug: learned-so-far
---

You have come a very long way since starting **Makestagram**!

At this point we have covered many of the most important fundamentals of iOS development. This chapter is designed to be a decision point for you:

1. You can stop the tutorial here and start working on your own app
2. You can keep going with Makestagram, diving into more design details and fine tuning
3. You can pause here, start your own app, then come back later.

In part two of Makestagram we will cover:

1. Searching for and following friends
1. Implementing a sign up and log in screen
1. Fixing memory leaks
1. How to handle errors
1. Displaying more information about each post
1. How to add an open source library to the project

#What Have You Learned so Far?

Let's take a look at what you have learned so far.

##How to Create a Parse App

At the beginning of this tutorial you learned how to set up a new Parse App.

##How to Design a Data Model

After setting up the basic app we discussed the data model of _Makestagram_. We also covered the process so you can identify your data model based on the different requirements for your app.

##Creating the Data Model in Parse

You learned how to take the data model you designed and create the respective classes, properties, and relationships in Parse's dashboard. Remember what a _Pointer_ is?

##Connecting to Parse

In this step you connected your iOS app to your Parse server backend. You also set up a simple login mechanism.

##Setting up the App Structure

You learned how to create an app that uses a `UITabBarController`. You built the screen flow for **Makestagram** in Interface Builder using a storyboard.

##Implementing Photo Taking

You implemented a fairly complex feature that involved communication between different classes. You got to use callbacks and delegates - two popular paradigms in iOS development. This chapter should help you as a reference to understand more complex information flow in your app. You also learned how important it is to try to avoid placing all of your code in view controllers.

##Implementing Photo Upload

In this chapter you wrote your first data to the Parse backend! You also learned how to verify that your code works by using the Parse dashboard and data browser.

##Improving the Upload Code

A very important chapter of this tutorial! You learned how and why to create custom classes for Parse objects, by creating the custom `Post` class. You learned what it means to perform something in the _background_ and why that is necessary to create responsive apps.

##A Word on Security

You learned what ACLs are, why they are important, and how to set up a good default configuration.

##Fetching Data from Parse and Building the Timeline

You spent some time in Interface Builder, setting up the basic table view for the **Makestagram** timeline. Then you learned about `PFQuery`. You built a fairly complex query that fetches all the relevant posts for a user's timeline.

##Creating a Custom Post Table View Cell

You learned how to create a custom table view cell. You also learned how to download `PFFile`s. By the end of this step you were displaying photos on a user's timeline.

##Restructuring the Query Code

You learned how you should structure your Parse query code in larger apps.

##Improving the Image Download with Bindings

An extremely important chapter! You learned about _lazy loading_ and _asynchrony_ - two core concepts for most of the apps that you will build. You also learned how to use the `Observable` class together with _bindings_ to deal with information that is available asynchronously. You used that knowledge to automatically update the images displayed on the user's timeline, as soon as the image download completed.

Understanding the concepts in this step is essential!

##Decorating the Post Table View Cell

In this step you spent some more time with Auto Layout and further customized the `PostTableViewCell`.

##Implementing the Like Feature

Another extremely important step - you implemented the _like_ feature, one of the core features of the app. In this chapter you revisited many familiar concepts, like building Parse queries and using bindings. You also learned about the useful Swift methods `filter` and `map`.

##Keeping the UI up to Date

This step essentially taught you how to build interactive features that trigger changes in your app and on the Parse server. Another essential step that you should bookmark and come back to when you start building your own app. You also learned about how object _equality_ is defined and can be redefined in Swift.

##Pull-To-Refresh and Endless Scrolling

You learned how to be more thoughtful of your user's resources by querying a relevant subset of information to show in your app. You learned how to use the `TimelineComponent` to add pull-to-refresh and endless scrolling to your timeline.

## Best Practices

Throughout the tutorial you learned how to employ _best practices_ for many things, especially be separating code out into many classes to make it more modular.

#Where to go from here?

You have two options. You have learned enough to get started on your own app and come back if you are interested in any of the tweaks and features that we will be implementing in the next steps.

Or you can continue with this tutorial and learn what goes into finishing the entire _Makestagram_ app.

**It's up to you and your schedule!**
