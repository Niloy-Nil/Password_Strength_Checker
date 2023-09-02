# Password_Strength_Checker


Installing
----------
### Server-side (nodejs) ###
From the command line:

```sh
npm install owasp-password-strength-test
```

### In-browser ###
Within your document:

```html
<script src='owasp-password-strength-test.js'></script>
```

Features
--------
This module is built upon the following beliefs:

1. [Passphrases are better than passwords][xkcd].

2. Passwords should be subject to stricter complexity requirements than
   passphrases.




Usage
-----
After you've included it into your project, using the module is
straightforward:

### Server-side ###
```javascript
// require the module
var owasp = require('owasp-password-strength-test');

// invoke test() to test the strength of a password
var result = owasp.test('correct horse battery staple');
```


The returned value will take this shape when the password is valid:

```javascript
{
  errors              : [],
  failedTests         : [],
  requiredTestErrors  : [],
  optionalTestErrors  : [],
  passedTests         : [ 0, 1, 2, 3, 4, 5, 6 ],
  isPassphrase        : false,
  strong              : true,
  optionalTestsPassed : 4
}

```

... and will take this shape when the password is invalid:

```javascript
{
  errors: [
      'The password must be at least 10 characters long.',
      'The password must contain at least one uppercase letter.',
      'The password must contain at least one number.',
      'The password must contain at least one special character.'
    ],
    failedTests         : [ 0, 4, 5, 6 ],
    passedTests         : [ 1, 2, 3 ],
    requiredTestErrors  : [
      'The password must be at least 10 characters long.',
    ],
    optionalTestErrors  : [
      'The password must contain at least one uppercase letter.',
      'The password must contain at least one number.',
      'The password must contain at least one special character.'
    ],
    isPassphrase        : false,
    strong              : false,
    optionalTestsPassed : 1
}
```




Configuring
-----------
The module may be configured as follows:


```javascript
var owasp = require('owasp-password-strength-test');

// Pass a hash of settings to the `config` method. The settings shown here are
// the defaults.
owasp.config({
  allowPassphrases       : true,
  maxLength              : 128,
  minLength              : 10,
  minPhraseLength        : 20,
  minOptionalTestsToPass : 4,
});
```




Extending
---------
If you would like to filter passwords through additional tests beyond the
default, you may simply push new tests onto the appropriate arrays within the
module's `test` object:

```javascript
var owasp = require('owasp-password-strength-test');

// push "required" tests onto `tests.required` array, and push "optional" tests
// onto the `tests.optional` array.
owasp.tests.required.push(function(password) {
  if (password === 'one two three four five') {
    return "That's the kind of thing an idiot would have on his luggage!";
  }
});
```


Testing
-------
To run the module's test suite, `cd` into its directory and run `npm test`. You
may first need to run `npm install` to install the required development
dependencies. (These dependencies are **not** required in a production
environment, and facilitate only unit testing.)


Contributing
------------
If you would like to contribute code, please fork this repository, make your
changes, and then submit a pull-request.

[guidelines]: https://www.owasp.org/index.php/Authentication_Cheat_Sheet#Implement_Proper_Password_Strength_Controls
[xkcd]: http://xkcd.com/936/ 
