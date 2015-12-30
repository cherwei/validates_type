## validates_type

![Build Status](https://travis-ci.org/yez/validates_type.svg?branch=master)

### Rails type validation

#### Purpose

Most Rails applications will have types coerced by their ORM connection adapters (like the `pg` gem or `mysql2`). However, this only useful for applications with very well defined schemas. If your application has a legacy storage layer that you can no longer modify or a lot of `store_accessor` columns, this solution is a nice middle ground to ensure your data is robust.

This also prevents your data from being coerced into values that you did not intend by ActiveRecord adapters.

#### Usage

##### With ActiveRecord

```ruby
class Foo < ActiveRecord::Base

  # validate that attribute :bar is a String
  validates_type :bar, :string

  # validate that attribute :baz is an Integer with a custom error message
  validates_type :baz, :integer, message: 'Baz must be an Integer'

  # validate that attribute :qux is an Array, allow blank
  validates_type :qux, :array, allow_blank: true

  # validate that attribute :whatever is a Boolean
  validates_type :whatever, :boolean

  # validate that attribute :thing is a Float or nil
  validates_type :thing, :float, allow_nil: true

  # validate that attribute :when is a Time
  validates_type :when, :time

  # validate that attribute :birthday is a Date or blank
  validates_type :birthday, :date, allow_blank: true

  # validate that attribute :fox is of type Custom
  validates_type :fox, Custom
end
```

##### With ActiveModel

```ruby
class Bar
  include ActiveModel::Validations

  attr_accessor :foo, :qux

  validates_type :foo, :string

  # Custom error message support
  validates_type :qux, :boolean, message: 'Attribute qux must be a boolean!'
end
```

##### With shortcut syntax

```ruby
class Banana < ActiveRecord::Base

  # The banana's peel attribute must be a string
  validates :peel, type: { type: :string }

  # Custom error message for ripeness of banana
  validates :ripe, type: { type: :boolean, message: 'Only ripe bananas allowed' }
end
```

##### With multiple modifiers

```ruby
class Foo < ActiveRecord::Base
  # validate that attribute :baz is an Integer with a custom error message
  #   only if :conditional_method evaluates to true
  validates_type :baz, :integer, message: 'Baz must be an Integer', if: :conditional_method

  def conditional_method
    # some kind of logic that is important to pass
  end

  # validate that attribute :baz is a Float and is included in a specific array
  validates_type :foo, :float, in: [1.0, 2.5, 3.0]
end
```

#### Supported types

- `:array`
- `:big_decimal`
- `:boolean`
- `:date`
- `:float`
- `:hash`
- `:integer`
- `:string`
- `:symbol`
- `:time`

#### Any class name, possibly something custom defined in your app, is also game.

#### Contributing

Please feel free to submit pull requests to address issues that you may find with this software. One commit per pull request is preferred please and thank you.
