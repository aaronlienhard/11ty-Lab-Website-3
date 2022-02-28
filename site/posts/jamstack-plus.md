---
title: Using the GeoIP Element + Wikipedia Query Tag
description: Second dev.to blog post about GeoIP element and Wikipedia Query Tag
date: 2022-01-31
tags:
  - Wikipedia Query
  - Web Components
  - GeoIP Element
layout: layouts/post.njk
---
## What is the GeoIP element?

The GeoIP element allows you to take a computer's ip address and convert it to both a longitude and latitude. Using this element, a webpage can instantly return a geolocation just based off of the computer's ip address that it accesses the webpage from.

Check out more information on the GeoIP at this link: [GeoIP](https://freegeoip.app/)

## What can we do with the GeoIP element + Wikipedia Query tag?

By using the geolocation returned with the GeoIP element, you're able to search parts of that location with the Wikipedia Query tag. When the search is completed, the query tag displays the Wikipedia article that is associated with the search term you feed the tag. 

If you want to add the wikipedia query tag to your webpage, simply import it at the top of your JavaScript File using this code:

```
import '@lrnwebcomponents/wikipedia-query/wikipedia-query.js';
```

You can also check out component [here](https://www.npmjs.com/package/@lrnwebcomponents/wikipedia-query)

For example, if I wanted to query Wikipedia for the city and state that is returned from the element, the code would look like this:

```
<wikipedia-query search="${this.city}, ${this.state}"></wikipedia-query>
```


Then, if the location was "University Park, United States" then the wikipedia-query tag would return this on the webpage: 

![University Park, United States Example Wikipedia Query](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fxr22hqj81yewoqli8su.png)

## How does the GeoIP element grab this data?

The GeoIP element uses fetch(), this is what the fetch command looks like within the element:

```
fetch(this.locationEndpoint + userIPData.ip)
```

The first part of the fetch gets, "this.locationEndpoint" this part is connected to [Free Geo IP](https://freegeoip.app/json/). If you go to this website, then you'll get a json response that shows a json file that displays various information based on that system's ip address. Here is an example of what it shows: 

```
{
"ip": "23.105.173.167",
"country_code": "US",
"country_name": "United States",
"region_code": "DC",
"region_name": "District of Columbia",
"city": "Washington",
"zip_code": "20016",
"time_zone": "America/New_York",
"latitude": 38.9379,
"longitude": -77.0859,
"metro_code": 511
}
```

The second part of the fetch grabs the user's ip data from the userIP JavaScript file. Here's the code it uses in order to get the user's ip address:

```
  async updateUserIP() {
    return fetch(this.ipLookUp)
      .then(resp => {
        if (resp.ok) {
          return resp.json();
        }
        return false;
      })
      .then(data => {
        this.ip = data.ip;
        return data;
      })
      .then(data => {
        this.location = `${data.city}, ${data.country}`;
        return data;
      });
  }
```

Using these two data points, the element is able to convert the user's ip address that it gets and change it into a usable longitude and latitude of that ip's location.

## What can we do with the longitude and latitude we receive?

With the longitude and latitude from the fetch() method, we can plug those values into a Google Maps url and embed that location's Google Map's result into the webpage. We do that using this code:

```
https://maps.google.com/maps?q=${this.lat},${this.long}&t=&z=15&ie=UTF8&iwloc=&output=embed
```

Here's an example of what that embed looks like on the website:

![Google Maps Embed Example](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/864d3e40icmsx1syswok.png)

## My github repository to view the used project's code

If you want to look at the ip-project github repo, you can do so from this link: [IP Project Repository](https://github.com/aaronlienhard/ip-project)