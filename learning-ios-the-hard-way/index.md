# Things I've learned trying to build my first iOS 8 App

Before anything, I must say:

1. I do know how to code in other languages in a professional (senior) level. Python, PHP and Java are my main languages at the moment

1. I've never coded any mobile app that is not web in my life before

1. I've always wanted a mobile expenses record app that works in an specific way. I've already built that app as a web-based app (It is not public because it was built for my own specific use)

I've wanted to learn to build mobile apps for a while now. I have an iPhone so the smart thing to do is to build an iOs app. The thing is, I've always hated Objective-C. I don't know why but it is a horrible horrible language for me to learn. So, every time I started a new app I quit in the middle because that is just something I could not use. It's not the static typing, it's not the pointers, It's just that... it is too damn different from everything else I'm used to.

Luckily for me, Apple released the [swift programming language](https://developer.apple.com/swift/) and now I can code iOS apps without wanting to pull my eyes out from their sockets with a spoon. It's more similar to languages of this (last) century, and the syntax is something I can get used to (because I already am). The thing is, I have no idea how.

So, I downloaded Xcode 6 Beta, installed it, opened it, created a new project, added a label, moved a table view, changed the title and after a while I realised...

![I Have no idea what I'm doing](http://www.angelfoodcomic.com/wp-content/uploads/I-have-no-idea-what-I-am-doing.jpg)

When I convinced myself that this was not something I could learn by pushing buttons, I started looking for tutorials.

I Found [this series of tutorials](http://www.raywenderlich.com/74438/swift-tutorial-a-quick-start) which were pretty awesome to start. The thing is, none of those tutorials explain how the heck **UIKit** is thought to be used. They just teach how to do a specific task, but I needed more low-level knowledge to start (I forget everything if I learn stuff by heart, I need to know how it works).

The take-away from those tutorial was (and this is coming after a day of doing several of these):

1. It looks like **UIKit** is a library that contains every iOS thing you may need to *Draw* on the phone. Meaning that every element you can draw or any class you should inherit from to use it is in there.

1. It looks like everything in **UIKit** is though with an **MVC** architecture in mind. Meaning you always end up with a certain **View** talking with a **Controller** that uses a **Model** you code. Every **View** in **UIKit** has a **ViewController** that you will have to extend and override to your needs.

After I *learned* all that in one day, I thought I could just start a new empty project in Xcode and build my expenses app.

![I've never been so wrong](http://media.tumblr.com/add585e61c39de42d4b692a23fc86208/tumblr_inline_mmkjegnCVQ1qz4rgp.gif)

## Chapter 1: Learning the hard way

I created a new empty project, created a storyboard (called it *Main* just in case without knowing if it was needed or not), added a ViewController from the object library, added a TableView (because I'm sure I'll use one). Finished my UI, pushed PLAY. And there it was. I've built a perfect white app for iOS 8.

I guessed it was because there was no (custom) code around it, so, almost copying and pasting the code from the other tutorials, I created a new class that extended from **UIViewController** and added two different `tableView` functions

	func tableView(tableView: UITableView!, numberOfRowsInSection section: Int) -> Int {
		return 10
	}

and

	func tableView(tableView: UITableView!, cellForRowAtIndexPath indexPath: NSIndexPath!) -> UITableViewCell! {
        let cell: UITableViewCell = UITableViewCell(style: UITableViewCellStyle.Default, reuseIdentifier: "bookCell")
        cell.text = "Text"
        return cell
    }

Because that is what you are supposed to do, right?

According to what I thought I did, I should have a TableView with 10 cells displaying "Text". But nothing happened.

1.5 hours later of reading, looking, comparing and rage-closing Xcode, I realised every *ViewController* that you drag & drop from the objects library has a **class** attribute in the *Indentity Inspector*. I changed that so it uses my *ViewController* class. Play. Nothing.

30 minutes later, I found that the project has an attribute called *Main Interface* in the *General* tab under *Deployment Info*. I chose my *Main* storyboard (I suppose it chooses from my *.storyboard* files because that is the only file called Main in my project). Play. Nothing.

![Y won't this shit work?!](http://wac.450f.edgecastcdn.net/80450F/my1073fm.com/files/2013/08/Frustration-630x419.jpg)

This is were I went bananas and started a new SingleView project to compare what the heck wasn't I doing right. I realised that my `AppDelegate` class had 3 lines different from the other one (after opening two Xcode projects side by side and comparing file by file)

```
	self.window = UIWindow(frame: UIScreen.mainScreen().bounds)
  self.window!.backgroundColor = UIColor.whiteColor()
  self.window!.makeKeyAndVisible()
```

I guess this was overriding my main interface configuration and displaying a white window as my app. Damn you lines!!!! I removed them. Play. Now I see a table view!

![Empty table cells](http://cl.ly/image/151A2S1V0f3X/Screen%20Shot%202014-06-27%20at%2011.07.43%20PM.png)

![Y U NO SHOW TEXT ROWS](http://cdn.memegenerator.net/instances/500x/51774477.jpg)

That is when I remembered that table views had something called **delegate** and **dataSource**, I had no idea how to set that. I remembered from the tutorials that I had to drag something and drop it to another something while holding control key (apparently you do that a lot in Xcode). So, I dragged the *Table View* to everything I found that it was droppable. After several intents, I figured out I had to Drag the *Table View* to the *View Controller* (Yellow thingie at the top)

![Drag and drop to the yellow thingie](http://cl.ly/image/1g19393V011D/Screen_Shot_2014-06-27_at_11_14_08_PM.png)

Doing that, you can set the *delegate* and *dataSource* of the *Table View* to be the same *ViewController* class I wrote before.

> The funny thing is that `tableView` functions are from **UiTableViewController** but as I wrote those functions in my class that extends from **UIViewController** it does not fail. I suppose *UiTableViewController* is just an "abstract" class extending from *UIViewController*.

Play and IT WORKS!

![My very first mobile app ever](http://cl.ly/image/1F2n231t1K0K/Screen%20Shot%202014-06-27%20at%2011.06.25%20PM.png)

Doing this the hard way I learned several things:

- Views in the storyboard should have the same name as the class that will have the code with the behaviour. In other words, every *View* in the storyboard is actually a *ViewController*

- It's completely impossible to learn how the heck things work here if you don't know what you are looking for.

- Xcode and storyboards do magic

So, that is how I learned how to build an app the does nothing.

Now, I need to learn how to add cells to a table based on a dynamic object (array) and how to do navigations.
