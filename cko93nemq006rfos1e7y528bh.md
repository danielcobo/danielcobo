## Check if the user is from EU with JavaScript

Obviously your best guess is to ask the user. That being said, we generally want to avoid interrupting the user. Websites and apps are (or at least should be) about what the user's needs not ours. 

That being said, given the many legislation requirements such as GDPR and cookie policy within the EU, it would be useful to know if we should display certain messaging. 

<h2 id="solution">Solution</h2>

%[https://codepen.io/DanielCobo/pen/OJWeEEW]

Let's break it down.

1. Wrap everything in an [IIFE][1] to avoid global namespace pollution:
```javascript
(async function () {
  //Our code goes here.. keep reading
})();
``` 

2. Write a function to get the data from an IP lookup API. The function takes an optional `ip` parameter. If no value is given, no worries, the API will use the current IP that is making the call. In this case this will be the IP of whoever is visiting the page. The function will `fetch` the data, `await` for the reply and parse it. Parsing is minimal, `fetch` has methods to treat responses as `JSON` or text. we simply transform a text response into a `boolean` value - `true` if from EU and `false` if not. Simple right?  
```javascript
  /**
   * Check if from EU based on IP
   * @param {string} [ip] - IP, default is current
   * @returns {string} - true/false if IP is from EU (https://ipapi.co/api/#complete-location)
   */
  const fromEU = async function fromEU(ip) {
    let url = 'https://ipapi.co/';
    if (ip) {
      url += ip + '/';
    }
    url += 'in_eu/';
    return fetch(url)
      .then(function (response) {
        return response.text();
      })
      .then(function (txt) {
        if (txt === 'True') {
          return true;
        } else {
          return false;
        }
      });
  };
``` 

3. Remember, the function you just wrote is `async`, so `await` for the parsed reply (`true`/`false`). This would also be a great time to **handle any errors**, for this simple demo, let's just `log` to `console`.
```javascript
  //Get data from IP lookup API
  const isFromEU = await fromEU().catch(function (err) {
    console.log(err);
  });
```

4. Do something with the data we just got. I admit, this is not particularly creative, but it'a a very direct way to show what this tutorial is about. Basically, if this IP is within the EU we want to checkmark and if not, then no checkmark. 
```javascript
 //Un/check if from the EU
  let checked = '';
  if (isFromEU) {
    checked = 'checked';
  }
  document.getElementsByTagName('body')[0].innerHTML =
    '<label><input type="checkbox" name="fromEU" ' +
    checked +
    '>I am from the EU</label>';
``` 

For the full code, check the [demo above](#solution).  

Questions? Comments? Let me know here or [@danielcobocom][2].

## API disclaimer

Whenever you are using an API, like in this case it's an IP lookup service, be sure to check the terms, pricing, etc. APIs are great for adding function and often save time and money, but at the same time they can be points of failure, so always take some time to make sure the API is a good fit for your project.

[1]: https://developer.mozilla.org/en-US/docs/Glossary/IIFE
[2]: https://twitter.com/danielcobocom