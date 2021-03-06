# Capistrano Resque

Basic tasks for putting some Resque in your Cap.

### In your Capfile:

```
require "capistrano-resque"
```

### In your deploy.rb:

```
set :workers, { "my_queue_name" => 2 }
```

You can also specify multiple queues and the number of workers
for each queue:

```
set :workers, { "archive" => 1, "mailing" => 3, "search_index, cache_warming" => 1 }
```

The above will start five workers in total:

 * one listening on the `archive` queue
 * one listening on the `search_index, cache_warming` queue
 * three listening on the `mailing` queue

### The tasks

Running cap -vT | grep resque should give you...

```
➔ cap -vT | grep resque
cap resque:start     # Start Resque workers
cap resque:stop      # Quit running Resque workers
cap resque:restart   # Restart running Resque workers
```

### Restart on deployment

To restart you workers automatically when `cap deploy:restart` is executed
add the following line to your `deploy.rb`:

```
after "deploy:restart", "resque:restart"
```

## Logging

Currently, resque doesn't log to a logfile. I'm using the 'logfile' branch of http://github.com/sj26/resque in my consuming Gemfile to achieve this functionality. Hopefully that will get merged into defunkt/resque soon.
