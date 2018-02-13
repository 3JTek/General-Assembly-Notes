# MVC

MVC stands for **Model-View-Controller** and is a _design pattern_ used to keep our code tidy and easy to work with. The main idea is a _separation of concerns_. When working with a complex web application it is very easy for parts of the codebase to become interconnected, which makes things more difficult to use. Imagine a box of cables: very quickly they become intertwined and then we have to spend time and effort pulling them apart again.

By separating our code into smaller files that do specific jobs we can keep everything organised and manageable. Take the human body as an example: the lungs do not know how to pump blood, nor do they care, their job is to expel carbon dioxide and take in air. Similarly the heart has no idea how to expel carbon dioxide, it only knows how to pump blood. This is useful because each part of the system knows exactly what to do and does not get confused with another.

With a web application, we can split our codebase into three basic parts:

| **Section** | **Purpose** |
|-------------|-------------|
| Model | Verify data and interface with the database |
| View | Display the data to the user (often in the form of HTML) |
| Controller | Get the data from the model and assign it to the correct view or template |

![](https://user-images.githubusercontent.com/3531085/36153331-d07920ee-10c5-11e8-8418-8d490540dfdc.png)

Each collection in our database has its own MVC set, known as a _resource_. Sitting on top of all of our resources is our router, which sends the relevant request to the relevant controller.

Let's imagine we have a `books` resource. A user navigates to `/books` on our website. The following process happens:

1. The browser makes a request to `/book`
1. The router receives the request and sends it on to the `books` controller
1. The controller asks the `books` model to get the book data from the database's `books` collection
1. The model sends the data back to the controller
1. The controller passes the data to the correct template
1. The rendered template is sent back to the browser

>**Note**: In a web API the View is considered the JSON payload. We often do not have a separate file for this, but instead simply render the JSON in the controller.

Sticking to this design principal means that our code is easier to read, easier to understand, and easier to maintain

- [MVC Explained Through Ordering Drinks At The Bar](https://medium.freecodecamp.org/model-view-controller-mvc-explained-through-ordering-drinks-at-the-bar-efcba6255053)
- [Model-View-Controller](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)
