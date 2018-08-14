---
title: Maps4News API Documentation

language_tabs:
  - javascript--wrapper: Wrapper
  - php: PHP

toc_footers:
  - <a href='https://api.maps4news.com/register'>Sign Up for Maps4News</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>
  
includes:
  - v1/directory

search: true
---

# Introduction

Welcome to the API documentation for Maps4News.

Our API allows you to manage everything about your Maps4News account and your organisation. As well as generate Maps.

# API Wrapper

> You can install the library using:

```
npm install @mapcreator/maps4news
```

If you are using JavaScript to develop your app then you are in luck. 
We have created a query builder-like library that is able to do everything our API offers. It even does the Oauth login for you, in redirect, popup or password flow.

The library is freely available on [github](https://github.com/MapCreatorEU/api-wrapper) and [npm](https://www.npmjs.com/package/@mapcreator/maps4news).

More information about the wrapper can be found on [this page](wrapper.html)

# Authentication

> To authorize, use this code:

```javascript
import { ImplicitFlow, Maps4News } from '@mapcreator/maps4news';

const API_CLIENT_ID = 0;
const API_HOST = 'https://api.maps4news.com';
const REDIRECT_URL = 'http://localhost/';

const auth = new ImplicitFlow(API_CLIENT_ID, REDIRECT_URL);
const api = new Maps4News(auth, API_HOST);

// Somewhere in your application
api.authenticate();

// Get the user's information
api.users.get('me').then(console.log);
```

```php
This example uses the guzzlehttp package from composer.

<?php

$host = "https://api.maps4news.com/v1";
$client_id = 0;
$secret = "secret";
$redirect_url = "http://localhost/callback";

////////////////////////////
// /login route in your app.

// Prepare redirect to API login page.
$query = http_build_query([
    'client_id' => $client_id,
    'redirect_uri' => 'http://localhost/callback',
    'response_type' => 'code'
]);

// Redirect user.
header("Location: $host/oauth/authorize?$query");

//////////////////////////////
// /callback route in your app.

$http = new GuzzleHttp\Client();

// Get the user's access_token.
$response = $http->post("$host/oauth/token", [
    'form_params' => [
        'grant_type' => 'authorization_code',
        'client_id' => $client_id,
        'client_secret' => $secret,
        'redirect_uri' => $redirect_url,
        'code' => $_POST['code']
    ]
]);

// Get the access token.
$token = json_decode((string) $response->getBody(), true)['access_token'];

// Request the user's info.
$response = $http->get("$host/v1/users/me", [
    'headers' => [
        'Authorization' => "Bearer $token",
        'Accept' => 'application/json'
    ]
]);

// Display the user's information
print_r(json_decode((string) $response->getBody()));
```

> Make sure the <code>client_id</code>, <code>host</code> and <code>redirect_url</code> are correctly filled in.

The Maps4News API is an OAuth2 API. We support implicit and password flows.

<aside class="notice">
You must have a valid client registered with us to be able to use the API. Clients are currently on request.
</aside>

# Next

The next step in learning about our API would be to look into our OpenAPI documentation about all the available endpoints.

[API OpenAPI (Swagger) Definition](api/index.html)

Our you can look into how the wrapper works.

[API Wrapper Class Documentation](/wrapper/index.html)