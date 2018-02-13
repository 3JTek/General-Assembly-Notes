# Templating with EJS

We can make a website either as a collection of HTML files, or we can use a _templating engine_. This allows us to reduce the amount of files in our application by allowing us to reuse the same templates, and inject different data into them.

Take Airbnb for example, every property is displayed in exactly the same way, only the specific data of the properties changes. Rather than having a separate HTML file for each property, we have a generic property template file, and we inject the specific data.

In order to do this we have to use a template engine which has specific tags that indicate that the data needs to be injected at that position. Its very similar to having an email template and performing a mail merge on it. Rather that using specific data we use a placeholder instead, that is swapped out for the actual data (a person's name, for example).

On this course we use EJS as our templating engine of choice. It's simple to use and is similar to the original templating engine: PHP.

## EJS tags

| **Tag** | **Purpose** |
|---------|-------------|
| `<% %>` | Non printing - used for control flow like loops and if/else statements |
| `<%= %>` | Safe printing - prints data but does not render HTML tags |
| `<%- %>` | Unsafe printing - prints data but does render HTML tags |

## An example

Most commonly we'll use EJS to render an array of data. Let's take some trainers as an example:

```js
const trainers = [{
  name: 'Air Jordan',
  brand: 'Nike',
  price: 149.99
  color: 'black'
}, {
  name: 'Campus',
  brand: 'Adidas',
  price: 79.99,
  color: 'navy'
}]
```

We can inject the data into the template when we call `res.render`:

```js
app.get('/trainers', (req, res) => res.render({ trainers }));
```

Now inside the template file we can access now the array `trainers`:

```html
<% trainers.forEach(trainer => { %> <!-- <% %> for control flow -->
  <div class="trainer">
    <h4><%= trainer.name %></h4> <!-- <%= %> for printing variables -->
    <h5><%= trainer.brand %></h5>
    <p>£<%= trainer.price %></p>
    <p>Color: <%= trainer.color %></p>
  </div>
%>
```

Using a templating engine can DRY up and simplify our server-side code, and help to keep our design and layout consistent across a whole website.

## Further reading

- [EJS on Github](https://github.com/mde/ejs)
- [Creating a Dynamic View with EJS](http://perkframework.com/v1/guides/creating-a-dynamic-view-with-ejs.html)
