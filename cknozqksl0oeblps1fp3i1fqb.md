## Get location by IP with JavaScript

[Skip to solution](#solution).

## Problem

Knowing your customer location can help you tailor their experience. Other times you may want to check for this information to compile statistics or comply with regulations. 

### What's the best way to get location data? 

One way to do it would be using the built-in HTML geolocation API. Another would be to use an IP lookup API. The best solution depends on your goals and needs.

> ...most accurate location? Use the HTML geolocation API. 

Need the most accurate location? Use the  [HTML geolocation API][1]. This will prompt the user for permission to use GPS (or whatever the device thinks is most accurate). 

The prompt is great for privacy, but it can also be annoying to the user. This is especially true if they don't expect it. You should also keep in mind that modifying the design of the prompt is generally not an option. This makes sense, since security related UI for functions such as exact location should be uniform in regard to the device, not the app or website. 

HTML geolocation is best when you need a very detailed location, but it can be too obtrusive when a general location would be enough. Unless you need GPS tracking, I would stay away from this option.

> ...general location like city is enough? Use an IP lookup service.

Don't want to bother the user and general location like city is enough? Use an IP lookup service. Another benefit of this approach is the __extra information__, depending on the provider, most commonly you will get things like currency, language, country, etc. 

Beware, the __accuracy of this approach will depend on the IP__, so if the user is using a VPN, the location can be wildly off (you will get the location of the VPN). 

If you are getting the location based on IP, it's a good idea to check with the user for confirmation or even better, use it only for default values only and let the user change any locale settings easily.

<h2 id="solution">Solution</h2>

%[https://codepen.io/DanielCobo/pen/oNBPQqx]

First we write a function to grab the data from [ipapi.co][2].

```javascript
/**
 * Get data based on IP
 * @param {string} [ip] - IP, default is current
 * @returns {Object} - parsed API response
 */
const getLocale = async function getLocale(ip) {
  let url = 'https://ipapi.co/';
  if (ip) {
    url += ip + '/';
  }
  url += 'json/';
  return fetch(url).then(function (response) {
     return response.json();
  });
};
``` 
The function takes an optional argument `ip`. This is useful in case we have an IP we would like to retrieve data for. The default `ip` value is the so-called public IP. This means that if the user has a VPN or something similar, it will be the VPN IP.

Secondly, we call `getLocale` and [`await`](4) for the response.

```javascript
//Get locale data
const locale = await getLocale().catch(function (err) {
  console.log(err);
});
```

The `locale` object we just created holds all the location data and more. You can read the details in the [API documentation][5].

For us the most interesting properties are:
- `locale.country_name` - name of the country
- `locale.country_code_iso3` - standardised [ISO3 country code][6]
- `locale.region` - in the testing I've done so far only useful for US, where it will give the state name
- `locale.city` - name of the city
- `locale.postal` - zip code

## Notes on using the API

Here I used the API of [ipapi.co][2], but there are [many other IP lookup providers][3] out there. I chose this particular one because:
- their API is super simple
- they offer a good free solution 
- seems no signup is needed 

The above gives me confidence when I want to make a quick prototype or a small project. If I was to release an app to production it's probably better (and more fair) to get a paid plan. 

At the very least be sure to double __check the TOS of whatever API provider you are using__. This way your app is less likely to suddenly lose a critical part of it's functionality due to failing APIs.

[1]:https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API
[2]:https://ipapi.co
[3]:https://stackoverflow.com/a/35123097
[4]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await
[5]:https://ipapi.co/api/
[6]:https://www.iso.org/iso-3166-country-codes.html

