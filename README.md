# TWire

TWire is a social network built in rails, used as an example of Hotwire and its sorrounding technologies

## Required software

- Postgresql 9.4+ (14 recommended)
- Redis
- Ruby 3.0.3
  - It's suggested to install it using rbenv or rvm as version managers. Their official guides should be able to help you (see https://rvm.io/ or https://github.com/rbenv/rbenv)

## Installation

```bash
bundle install
rails db:create db:migrate
```

## Usage

```bash
rails server
```


## Specs & linter

```bash
bundle exec rubocop # -A if you want to use autosolve
bundle exec rspec
```

## Deployment

### Initial deployment
```
# deploy redis
cd deploy/redis
fly launch --image flyio/redis:6.2.6 --no-deploy --name twire-redis # Pick your own name!
fly volumes create redis_server --size 1

redis_password=$(rails secret) # you can see it by doing echo $redis_password
fly secrets set REDIS_PASSWORD=$redis_password 

flyctl deploy
cd -

# Deploy main app
flyctl launch # Answer yes, create a new Postgresql DB, and wait till it finishes
flyctl secrets set REDIS_URL=redis://user:$redis_password@twire-redis.internal:6379/
flyctl deploy
```

### Normal deployments

```
flyctl deploy
```


### Running commands in the remote environment
```
flyctl ssh console
cd app 
bundle exec rails console
```

### Github actions

Github actions are configured to automatically run on any push to master


## TODOs & Contribution

This project is just an example on how to use Hotwire and https://fly.io/, therefore some are features and fixes are pending, you can find a list of them here https://github.com/hdf1986/twire/issues .

Feel free to fork the project with your fix, and to open a pull request once you think it's ready. You can also collaborate reporting issues at https://github.com/hdf1986/twire/issues

## License

MIT License. Copyright 2022 Hugo David Farji