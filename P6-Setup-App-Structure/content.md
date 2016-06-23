---
title: "Setting up the App Structure"
slug: setup-app-structure-uitabbarcontroller
---

Once you have decided on your app's features, your next step should be to outline your app's structure. The outline should contain all the screens in your app, and should include how they will be connected. For **Makestagram**, the outline would look like this:

![app structure outline](app_structure.png)

You should come up with a similar diagram before writing code when it's time for you to make your own app - it will give you a better idea of how much effort it will take to build your app.

#Another Container View Controller: The UITabBarController

In this section we're going to set up the basic structure of our app, starting with a *container view controller*, a view controller that can present other view controllers.

In **Make School Notes** we used a `UINavigationController` as our container view controller, which allowed us to drill down on content by *pushing* and *popping* view controllers.

In **Makestagram**, we will be using a different container view controller called the `UITabBarController`. The `UITabBarController` contains a *tab bar* and *tab bar items*. Each tab bar item is connected to a specifc view controller, and tapping a tab bar item displays the appropriate view controller. Below is a screenshot from the finished **Makestagram** app that contains a tab bar (the tab bar is enclosed by a red rectangle) and three tab bar items (home, camera, friends):

![Finished Makestagram with tab bar highlighted in red](tab_bar_example.png)

By selecting the appropriate tab bar item, the user can switch between the Timeline Screen, Photo Capture Screen, and Friends Search Screen.

It is common to see multiple types of container view controllers in a single app - we will explore this idea when adding the image filter feature to **Makestagram** later on.

#Setting Up the UITabBarController

Since we won't be adding the login screen until later, the first screen we need to setup is our Tab Bar Screen.

The **Makestagram** project template that you downloaded earlier currently has one screen, a simple `UIViewController`. We don't need that screen, so let's go ahead and delete it.

> [action]
Open *Main.storyboard*. Select the existing view controller; after you have selected it you should see a blue border surrounding it:
![Blank view controller selected in Storyboard](selected_vc.png)
>
Hit the *delete* key to remove this view controller. Now you should see a blank storyboard:
![Empty Storyboard](blank_storyboard.png)

Let's also delete the screen's accompanying source file called *ViewController.swift*.

> [action]
Delete the *ViewController.swift* file:
![ViewController.swift right clicked](delete_vc.png)
![Select Move to Trash button](delete_vc_trash.png)

Now we can add the `UITabBarController`.

> [action]
Drag the `UITabBarController` from the component library into the storyboard:
![Drag Tab Bar Controller into Storyboard](add_tab_bar.png)

We have now set up the container view controller for our app - time to run it?

If you run the app now, after the splash image disappears, you will see a blank black screen. You will also see the following error in the console:

> Makestagram[76593:9371786] Failed to instantiate the default view controller for UIMainStoryboardFile 'Main' - perhaps the designated entry point is not set?

**Why Are We Seeing This Error?**

When working with storyboards, we have to designate an entry point, or more simply, we have to tell UIKit (the Apple framwork which handles manipulating the user interface) what view controller should appear first (we call this view controller the *Initial View Controller*). When UIKit doesn't know which view controller to show first (a.k.a. when the entry point is not set), it defaults to showing an empty black screen.

We are seeing this error because the `UIViewController` that we initially deleted from the project was set as the Initial View Controller, and after deleting it, we never set another view controller to be the new Initial View Controller. Let's fix this error by assigning our tab bar controller as the Initial View Controller.

> [action]
Configure the tab bar controller to be our app's Initial View Controller. Click on the Tab Bar View Controller. Next select the *Attributes Inspector* in the right bar (1). Then check the *is Initial View Controller* checkbox (2). As a confirmation you should see an arrow pointing to the Tab Bar View Controller (3):
![Tab Bar Controller is the initial view controller](initial_vc.png)

Great! When you run the app now, you should see a tab bar with two items:
![Blank app in simulator with two empty tab bar items](tab_bar_done.png)

#Adding a Third View Controller to the Tab Bar

The default tab bar comes with two view controllers; however, we need three view controllers for **Makestagram**. Lets add the third view controller now.

> [action]
Add a third view controller, as shown in the video below. Add a blank view controller, then *control* (⌃) click and drag from the Tab Bar Controller to the blank view controller, and select "Relationship segue: view controller".
![ms-video](https://s3.amazonaws.com/mgwu-misc/SA2015/AddViewController_TabBar_Small.mov)

We now have all of the required view controllers set up!

#Decorating the Tab Bar Items

Currently the tab bar has three entries and looks like this:

![Blank tab bar with 3 items](tab_bar.png)

The empty tab bar looks pretty bad, so let's add an image to each tab bar item to make things look nicer. (Images will also have the benefit of making it easier to identify the different tabs.)

> [action]
[Download the art pack for this tutorial!](https://s3.amazonaws.com/mgwu-misc/SA2015/Makestagram_Art.zip)

##Adding Assets to an App

Now it's time to add some of these assets you just downloaded to our app. All assets used within an iOS app are stored in *Asset catalogs*. Every iOS project that you create with Xcode comes with one default asset catalog called *Images.xcassets*:

![Images.xcassets highlighted](asset_catalog.png)

That asset catalog already contains one resource for the App's icon. You add new resources to your app by creating new entries (called *Image Sets*) in this asset catalog. You can also create multiple asset catalogs, which is useful for apps with huge amounts of images.

Let's briefly discuss some important concepts about asset handling on iOS. You have probably realized that we're providing three different image files for each asset we wanted to use in our App (*@1x*, *@2x* and *@3x*). These different images have different resolutions, each suited to a specific type of iOS devices with a different screen resolution. The *@1x* assets are used for the oldest iOS devices, e.g. iPhone 3Gs, which don't have retina displays. The *@2x* images are used for the 3.5 and 4 inch retina screens of the iPhone 4(S) and iPhone 5(S). Finally, the *@3x* images are used by the iPhone 6(s) and iPhone 6(s) Plus. In most cases you won't have to spend too much time thinking about this, as long as you provide assets in all relevant resolutions.

Applications that designers use, like [Sketch](https://www.sketchapp.com/) allow *exporting* all versions at the same time. And applications like _Xcode_ allow *importing* all versions at the same time.

Let's do that now!
> [action]
Unzip the downloaded art pack. Open *Images.xcassets* and click the small `+` icon at the bottom of the list that has only `AppIcon` in it. Select `Import` from the pop-up menu. Select the unzipped art folder and bam!
>
Note: In order to import all images at once like this, they must be named following the same conventions as in the art pack. For example: anImage.png, anImage@2x.png, anImage@3x.png.
![Filled asset catalog](all_assets.png)

You should also note that when working with asset catalogs, we don't reference images by their file name, but instead by the name of their Image Set. In our example, our image sets are called *camera*, *people* and *home*.

##Assigning Images to Tab Bar Items

Now that we have added images to our app, we can assign them to our tab bar items.

When we're done, the tab bar should look like this:

![Tab bar with home, camera and people icons](tab_bar_order.png)

Let's set the images up!

> [action]
**Repeat the following steps for all three view controllers**:
>
1. Select a view controller in the left panel of the storyboard
2. Select the view controller's tab bar item (`Item X` with a little star icon)
3. Open the *Attributes Inspector* in the right panel
4. Clear the *Title*
5. Set the Item *Image* to *home*, *camera* or *people*, depending on which view controller you are currently setting up
>
![All five tab bar item steps highlighted](setup_tab_bar_item.png)

Now we have images on each tab bar item - but something doesn't look quite right. The images aren't vertically centered. That's because iOS reserves some space below the image for the *Title* of the tab bar item. For **Makestagram** we don't need titles, instead we want the images to be centered. We can accomplish that by adjusting the *Image Inset* for each tab bar item.

> [action]
**Repeat the following steps for all three view controllers**:
>
1. With the tab bar item selected, open the *Size Inspector* on the right panel
2. Set the *Image Insets* as following:
      - *Top*: 5
      - *Bottom*: -5
>
Or, if you are feeling really fancy, you can select all three tab bar items in the hierarchical menu on the left by command-clicking them, and then you can change the setting for all three at the same time!
![Adjusting tab bar item insets](tab_bar_item_insets.png)

Now all of your tab bar items should have images that are nicely centered.

##Reordering Tab Bar Items

Just in case you didn't set up the view controllers in the correct order, I want to show you how you can reorder them. Select the tab bar view controller in your storyboard, then simply drag the tab bar items to rearrange them:

![ms-video](https://s3.amazonaws.com/mgwu-misc/SA2015/ReorderTabBarItems_small.mov)

Now we have a nice looking Tab Bar that connects to the three View Controllers of our app.

#Creating Classes for our View Controllers

To finish this section, let's create the source code files for all three view controllers that we will be working on throughout this tutorial. We'll add them to the  *ViewControllers* group to keep this project nicely structured.

> [action]
Create the `TimelineViewController` class as a subclass of `UIViewController` as shown in the video below:
![ms-video](https://s3.amazonaws.com/mgwu-misc/SA2015/Timeline_NewViewController_small.mov)
**Repeat these steps** to create a `PhotoViewController` and a `FriendSearchViewController`, which also should be subclasses of `UIViewController`.

When you're done you should have three View Controllers:
![Three newly created view controllers in the ViewControllers group](three_vcs.png)

Make sure that each View Controller starts with the line `import UIKit`. That indicates that your file has been created from the correct iOS template.
If you see the line `import Cocoa` instead, you mistakenly created an OS X file, and the project won't compile. You can simply replace `import Cocoa` with `import UIKit` to fix that issue.

##Configuring Custom Classes in Storyboard

The last setup step that remains is connecting the classes that we just created to our view controllers in our storyboard.

> [action]
**Repeat the following steps for all three View Controllers**:
>
1. Select the View Controller in Storyboard
2. Open the *Identity Inspector* in the right panel
3. Set the custom class to match the current View Controller
>
![Setting TimelineViewController custom class in Storyboard](vc_custom_classes.png)

#Wrapping up

Congratulations! We have now set up the basic structure of **Makestagram**! You should have three tab bar items with icons, connected to three view controllers in your storyboard, each backed by a corresponding Swift file.

We're ready to implement our first Feature: **Uploading photos!**
