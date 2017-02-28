---
layout: default
---

## Install dependencies

Ruby dependencies are usually managed through bundler.

### Install dependencies

Install dependencies defined in Gemfile

    bundle install

### Run commands using bundler

If you wanted to run any command using the dependencies, run

    bundle exec <command>

e.g.

    bundle exec sidekiq

Note, most used commands for example `rails` and `guard` have [binstubs](http://bundler.io/v1.10/bundle_binstubs.html) generated in `bin/` folder. For instance, launch Rails console with:

    bin/rails console

which is equivalent to

    bundle exec rails console

## Migrations

### Run migrations

Run migrations defined in `db/migrate` folder in default environment run

    bin/rails db:migrate

Give environment in `RAILS_ENV` environment variable

    RAILS_ENV=test bin/rails db:migrate

### Create migrations

Migrations can be generated with [Rails migration generator](http://edgeguides.rubyonrails.org/active_record_migrations.html)

    bin/rails generate migration AddNameToUser

It generates a migration file to `db/migrate` that can be modified. http://edgeguides.rubyonrails.org/active_record_migrations.html contains more information about the DSL.

## Running Rails applications

### Foreman

We run rails applications using [Foreman](https://www.theforeman.org/). The commands run are defined in `Procfile`.

To run all the services

    bundle exec foreman

Run only one service

    bundle exec foreman web

## Testing

Tests are written in [RSpec](http://rspec.info/).

### Run whole test suite

To run whole test suite

    bin/rspec

### Watch for changes and run only relevant files

[Guard](https://github.com/guard/guard) is used to watch for file changes and run only relevant tests

    bin/guard

and then edit a file.

## Log files

Log files are located in `log/` folder. Each environment has its own file.

## Rails console

### Opening console

Rails console can be accessed with

    bin/rails console

It contains full Rails runtime, so one can access e.g. models directly

    [3] pena(main)> catalog = ProductCatalog.first
      ProductCatalog Load (0.5ms)  SELECT  "product_catalogs".* FROM "product_catalogs" ORDER BY "product_catalogs"."id" ASC LIMIT $1  [["LIMIT", 1]]
    => #<ProductCatalog:0x007fbd585410d8
     ....
     config_changed_at: nil>
    [4] pena(main)> catalog.config = {foo: 'bar'}
    => {:foo=>"bar"}
    [5] pena(main)> catalog.save!
       (0.2ms)  BEGIN
      SQL (3.0ms)  UPDATE "product_catalogs" SET "updated_at" = $1, "config" = $2 WHERE "product_catalogs"."id" = $3  [["updated_at", 2017-02-28 20:36:22 UTC], ["config", "{\"foo\":\"bar\"}"], ["id", 1]]
      SQL (3.7ms)  INSERT INTO "versions" ......
       (0.3ms)  COMMIT
    => true

### Pry

[Pry](https://github.com/pry/pry) is the replacement console for the default irb. Pry knows few nice tricks:

#### Show documentation

Show documentation for class/method use `?`

    [13] rails(main)> ? String#length

    From: string.c (C Method):
    Owner: String
    Visibility: public
    Signature: length()
    Number of lines: 1

    Returns the character length of str.

Supports at least RDOC style

#### Show implementation

Show implementation of class/method, use `$`

    [18] rails(main)> $ ProductFeed#latest_upload

    From: /Users/oskari/smartly/pena/app/models/product_feed.rb @ line 91:
    Owner: ProductFeed
    Visibility: public
    Number of lines: 3

    def latest_upload
      recent_uploads.first
    end

#### Show methods

Show methods of class/module, use `ls`

    [19] pena(main)> ls ProductFeed::S3Output
    constants: IoDelegator  MULTIPART_CHUNK_SIZE
    Object.methods: JSON  yaml_tag
    ProductFeed::S3Output#methods: after_finish  close  error  finish  start  upload  write

## Debugging

### Print variable

Variable can be printed to screen with `puts`

    puts user

If the variable is not string, it will be converted using `to_s` method. To access the more detailed information, `#inspect` function can be used

    puts user.inspect

### Debugger

[Byebug](https://github.com/deivid-rodriguez/byebug) is a gdb style debugger that can be used to debug Ruby. It's similar to Python's pdb.

#### Attach debugger

Add `binding.pry` into the source to break after that line. It will break into the terminal window running the process:

    Frame number: 0/84

    From: /pena/app/controllers/product_catalogs_controller.rb @ line 7 ProductCatalogsController#index:

         5: def index
         6:   binding.pry
     =>  7:   catalogs = Facebook::ProductCatalog.list(
         8:     facebook,
         9:     params[:business_id],
        10:     params.slice(:fields, :limit)
        11:   ).sort_by(&:name)
        12:
        13:   render json: catalogs
        14: end

Move around with `next`, `step`, `continue`. The prompt works as a Rails console, so variables can be stored and arbitrary statements run.

**Note!** For some reason Foreman does not render the input of the characters even though it process them. So either drive blind or run the debugged process separately from Foreman.

## Docker

Docker is used through [docker-compose](https://docs.docker.com/compose/). To run a command inside docker container

    docker-compose run web whoami

    docker-compose run test bin/guard

For example these two commands (in separate terminals) can be used to launch sidekiq workers using Foreman but Rails application without, so you can see debug input (see: Debugger note):

    docker-compose run web bundle exec foreman start worker
    docker-compose run --service-ports web bundle exec puma -p 11000 -C config/puma.rb
