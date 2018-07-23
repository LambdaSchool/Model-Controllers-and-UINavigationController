## Objectives

- Use UINavigationController to display data hierarchically 
- Implement a segue between a table view cell and a detail view controller
- Use the UINavigationItem API to customize navigation bar
- understand and explain the purpose of the MVC design pattern
- understand and explain the purpose of the model layer in MVC
- understand and explain the purpose of the controller layer in MVC
- understand and explain the purpose of the view layer in MVC
- implement a model controller

## Objective 1 - Use UINavigationController to display data hierarchically

### Lecture

One of the most common forms of navigation in an iOS app is called "Master-Detail". This means there is some sort of master list of information that is displayed to the user, which is usually achieved by using a table view or a collection view, and to see details about something in that list, you must navigate to a more detailed view.

Even if you've never heard of the master-detail pattern, you have surely used it before. For example, the iOS Contacts app shows a list of the contacts on your phone. If you want to see more information about the contact than their name however, you tap on the cell in the master list and it will bring you into a more detailed view that shows more information about that specific contact. 

The way we can implement this navigation between these two views is by using a few things. The first is a navigation controller. Navigation controllers (`UINavigationController`) create and manage what is called a "navigation stack". When you navigate from a master view to a detail view, the detail view is placed on top of the master view in a stack. More views could be placed on top of the detail view to create a larger stack. 

Using navigation controllers allows for us to navigate (hence the name) between our view controllers much easier. They will automatically create the back button you see at the top-left of the screen when you go into a detail view that easily lets the user go back to the previous view.

To use a navigation controller, you have to "embed" a view controller in the stack view. So if we embed the master list of contacts in a navigation controller, it becomes the "root view controller", which simply means it's the first view controller in the navigation stack. If you try this out in a storyboard, you will notice it will add a gray bar at the top of the view controller. This is called a navigation bar (`UINavigationBar`). Navigation bars give the user a "physical" representation of the navigation stack that they can interact with, while the navigation controller manages the navigation stack in the background based on how the user interacts with the navigation bar.

To add a navigation controller in a storyboard, there are two methods:

- The first is to select the "Navigation Controller" from the object library and drag it onto the canvas. It will expand into two view controller scenes. The first is a navigation controller, and the second will be a table view controllers. It will look like this:

![](https://user-images.githubusercontent.com/16965587/41450908-84445f7e-7027-11e8-9610-d5cc79e00a1b.png)

- Sometimes you will want a navigation controller but won't want the table view controller that comes along with it and instead want to use a normal view controller, or collection view controller, etc. What you can do in that case is select a view controller scene by selecting the yellow circle on a scene, then selecting Editor -> Embed In -> Navigation Controller in the menu like so:

![](https://user-images.githubusercontent.com/16965587/41451018-1765caf4-7028-11e8-9153-3eb09d13c78a.png)

## Objective 2 - Implement a segue between a table view cell and a detail view controller

### Lecture

The second thing needed in order to create use the master-detail, or any navigation pattern for that matter is called a "segue" (pronounced "seg-way"). Segues create a link between two view controllers that let you navigate from one to the other. The view controller the segue starts from is the "source" view controller, and the view controller the segue goes to is the "destination" view controller.

Segues are like a one way plane ticket to somewhere. You can fly to the destination, but you can't come back. For this reason, navigation controllers are especially helpful because they will automatically give the user a way to go back to the view they came from without us having to write any extra code. 

Segues are represented by the arrows in-between view controllers. The way that you create a segue from one view controller to another is by holding the control key, clicking from a UI element that users can interact with such as a button or a table view cell and dragging to the view controller you wish to segue to. It will pull up a contextual menu that will ask what kind of segue you want to create:

![](https://user-images.githubusercontent.com/16965587/41451314-8942006a-7029-11e8-998f-ae7daf0da258.gif)

From that contextual menu, you will have noticed that quite a few options came up. The first section of options are different kinds of segues we can use. The two that you need to worry about right now are:

- "Show" style segues will add the view controller to the navigation stack. These are easily the most common segue style.
- "Present Modally" will present the new view controller outside of the navigation stack completely, and it will always present it coming from the bottom of the screen and sliding upwards.

Show segues will make the destination view controller have a push (slide in from the right to the left) animation **unless** the source view controller is not a part of a navigation stack, in which case they will be presented in the same way as a Present Modally style segue.

## Objective 3 - Use the UINavigationItem API to customize navigation bar

### Lecture

If we want to add UI elements to a navigation bar, we can't actually add them directly. Instead we use navigation items. 

Navigation items (`UINavigationItem`) allow us to customize what is displayed on a navigation bar. For example, we can give it a title that describes what the view controller is, like "Contact List" or "Edit Contact". We can also add buttons to the navigation item, which will then place them on the navigation bar for us.

It should be noted that you can set up navigation controllers, segues, and navigation items both in Interface Builder or programmatically.

## Objective 4 - Understand and explain the purpose of the MVC design pattern

### Lecture

In iOS development, we use archetectural design patterns in order to structure our code in a way that promotes easy scalability and reusability. The most commonly used design pattern in iOS development is called MVC. _MVC_ stands for "Model-View-Controller". Each part of MVC has a specific job that helps keep our code clean while being scalable and reusable:

1. _Models_ represent a set of data in the application. Let's say we want to create an app that has a user profile. We might need to save a username, email, and age for each user. Managing these four properties separately would be a pain, and grouping the properties together in something like a dictionary isn't much better. Instead, we can make a class or struct with all the properties that represent a user profile. **Models don't do anything else besides represent the data in our app.**
2. _Views_ represent what the user will see on the screen, such as buttons, table views, etc. **The only job of the view is to show the information provided to it, and to receive user input.**
3. _Controllers_ manage the flow of our data in an application. It's the job of controllers to pass information between models and views. Controllers are separated into two specific sub-controllers:
  - View Controllers: You should already be familiar with view controllers. Take table view controllers as an example: a table view doesn't know what information to be displayed in its cells, but in the table view controller subclass, we give it that information by implementing the `numberOfRowsInSection` and `cellForRowAt` functions. This is an example of passing data from a view controller to a view.
  - Model Controllers: The job of a model controller is to handle any interaction with your model objects.
  
This is meant to be a very general overview of MVC, and we will go into more depth with each part of it during this lesson.

## Objective 5 - Understand and explain the purpose of the model layer in MVC

### Lecture

Let's take a closer look at the model part of MVC. As stated before, we use model objects to represent real-life data in our application. This makes it easy for us to keep related information together.

We'll take the earlier example of a user profile and see how we would create a model object for it in code.

``` Swift
struct UserProfile {
    
    let username: String
    let email: String
    var age: Int
}
```

Note: The `UserProfile` object could be a class as well, we would just have to add an initializer since classes don't synthesize one for you behind the scenes.

That's it for a simple model object. Generally, there is little else you will need to add to one. It really is simply a representation of data. Models are "dumb" in the sense that they can't do much themselves, which is where model controllers come in.

## Objective 6 - Understand and explain the purpose of the controller layer in MVC

### Lecture

Earlier we learned that controllers manage the flow of data, and are separated into two different kinds of controllers. You should already be familiar with view controllers and their responsibilities. We're going to focus on model controllers right now.

A Model Controller's job is to handle interactions between your model objects and the rest of your application. Generally you will have one model controller for every model object in your application. E.g. The `UserProfile` object above would have a corresponding model controller class. We would call it `UserProfileController`.

We use an acronym called "CRUD" to describe the most basic and most common interactions with a model object that a m
odel controller handles. CRUD stands for "Create, Read, Update, Delete". In just about any application, you will want to have functions that will perform a few if not all parts of CRUD. This doesn't mean that model controllers are limited to those four things. Really if you want to interact with models in any way, it should go through the model controller.

It should be noted as well that there are times when you will want multiple CRUD functions like two Update functions, etc. Don't worry about that for now, but just know that you aren't limited to making just one function for each part of CRUD.

If a user fills out their information on a profile creation screen then taps a save button, they assume that the application will take that data and save it. The job of the model controller is to receive that information that the view gives it and create a new instance of the `UserProfile` object. This is an example of a "Create" function in CRUD. Let's see what this function would look like in code:

``` Swift
class UserProfileController {
    
    // Create
    
    func createUserProfileWith(username: String, email: String, age: Int) {
        
        let profile = UserProfile(username: username, email: email, age: age)
    }
}
```

Now even though we create a new instance of `UserProfile` this isn't very useful, and if you were to paste that code in a Playground, it would actually give you a warning saying that the profile constant isn't used at all. This brings us to the next part of CRUD called "Read". 

Read means that we need something that the rest of the application will be able to "read" or access the `UserProfile` objects that we have in our app. Generally, this is simply an array that will hold a bunch of `UserProfile` objects. So let's add that to the `UserProfileController`

``` Swift
class UserProfileController {
    
    // Create
    
    func createUserProfileWith(username: String, email: String, age: Int) {
        
        let profile = UserProfile(username: username, email: email, age: age)
    }
    
    // Read
    
    var userProfiles: [UserProfile] = []
}
```

So now, we have a place that can store our `UserProfile` objects, and we can access them whenever we need to. Let's change the create function so that it will add the `UserProfile` it creates to that array:

``` Swift
class UserProfileController {
    
    // Create
    
    func createUserProfileWith(username: String, email: String, age: Int) {
        
        let profile = UserProfile(username: username, email: email, age: age)
        
        userProfiles.append(profile)
    }
    
    // Read
    
    var userProfiles: [UserProfile] = []
}
```

Now our Create function will actually be useful to us. This way when the user wants to save their information, the `createUserProfileWith` function will bundle up the individual properties (username, email, age) into an instance of `UserProfile`, then place it in the `userProfiles` array so that we can access it later.

"Update" in CRUD is very similar. We would just give the model controller an instance of a `UserProfile` we want to update, and the information we want to update like a new age when a user has a birthday.

If we wanted to make a "delete" function, we would simply find the profile in the `userProfiles` function and remove it. 

This is a lot to take in, so don't worry if you don't completely understand it. The good thing about learning MVC is that we will use it in every project so you will have plenty of practice to let it soak in.


## Objective 7 - Understand and explain the purpose of the view layer in MVC

### Lecture 

Views are the UI elements that the user sees on the screen. The job of views are simply to show information to the user, and relay back to us when the user interacts with the UI in any way. For example, when the user fills out their information on the screen then taps the save button, that button will tell us that it was tapped, allowing us to do something at that time. If we are using storyboards, the way the button will alert us is through an `@IBAction` we create. 

We can also give views information to display, as you've seen before. Table views are an example of this. We could have a profile view controller where the user can go to see their `UserProfile` information. The view controller would have `@IBOutlets` that give us access to the different views in the view controller. This allows us to take the `UserProfile` information and pass it to the views. 

