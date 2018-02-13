# REST

REST stands for **REpresentational State Transfer** which is a confusing acronym because it doesn't seem to describe what it actually means. Simply put REST is a way of defining our routes and HTTP request verbs to ensure that all the CRUD actions are available for a specific resource.

There are seven actions that a user may need to perform on a resource in order to view it, edit it, update it and delete it. We use a specific set of URLs for each action, coupled with a specific verb to maintain consistency of our application and to ensure that it behaves as expected.

Here is an outline of seven RESTful routes for a `foods` resource:

| **Route** | **Path** | **Verb** | **Response** |
|-----------|---------|----------|--------------|
| INDEX  | `/foods` | GET | Display all of the food items |
| NEW | `/foods/new` | GET | Display a form to create a new food item |
| CREATE | `/foods` | POST | Create the food item, then redirect to `/foods` |
| SHOW | `/foods/:id` | GET | Display a single food item |
| EDIT | `/foods/:id/edit` | GET | Display pre-populated form to edit a specific food item |
| UPDATE | `/foods/:id` | PUT | Update the food item, then redirect to `/foods/:id` |
| DELETE | `/foods/:id` | DELETE | Delete the food item, then redirect to `/foods` |

Any resource that we need to all CRUD actions on should be RESTful in this way.

Other routes, like `/about` or `/login` are not RESTful, since they do not require full CRUD behaviour. In that case there is no specific set of routes and verbs that we need to stick to, but it is worth attempting to stay as close as possible to the REST paradigm. This will make it easier for other developers to know what to expect when working with the code base.

REST is a design pattern, you don't _have_ to use it<sup>\*</sup>, but it is a great way to keep things organised.


<small><sup>\*</sup> On WDI you absolutely have to use it!</small>

## Further reading

- [What is REST?](https://www.codecademy.com/articles/what-is-rest)
- [Understanding REST](https://medium.com/@sagar.mane006/understanding-rest-representational-state-transfer-85256b9424aa)
