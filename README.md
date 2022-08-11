

# Runway Example ruby App

This is an example app demonstrating how to deploy a ruby app
to [runway](https://runway.planetary-quantum.com/).

* clone this repo, and navigate into that directory
* `runway app create`
* `runway app deploy`
* `runway open`

You can then deploy changes by `git commit`ing them, and running `runway app
deploy` again.

This is the **rails** demo app (created with `rails new`).

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
runway app config set RAILS_MASTER_KEY=41e7465012996c66bb8984cb72ab3bb6
```

### Bundler

It's important to **not** push a `.bundle/config` to runway. Just put `.bundle`
into the `.gitignore` (as it is in this example), and that should work.

Other than that, `rails` apps work out of the box - `bundle exec rails
assets:precompile` is automatically called, and the webserver is selected by
what you have in your `Gemfile` (`puma` by default, for rails).

