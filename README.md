

# Runway Example ruby App

This is an example app demonstrating how to deploy a ruby app
to [runway](https://runway.planetary-quantum.com/).

* clone this repo, and navigate into that directory
* `runway app create`
* `runway app deploy`
* `runway open`

You can then deploy changes by `git commit`ing them, and running `runway app
deploy` again.

This is the **rails** demo app.

There are different options - you can either do
* `rails new --skip-docker`, and use runway's automatic detection of ruby projects for deployment
* or just `rails new` (as this example did), which will generate a `Dockerfile`, and runway will then use that
    * the latest `rails` generates a working `Dockerfile`, but previous versions
      might have a non-numeric `USER` line (which does not work on runway) - just apply [this patch manually](https://github.com/rails/rails/commit/7ff33d8b3c6d2c5187ac2a59067fce8eea7ceba3)

### `RAILS_MASTER_KEY`

Rails's encrypted credentials file setup needs a build-time and run-time env
variable called `RAILS_MASTER_KEY`. `rails new ...` creates that key, in
`config/master.key`, but it's gitignored (as it should be), so runway won't see
it.

So we need to set it:
```
# normally, you would run `rails new` yourself, and then do
runway app config set RAILS_MASTER_KEY=$(cat config/master.key)

# for this example only, that value is
runway app config set RAILS_MASTER_KEY=7c2260da949a113f9b8f0a5d7d4556ba
```

### Bundler

It's important to **not** push a `.bundle/config` to runway. Just put `.bundle`
into the `.gitignore` (as it is in this example), and that should work.

You might need to run `bundle lock --add-platform x86_64-linux`, so the build
works on runway (but there will be an error message telling you that, if it is
necessary).

Other than that, `rails` apps work out of the box - `bundle exec rails
assets:precompile` is automatically called, and the webserver is selected by
what you have in your `Gemfile` (`puma` by default, for rails).

