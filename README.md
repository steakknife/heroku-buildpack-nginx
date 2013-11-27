Nginx build pack
========================

This is a build pack bundling Nginx for Heroku apps.

Configuration
-------------

The config files are bundled:

* conf/nginx.conf.erb

### Overriding Configuration Files During Deployment

Create a `conf/` directory in the root of the your deployment. Any files with names matching the above will be copied over and overwitten.

This way, you can customise settings specific to your application, especially the document root in `nginx.conf.erb`. (Note the .erb extension.)

Alternatively, the bundled `nginx.conf.erb` will automatically include all nginx configuration snippets within the application directory: `conf/nginx.d/*.conf`. This is another way that you can modify the `root` and `index` directives. Further, if the config snippets end with `.erb`, they will be parsed and have `.conf` extension appended to its filename. 

### Running App-specific Scripts
Heroku now supports running a single `.profile` script in the root of your application during startup, right before `boot.sh` is executed. See <https://devcenter.heroku.com/articles/dynos#startup>.

For more advanced usage of .profile scripts, see <https://devcenter.heroku.com/articles/profiled>.


### Deploying
To use this buildpack, on a new Heroku app:
````
heroku create -s cedar -b https://github.com/steakknife/heroku-buildpack-nginx.git
````

On an existing app:
````
heroku config:add BUILDPACK_URL=https://github.com/steakknife/heroku-buildpack-nginx.git
````

Push deploy your app and you should see Nginx bundled.

**Note**: There are two branches in this buildpack, `master` and `develop`.
The former is the default and the latter has more recently released versions of upstream software.
To select the `develop` branch, append `#develop` to the buildpack URL above, without any spaces.

Testing the Buildpack
---------------------
Setup the test environment on Heroku as follows:
```
$ cd heroku-buildpack-nginx/
$ heroku create -s cedar -b https://github.com/steakknife/heroku-buildpack-nginx.git
Creating deep-thought-1234... done, stack is cedar
http://deep-thought-1234.herokuapp.com/ | git@heroku.com:deep-thought-1234.git
Git remote heroku added
```

Then, push the buildpack to be tested into Heroku:
```
$ git push -f heroku <branch>:master  # where <branch> is the git branch you want to test.
```

Finally, run those tests:
```
$ heroku run tests-with-caching
```

If you run your tests programatically, you might need the follow command instead:
```
$ heroku run tests-with-caching | bin/report
```

Source: <https://github.com/ryanbrainard/heroku-buildpack-testrunner>

Credits
-------

Forked from <https://github.com/iphoting/heroku-buildpack-php-tyler>

Original buildpack adapted and modified for Nginx + PHP support by [Ronald Ip][iht]. Buildpack originally inspired, and forked from <https://github.com/heroku/heroku-buildpack-php>.

Credits to original authors.

[iht]: http://ronaldip.com/

