The repository is just for trying Rails 7 alpha2.

## Structure

- Rails 7 alpha2
  - import-map (`--javascript=importmap`)
  - `--skip-webpacker`
  - SQLite3
- Ruby 3.1.0-preview1
  - YJIT（`RUBY_YJIT_ENABLE=1`）

Note that Webpacker/Webpack, node, and yarn are not included. Use the new import map instead.

## Environment

- Docker version 20.10.7, build f0df350
- docker-compose version 1.29.2, build 5becea4c
- [dip](https://github.com/bibendi/dip) 7.1.4 (optional)

## Steps

Assumed that `dip` command is available. You can alse use normal `docker-compose` commands.

- `dip compose build`
- `dip bundle install`
- `dip sh` to login the container and run `rails new --database=postgresql` with whatever options you like. The following is just my preference.

```sh
rails new . --database=postgresql --skip-active-storage --skip-action-mailer --skip-active-job --skip-action-cable --skip-action-mailbox --skip-action-text
```

- Add config to `config/environments/development.rb` like the following:

```ruby
config.hosts << "localhost" << "lvh.me"
config.web_console.whitelisted_ips = '0.0.0.0/0'
```

- Temporal fix: add the followint to the `test` group in Gemfile to add [rexml](https://github.com/ruby/rexml) and then run `dip bundle install`. See [#43248](https://github.com/rails/rails/issues/43248). I expect the issue will be fixed by the final release of Rails 7.

```ruby
gem 'rexml', '~> 3.2', '>= 3.2.5'
```

- `dip provision`
- `dip rails s` and open `lvh.me:3000` on your browser.

## import mapping

Read [importmap-rails README](https://github.com/rails/importmap-rails) first.

Well, you can use `dip im` to invoke `./bin/importmap`.