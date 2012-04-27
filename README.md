# Cookies.js

Cookies.js is a small javascript library that makes managing cookies easy.
In addition to its simple API, Cookies.js will automatically parse a JSON encoded string value
back into its native data type when accessed.

## Browser Compatibility

Chrome, ???

**Note**: For IE6 and 7 support, the 'JSON.parse' and 'JSON.stringify' functions must be [shimmed](http://en.wikipedia.org/wiki/Shim_\(computing\)).
Douglas Crockford's [json2.js](https://github.com/douglascrockford/JSON-js), or Kit Cambridge's [json3.js](http://bestiejs.github.com/json3/)
library is recommended.

## To Do

1. Write more unit tests, and improve existing ones
2. Run unit tests on a bunch of different browsers
3. Improve documentation


# API Reference

## Methods

### Cookies.set(key, value [, options])
Sets a cookie in the document. If the cookie does not already exist, it will be created.

#### Arguments:
*key*: A string value of the cookie key to set  
*value*: Any type that can be encoded in a JSON string (via JSON.stringify())  
*options*: An object containing additional parameters about the cookie (discussed below)

#### Returns:
The 'Cookies' object is returned to support chaining.

#### The 'options' Object:
*path*: A string value of the path of the cookie  
*domain*: A string value of the domain of the cookie  
*expires*: A number (of seconds), a date parsable string, or a Date object of when the cookie will expire  
*secure*: A boolean value of whether or not the cookie should only be available over SSL

If any property is left undefined, the browser's default value will be used instead. A default value
for any property may be set in the 'Cookies.defaults' object.

**Why use 'expires' instead of 'max-age' (or why not both)?**  
Internet Explorer 6 - 8 do not support 'max-age', so Cookies.js always uses 'expires' internally.
However, Cookies.js simplifies things by allowing the 'expires' property in the 'options' object to be used
in the same way as 'max-age' (by setting 'expires' to the number of seconds the cookie should exist for).

#### Example usage:
    // Setting values of various data types
    Cookies.set('string', 'value');
    Cookies.set('number', 123);
    Cookies.set('array', [1, 2, 3]);
    Cookies.set('object', { hello: 'world' });
    
    // Chaining sets together
    Cookies.set('string', 'value').set('number', 123);
    
    // Setting cookies with additional options
    Cookies.set('string', 'value', { path: '/', secure: true });
    
    // Setting cookies with expiration values
    Cookies.set('string', 'value', { expires: 600 }); // Expires in 10 minutes
    Cookies.set('string', 'value', { expires: '01-01-2012' });
    Cookies.set('string', 'value', { expires: new Date(2012, 0, 1) });

### Cookies.get(key)
*Alias: **Cookies(key)***

Retrieves the cookie value of the most locally scoped cookie with the specified key.
If the cookie value is a JSON encoded string, the parsed JSON value will be returned.

#### Arguments:
*key*: A string value of a cookie key

#### Returns:
A JSON parsed representation of the cookie value, if it can be parsed, otherwise the string value of the cookie.

#### Example Usage:
    // First set some cookies
    Cookies.set('string', 'value');
    Cookies.set('number', 123);
    Cookies.set('object', { hello: 'world' });
    
    // Get the cookie values (as its original data type)
    Cookies.get('string'); // "value"
    Cookies.get('number'); // 123
    Cookies.get('object'); // { hello: 'world' }
    
    // Using the alias
    Cookies('string'); // "value"
    
### Cookies.expire(key [, options])
Expires a cookie, removing it from the document.

#### Arguments:
*key*: A string value of the cookie key to expire  
*options*: An object containing additional parameters about the cookie (discussed below)

#### Returns:
The 'Cookies' object is returned to support chaining.

#### The 'options' Object
*path*: A string value of the path of the cookie  
*domain*: A string value of the domain of the cookie

If any property is left undefined, the browser's default value will be used instead. A default value
for any property may be set in the 'Cookies.defaults' object.

#### Example Usage:
    // First set a cookie and get its value
    Cookies.set('string', 'value').get('string'); // "value"
    
    // Expire the cookie and try to get its value
    Cookies.expire('string').get('string'); // undefined
    

## Properties

### Cookies.enabled
A boolean value of whether or not the browser has cookies enabled.

#### Example Usage:
    if (Cookies.enabled) {
        Cookies.set('key', 'value');
    }

### Cookies.defaults
An object representing default options to be used when setting and expiring cookie values.
By default, 'Cookies.defaults' is an empty object, but supports the following properties:

*path*: A string value of the path of the cookie  
*domain*: A string value of the domain of the cookie  
*expires*: A number (of seconds), a date parsable string, or a Date object of when the cookie will expire  
*secure*: A boolean value of whether or not the cookie should only be available over SSL

If any property is left undefined, the browser's default value will be used instead.

#### Example Usage:
    Cookies.defaults = {
        path: '/',
        secure: true
    };
    
    Cookies.set('key', 'value'); // Will be secure and have a path of '/'
    Cookies.expire('key'); // Will expire the cookie with a path of '/'