Rack::Timeout::Select
=============

A fork of [Rack::Timeout](https://github.com/heroku/rack-timeout) with extended functionality that allows filtering timeout by Rails request routes.
Default behaviour is still preserved, for documentation on original middleware see the link above.


Basic Usage
-----------

Pass an an array of paths strings you want to exclude from or only run timeout for.

### Rails apps, manually


```ruby
# Gemfile
gem "rack-timeout", require:"rack/timeout/base", :git => 'git://github.com/mkrl/rack-timeout.git'
```

```ruby
# config/initializers/rack_timeout.rb

# insert middleware wherever you want in the stack, optionally pass
# initialization arguments, or use environment variables
Rails.application.config.middleware.insert_before Rack::Runtime, Rack::Timeout::Select, service_timeout: 5, exclude: ["statistics"]

```

Or include the original `Rack::Timeout` instead, if you want to temporary disable path filtering.

Configuring
-----------

Same as the original Rack::Timeout, it takes the following settings, shown here with their
default values and associated environment variables.

```
service_timeout:   15     # RACK_TIMEOUT_SERVICE_TIMEOUT
wait_timeout:      30     # RACK_TIMEOUT_WAIT_TIMEOUT
wait_overtime:     60     # RACK_TIMEOUT_WAIT_OVERTIME
service_past_wait: false  # RACK_TIMEOUT_SERVICE_PAST_WAIT
exclude:           []     # RACK_TIMEOUT_EXCLUDE
only:              []     # RACK_TIMEOUT_ONLY
```

Both `exclude` and `only` can be used at the same time, in this case excluded paths will be substracted from the `only` array.

These settings can be overriden during middleware initialization or
environment variables `RACK_TIMEOUT_*` mentioned above. Middleware
parameters take precedence:

```ruby
use Rack::Timeout::Select, service_timeout: 5, exclude: ["api"]
```
[Demo application](https://github.com/mkrl/rack-timeout-test)
Please note that you may have controller actions with names similar to your excludes/targets, use with wise.

---
Copyright © 2010-2016 Caio Chassot, released under the MIT license
<http://github.com/heroku/rack-timeout>
