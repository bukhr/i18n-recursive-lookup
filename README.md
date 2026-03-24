# I18n Recursive Lookup ![Build status](https://api.travis-ci.org/annkissam/i18n-recursive-lookup.png)

Provides a backend to the i18n gem to allow a definition to contain embedded references to other definitions by introducing the special embedded marker `${}`.

All definitions are lazily evaluated on lookup, and once compiled they're written back to the translation store so that all interpolation happens once.

Supports recursive lookups in **strings**, **hashes**, and **arrays**.

## Examples

### String interpolation

```yml
foo:
  bar: boo
baz: ${foo.bar}
```

`I18n.t(:baz)` will correctly evaluate to `boo`.

### Hash recursion

```yml
messages:
  greeting: "Hello ${user.name}"
  farewell: "Goodbye ${user.name}"
user:
  name: "Alice"
```

`I18n.t(:messages)` will return: `{ greeting: "Hello Alice", farewell: "Goodbye Alice" }`.

### Array interpolation

```yml
foo: world
greetings:
  - "Hello ${foo}"
  - "Goodbye ${foo}"
```

`I18n.t(:greetings)` will correctly evaluate to `["Hello world", "Goodbye world"]`.

### Nested structures

```yml
base: "value"
nested:
  items:
    - "Item: ${base}"
    - "Another: ${base}"
  data:
    key: "${base}"
```

All nested arrays and hashes will be recursively processed with embedded references resolved.

## Installation

Install the gem either by putting it in your `Gemfile`

```bash
gem 'i18n-recursive-lookup'
```

or by installing it using rubygems

```bash
gem install i18n-recursive-lookup
```

Add it to your existing backend by adding these lines to your `config/initializers/i18n.rb` (create one if one doesn't exist):

```ruby
# config/initializers/i18n.rb
require 'i18n/backend/recursive_lookup'
I18n::Backend::Simple.send(:include, I18n::Backend::RecursiveLookup)
```

Of course you can replace the `I18n::Backend::Simple` with whatever backend you wish to use.

## TODO

- add detection for infinite embedding cycles
