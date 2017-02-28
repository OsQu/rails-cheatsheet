---
layout: default
---

Things to add:

- Installation
  * bundle install
- Migrations
  * Running
  * Creating
- Running rails
  * Foreman
  * Manually bundle exec
- Logs
- Rails console
  * Pry
- Debugging
  * puts / inspect
  * debugger

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

Run migrations define in `db/migrate` folder in default environment run

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

## Log files

Log files are located in `log/` folder. Each environment has its own file.

## Rails console


### Opening console

Rails console can be accessed with

    bin/rails console

It contains full Rails runtime, so one can access e.g. models directly

    user = User.find_by_name("OsQu")
    user.email = 'oskari@smartly.io'
    user.save!

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


# Docker

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](another-page).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# [](#header-1)Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## [](#header-2)Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### [](#header-3)Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### [](#header-4)Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### [](#header-5)Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### [](#header-6)Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
