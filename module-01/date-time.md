# Date and Time

Sometimes we need to access the computer's time and date settings to make an application work. For example if we wanted to display the current time on our web page, calculate how many days the user last logged in, when they last made a purchase, booked a train ticked etc.

## JavaScript `Date`

`Date` is a native JavaScript constructor function. We can create a date instance as we would with any other constructor:

```javascript
const currentDate = new Date();
```

We can now query this date instance to get the date and time:

```javascript
const hours = currentDate.getHours();
const minutes = currentDate.getMinutes();
const seconds = currentDate.getSeconds();
```

This will return the current hour, minute and second, of the moment **when the date instance was created**, and not the time when the methods are called.

We can also get the current day, month and year as well:

```javascript
const day = currentDate.getDay();
const month = currentDate.getMonth();
const year = currentDate.getFullYear();
```

### The problem with month

Unfortunately, for reasons only known to those who made it, `getMonth` returns a number from `0 - 11`, rather than `1 - 12`, even though the `getDay` function returns a number from `1 - 31`.

There's not much we can do about this unfortunately, we just need to be aware of it.

## `setTimeout` and `setInterval`

Another thing that is useful it to do is to be able to run a function or block of code at a certain time in the future, or every `n` seconds.

JavaScript's way of doing this is with `setTimeout` and `setInterval`. Let's see how we can use them:

```javascript
setTimeout(() => {
  console.log('setTimeout fired!');
  console.log(new Date());
}, 1000);
```

So in this case, callback function will be run **once** in 1000 milliseconds, or 1 second in the future.

```javascript
setInterval(() => {
  console.log('setInterval fired!');
  console.log(new Date());
}, 1000);
```

The syntax for `setInterval` is the same, but in this case it would run the code inside the callback function **every** 1000 milliseconds, forever.


In order to stop a `setInterval` you need to store the id returned from the function, then use it to clear the timeout with `clearTimeout` or `clearInterval`:

```javascript
const timerId = setInterval(() => {
  console.log('setInterval fired!');
}, 1000);

setTimeout(() => {
  clearInterval(timerId);
}, 5000);
```

The console log above would run every second, for 5 seconds, at which point the timer will be cleared from memory and stop running.

## Further reading

* [Date - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
* [Understanding Date & Time in JavaScript](https://www.digitalocean.com/community/tutorials/understanding-date-and-time-in-javascript)
* [JavaScript Date Cheatsheet](https://devhints.io/js-date)
