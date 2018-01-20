# Chrome Dev Tools

## What are Dev Tools?

Most modern browsers include a set of tools that allow developers to monitor and explore what's going on in a web page. The Chrome Developer Tools, which we often call the "Dev Tools", are a set of debugging tools built into Google Chrome.

We can do a lot of useful things with these tools, but some of the things that are most useful:

- We can view the HTML & CSS as the browser has understood them
- We can watch requests and responses as they are made and received
- We can observe JavaScript being run
- We can debug issues with our code
- We can issue JavaScript commands on a console, or browser command line

Having such a powerful set of browser tools at our disposal is incredibly valuable, and you should get into the habit of using them to their full potential.

## Using the Dev Tools

### Shortcuts

- <kbd>CMD</kbd> + <kbd>ALT</kbd> + <kbd>I</kbd>: open the DevTools (will open on the last tab you had open)
- <kbd>CMD</kbd> + <kbd>ALT</kbd> + <kbd>J</kbd>: open the DevTools on the console tab
- <kbd>CMD</kbd> + <kbd>SHIFT</kbd> + <kbd>C</kbd>: to open the DevTools with the inspector

If you forget these commands, you can always go to _View > Developer > Developer Tools_, but try to memorise the keyboard shortcuts.

### Dev Tools tabs

Overall, there are eight main tools available in the Developer Tools. You may see people with a few more as you can add custom ones using extensions.


- [**Elements**](https://developer.chrome.com/devtools/docs/dom-and-styles): Editing Styles And The DOM
- [**Resources**](https://developer.chrome.com/devtools/docs/resource-panel): Managing application storage
- [**Network**](https://developer.chrome.com/devtools/docs/network): Evaluating network performance
- **Sources**: A graphical interface to the V8 debugger
- [**Timeline**](https://developer.chrome.com/devtools/docs/timeline): Performance profiling with the Timeline
- [**Profiles**](https://developer.chrome.com/devtools/docs/profiles): Profile the execution time and memory usage of a web app or page.
- **Audits**: The Audit panel can analyze a page as it loads.
- [**Console**](https://developer.chrome.com/devtools/docs/console): The JavaScript Console

We won't use all of these tabs during the course, the key ones we are going to get very familiar with are:

- **Elements**
- **Network**
- **Console**

### Elements tab

You can use the Elements panel for a variety of tasks

#### Modifying CSS

For modifying and editing your CSS, the dev tools has made it easy to quickly test and edit CSS before incorporating back into your application.

After selecting an element in the elements tab, you will be able to see its styles in the styles panel:

![](https://user-images.githubusercontent.com/3531085/35187616-7b2c0200-fe1e-11e7-810f-8bf9d55faa0e.png)

You can now modify the styles live, including:

- Copying and pasting styles.
- Using <kbd>UP</kbd>/<kbd>DOWN</kbd> to increment/decrement values by one.
- Using <kbd>ALT</kbd> + <kbd>UP</kbd>/<kbd>DOWN</kbd> to increment/decrement by 0.1.
- Using <kbd>SHIFT</kbd> + <kbd>UP</kbd>/<kbd>DOWN</kbd> to increment/decrement by 10.
- Clicking on the colored sqaure next to a colour property brings up a colour-picker.
- Holding <kbd>SHIFT</kbd> while clicking on a coloured square cycles between HEX, RGB and HSL.

Aaside from seeing the CSS properties for any inspected element, we can also see a visual representation of the box model along with the computed values. Since CSS loads styles sequentially, it's possible for a style in one stylesheet to be overwritten by another stylesheet that was loaded after it.

The *Computed* tab lets us see the styles for any page or element **exactly as the browser has interpreted all of the CSS styles collectively**.

#### Modifying DOM elements

Inside the DOM tree view, we can see a representation of the Document Object Model as interpreted by the browser.

Elements can also be edited live:

- Copying and pasting elements with <kbd>CMD</kbd> + <kbd>C</kbd> & <kbd>CMD</kbd> + <kbd>V</kbd>
- Editing the raw HTML content by *right-clicking* and choosing "Edit as HTML"
- Editing attributes by _right-clicking_ and choosing "Edit Attribute"

### Network tab

The Network panel records information about each network operation in your application, including detailed timing data, HTTP request and response headers, cookies, WebSocket data, and more.

#### Filtering

By default, the network tab shows all requests being made. However, you can filter these requests by:

- All
- XHR
- Script
- Style
- Images
- Media
- Fonts
- Documents
- WebSockets
- Other

You can also search through these requests, which can be useful.

#### Sourcing images

If you hover over an image in the network tab, you can right click and `Copy as cURL`
If you paste your clipboard in the terminal, it will paste the curl command to download the file.

### Console tab

The JavaScript Console provides two primary functions for developers testing web pages and applications:

- Log diagnostic information in the development process using the [Console API](https://developer.chrome.com/devtools/docs/console-api)
- A shell prompt which can be used to interact with the document and DevTools.

When we write JavaScript that we intend to be processed in a browser, we can use commands like `console.log()` to log values from our Javascript straight to this tab, as the code executes. This is immensely helpful when we're trying to figure out if certain values are being retrieved or passed between functions. We'll use this feature a lot in the coming weeks.

#### Console shell

The console shell also allows us to execute Javascript and interact with the current DOM using Javascript, just like we would from a JavaScript file that we load with the page.

## Further reading

- [Chrome Dev Tools - 25 Tips & Tricks](https://www.keycdn.com/blog/chrome-devtools/)
- [Chrome Dev Tools Overview](https://developer.chrome.com/devtools)
- [Dev Tool Shortcuts](https://developers.google.com/web/tools/chrome-devtools/shortcuts)
