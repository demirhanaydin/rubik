# Rubik

Rubik is a gem to track realtime quantitive metrics such as the average duration of a method executed. Rubik is not suitable for long term metric collection since it 

## Installation

Add this line to your application's Gemfile:

    gem 'rubik', git: 'git@github.com:matrushka/rubik.git'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install rubik

## Usage
### Configuration
There are 2 options for configuration.

#### Class Method
You can set the redis server by using the class method "redis=".

```ruby
Rubik.redis = Redis.new
```

#### Global Variable
Rubik uses the global $redis variable if the redis server is not set with the class method.

```ruby
$redis = Redis.new
```


### Method Tracking
Rubik can track instance method execution times easily.

```ruby
class MyClass
  include Rubik
  track_method :run

  def run
    sleep 1
  end
end
```

Also Rubik can track inline block execution times.

```ruby
class MyClass
  include Rubik

  def run
    track_as 'myclass#run_method' do
      # do some stuff
      sleep 1
    end
  end
end
```

### Custom Metrics
Just use the Rubik.push_metric method to push your custom metric.

```ruby
  Rubik.push_metric 'my-metric-name', value
```

## Monitoring
### Rails 3+
Add the following to your  "config/routes.rb"

```ruby
require 'rubik/server'
mount Rubik::Server => '/rubik'
```

And then you can use the mounted route to see the monitoring dashboard.

### Stand alone
Here's an example config.ru for booting Rubik::Server in your choice of Rack Server

```ruby
require 'rubik/server'
Rubik.redis = Redis.new
run Rubik::Server.new
```

## TODO
* Complete the test suit
* Optimize the Rubik.query method
* Complete docs
* Implement a configruation system for changing sample size
* Calculate estimated size of a full metric in Redis for docs
* Think about a way to group metrics and split them into seperate dashboards

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
