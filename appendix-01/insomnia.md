# Intro to Insomnia

[Insomnia.REST](https://insomnia.rest/) is a program we can use to make HTTP requests. This is useful if we are testing external APIs or RESTful APIs that we have created. We can use it to make GET / POST / PUT (update) / DELETE requests. You can think of it a bit like a web-browser, but it just deals with data without any decoration.

> Useful APIs to play with using Insomnia:
 - [postcodes](https://postcodes.io/) `api.postcodes.io/postcodes/E17PT`
 - [People in space](http://open-notify.org/Open-Notify-API/People-In-Space/) `http://api.open-notify.org/astros.json`
 - [ISS location](http://open-notify.org/Open-Notify-API/ISS-Location-Now/) `http://api.open-notify.org/iss-now.json`

## Making a simple `GET` request
![get request](https://user-images.githubusercontent.com/40343797/50973100-00ca5280-14e0-11e9-96ee-f8f2defc4b7c.png)

To make a simple API request, add the API URL (end point) into the bar at the top of the page. You can can then select whether it is a GET, PUT, DELETE etc. The response is shown in the right-hand panel. It also shows the status which is returned with the response.

### Setting headers and the body of a request

When making API requests, you can include information in the body and the header of your requests. When using Insomnia, you add this information in the left-hand panels. We write this in JSON.

## Setting up a Workspace and folders

When you make a request, it is saved to Insomnia until it gets deleted or is overwritten. To keep track of your requests, you can make new WorkSpaces for projects you are working on and then refer back to them later. Within Workspaces, you can also create folders to organise similar requests. This can be very useful when testing complicated projects.

## Setting up environment variables

You can set variables within Workspaces and use these throughout insomnia. For example, you could save an auth token as a variable or the base URL. You can then just change the variable and your request will be updated.

## Extra features
* Repeating API requests (right click on Send button)
* Applying filters (bottom of response panel)

### More advanced features
* Chaining requests https://support.insomnia.rest/article/43-chaining-requests
* Adding documentation to a request and exporting workspaces

## Further reading
* [Insomnia.REST Documentation](https://support.insomnia.rest/)
* [Walk-through of testing an api](https://medium.com/@artiwarahdamrongchai/test-your-apis-with-insomnia-rest-client-355093f32755)
