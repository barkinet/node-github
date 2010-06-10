# JavaScript GitHub API for node.js

A node.js module, which provides an object oriented wrapper for the GitHub API. This library is a direct port of [php-github-api](http://github.com/ornicar/php-github-api) by Thibault Duplessis to JavaScript. The only major difference is that the JavaScript code is fully asynchronous.

## Example

Print all followers of the user "fjakobs" to the console.

    var sys = require("sys");
    var GitHubApi = require("github").GitHubApi;

    var github = new GitHubApi(true);
    github.getUserApi().getFollowers('fjakobs', function(err, followers) {
        sys.puts(followers.join('\n');
    });

First the _GitHubApi_ class is imported from the _github_ module. This class provides access to all of GitHub's APIs (e.g. user, commit or repository APIs). The _getFollowers_ method lists all followers of a given GitHub user. Is is part of the user API. It takes the user name as first argument and a callback as last argument. Once the follower list is returned from the server, the callback is called.

Like in node.js callbacks are always the last argument. If the functions fails an error object is passed as first argument to the callback.

## Authentification

Most GitHub API calls don't require authentification. As a rule of thumb: If you can see the information by visiting the site without being logged in, you don't have to be authenticated to retrieve the same information through the API. Of course calls, which change data or read sensitive information have to be authenticated.

You need the GitHub user name and the API key for authentification. The API key can be found in the user's _Account Settings_ page.

This example shows how to authenticate and then change _location_ field of the account settings to _Argentinia_:

    github.authenticate(username, token);
    github.getUserApi().update(username, {location: "Argentinia"}, function(err) {
        sys.puts("done!");
    });

Note that the _authenticate_ method is synchronous because it only stores the credentials for the next request.

## Running the Tests

The unit tests are based on the [node-async-test](http://github.com/bentomas/node-async-testing) module, which is provided as a git submodule. This has to be initialized once using:

    git submodule update --init
    
Running all unit tests:

    node test/AllTests.js
    
The test classes can also be run separately. This will e.g. run the UserApi test:

    node test/UserApiTest.js
    
Note that a connection to the internet is required to run the tests.

## TODO

* Port missing methods from the Issues API
* Implement the Gist AIP
* port to CommonJS (this should be easy because only the 'doSend' method is node specific)
* Integrate with node package managers
* API docs
  * fix and polish (there might still be some PHP-isms)
  * generate API documentation
* Documentation

## LICENSE

This code is licensed under a MIT license. See the LICENSE file for details.

## Credits

Thanks to Thibault Duplessis for the excellent php-github-api library, which is the blueprint of this library. Especially the unit tests prooved to be very helpful.