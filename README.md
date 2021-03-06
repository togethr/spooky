# :ghost: Spooky

[![Build Status](https://travis-ci.com/togethr/spooky.svg?branch=master)](https://travis-ci.com/togethr/spooky)

A simple Ruby wrapper for the [Ghost](https://ghost.org) Content API.

The Ghost Content API documentation can be found [here](https://ghost.org/docs/api/v3/content/#endpoints).

## Installation

Add spooky to your Gemfile:

```ruby
gem "spooky"
```

And then run:

    $ bundle

## Usage

To get started, a Ghost client needs to be initialized using the API URL of your Ghost installation and its content API key.

### Initializing a client (`Spooky::Client`)

#### Initialize manually

A client can be created by running the below with your credentials:

```ruby
client = Spooky::Client.new(api_url: "https://blog.yourdomain.com", api_key: "abc123")
```
#### Initialize with ENV variables

Spooky also has ENV variable support out of the box. You can set your credentials by setting the below ENV variables:

Attribute     | ENV variable
:------------ | :--------------------
api_url       | `GHOST_API_URL`
api_key       | `GHOST_CONTENT_API_KEY`

If they above are set then all you have to do to initialize a client is:

```ruby
client = Spooky::Client.new
```
### Fetching Posts

Spooky operates primarily on posts. Methods that return a collection also return a pagination object, if there is one.

#### Get all posts
```ruby
posts, pagination = client.posts
```

Returns an array of `Spooky::Post`s and a pagination meta hash.

#### Get all posts and include tags and authors
```ruby
posts, pagination = client.posts(tags: true, authors: true)
```

Returns an array of `Spooky::Post`s with `Spooky::Tag`s and `Spooky::Author`s where appropriate and a pagination meta hash.

#### Get a post by ID or slug

```ruby
post = client.post_by(id: 'abc123')
```

```ruby
post = client.post_by(slug: 'this-is-a-slug')
```

Both return a `Spooky::Post`.

#### Get posts with a filter applied

Filtering accepts simple hashes as conditions or [NQL strings for more complex filters](https://ghost.org/docs/api/v3/content/#syntax-reference).

```ruby
featured_posts, pagination = client.posts(filter: { featured: true })
```

```ruby
welcome_posts, pagination = client.posts(filter: "title:Welcome")
```

You can exclude a post using NQL operators, note the `-`:

```ruby
posts_without_welcome, pagination = client.posts(tags: true, filter: "title:-Welcome")
```

### Pagination

If applicable, Spooky will return the pagination meta hash directly from Ghost. It looks like this:

```ruby
{
    "page" => 1,
    "limit" => 3,
    "pages" => 3,
    "total" => 7,
    "next" => 2,
    "prev" => nil
}
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Create an issue with your comments or request or open a pull request with your change and accompanying tests.
This project adheres to the included Code of Conduct, see CODE_OF_CONDUCT.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

## Credits

This gem was originally developed by [infinityrobot](https://github.com/infinityrobot).