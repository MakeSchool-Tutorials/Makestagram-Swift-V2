---
title: "Creating a Custom Post Table View Cell"
slug: custom-table-view-cell-post
---

As promised, this step will focus on displaying the photos associated with each post. To display the posts we will need to switch from the default table view cell that we are using right now to a custom one. Whenever you want to create a table view cell that doesn't use one of the few default layouts that iOS has to offer, you will end up creating a custom one.

#Setting up a Custom Table View Cell

First, we will change the height of the cells.

##Changing the Row Height

We want the images to be displayed as squares, just like the old-school version of Instagram. Since the screen has a width of 320 points we should set the height of each cell to 320 as well. Later you will learn that it isn't a good idea to rely on specific constant numbers in your code or storyboard files - but for now the solution is just fine.

> [action]
Change the row height to _320_ as shown below:
![Changing table view cell row height](row_height.png)

You will also need to change the row height of the table view.

> [action]
Change the Table View row height to _320_ as well:
![Changing the table view row height](tv_row_height.png)

##Adding an Image View

Next, we'll add an image view that will display the uploaded photo.

> [action]
Add an image view to the table view cell by dragging it to the PostCell's Content View, as shown below:
![Dragging Image View to PostCell](add_image_view.png)
Change its size to be the full size of the cell by dragging the corners of the image view. We won't be using constraints until later on.

Additionally we need to change the _Content Mode_ of the image view. Currently it is set to the default value which is _Scale To Fill_. That will distort the image to fit into the size of the image view. Distorted images look ugly! It's much better to crop them. To do that we change the _Content Mode_ to _Aspect Fill_.

> [action]
> Change the _Content Mode_ of the image view in the _Attributes inspector_, as shown in the image below. Also select
> _Clip Subviews_ in the right panel:
> ![image view content mode](imageview_content_mode.png)

The _Clip Subviews_ option ensures that the image is not drawn outside the border of the image view. Without this option the image can leak over the edges of the image view.

##Creating a Custom Class for the Table View Cell
Since we will want to create an IBOutlet connection from this image view to our table view cell (to set the image), we will need to create a custom `UITableViewCell` subclass. That IBOutlet connection will allow the `TimelineViewController` to set the image as soon as a post is downloaded.

> [action]
Create a class called `PostTableViewCell` in the Xcode group shown below (don't forget to first create a folder on the filesystem and then import into your Xcode project!). The new class should be a subclass of `UITableViewCell`:
![image](post_table_view_cell.png)
Confirm that your newly created class is a subclass of `UITableViewCell`:
>
    import UIKit
>
    class PostTableViewCell: UITableViewCell {
>
        // ...
>
    }

##Connecting the Custom Class to the Table View Cell

Now that we have created the `UITableViewCell` subclass, we need to configure it to be used in our table view cell in our storyboard.

> [action]
Set the _Custom Class_ of the table view cell to our newly created `PostTableViewCell`:
![Setting the PostCell custom class to PostTableViewCell](cell_custom_class.png)

##Creating a Referencing Outlet for the Image View

To complete the last step we need to create a referencing outlet from the image view to the table view cell. This will allow us set the image displayed inside of each cell.

> [action]
Create a referencing outlet from the image view to the `PostTableViewCell` class; name the property `postImageView`:
![Dragging New Referencing Outlet from Image View to PostTableViewCell](image_ref.png)

#Adding Code to Display Images

Now we have a table view cell that will allow us to display photos that users have taken. We'll need to update our code to show these photos instead of the placeholder text that we're currently displaying. That means we need to update the code that creates our cells.

> [action]
> Update the `tableView(_:cellForRowAtIndexPath:)` method in `TimelineViewController` as following:
>
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        // 1
        let cell = tableView.dequeueReusableCellWithIdentifier("PostCell") as! PostTableViewCell
>
        // 2
        cell.postImageView.image = posts[indexPath.row].image
>
        return cell
    }

1. In this line we cast `cell` to our custom class `PostTableViewCell`. (In order to access the specific properties of our custom table view cell, we need to perform a cast to the type of our custom class. Without this cast the `cell` variable would have a type of a plain old `UITableViewCell` instead of our `PostTableViewCell`.)
2. Using the `postImageView` property of our custom cell we can now decide which image should be displayed in the cell.

Now you can run the app and you will see...

![Empty table view cells](empty_cells.png)

... empty Table View Cells! **Why is this happening?**

##Downloading PFFiles

Just like _PFPointers_, _PFFiles_ are not automatically downloaded together with their parent objects. Each of our posts references a _PFFile_, that reference is stored within the post. However, we are responsible for performing the download of the actual image.

This system makes a lot of sense - in many cases we want some information about a post (like the author, or when it was created) but we don't need access to the uploaded photo. It would be a waste of resources to download the photo in that case.

This means we need to write some extra code to download the image.

As promised, this chapter will focus on making some visual progress, so for now we will use a temporary solution to download the images.

> [action]
Extend the callback block of `findObjectsInBackgroundWithBlock` within the `viewDidAppear` method of `TimelineViewController` as shown below:
>
    query.findObjectsInBackgroundWithBlock {(result: [PFObject]?, error: NSError?) -> Void in
        self.posts = result as? [Post] ?? []
>
        // 1
        for post in self.posts {
            do {
                // 2
                let data = try post.imageFile?.getData()
                // 3
                post.image = UIImage(data: data!, scale:1.0)
            } catch {
                print("could not get image")
            }
        }
>
        self.tableView.reloadData()
    }

1. We loop over all posts returned from the timeline query
2. First, we are calling `getData()` to download the actual image file. We are given an `NSData` object with the image. We are currently not doing this in the background, but on the main thread - that is bad! We'll fix that issue in the next page.
3. Once we have retrieved and stored the data, we turn it from `NSData` into a `UIImage` using the `UIImage` constructor that takes `NSData`. We store the image in the `image` property of the `post`

Now you can run the app again. And for the first time you should see our photos on the screen!

![timeline with photos](photo_download_working.png)

That's awesome! This app is very slowly starting to look like a real photo sharing app!

#A Note on Error Handling

When your app makes it into the App Store, and into the hands of your users, you lose a lot of the control over it. One of the best examples of this is how reliable the internet of your users may or may not be. Perhaps they are in a country that blocks certain servers or services. Apps that crash get bad reviews, and might not even make it past the review process employed by Apple to get into the store in the first place.

Luckily for you, with Swift, Xcode will not allow you to do many unsafe things without _force_. But _force_ is usually not a _best practice_. `do/catch` however, is a _best practice_.  What you do with the error handling will be up to you, and will dictate the user experience for everyone who encounters the unexpected in your app. But, by using this pattern, you will avoid a crash, which is of the utmost importance on mobile app platforms. To many clients/users/employers, crashes are unacceptable. There are tools to help catch and report crashes so you can snuff them out, but those should be a safety net, not the main thing you rely on.

#Conclusion

In this chapter you have learned how to set up a custom table view cell. Almost all iOS apps use table views in combination with custom cells, so this knowledge will be very useful for your own app! You have also learned that `PFFile`s are stored as references in other `PFObject`s and are not automatically downloaded. For now we have implemented a primitive approach of downloading the photos for each post - after all, the focus of this chapter was to make some visual progress!

In the next step we will focus on improving the code that queries Parse data. The goal will be to set up a nice app structure that will allow us to add many different queries as we progress through this tutorial, without turning our project into a mess. We will also make some improvements to our photo download code.

Let's turn this working solution into a good one!
