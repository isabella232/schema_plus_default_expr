[![Gem Version](https://badge.fury.io/rb/schema_plus_default_expr.svg)](http://badge.fury.io/rb/schema_plus_default_expr)
[![Build Status](https://secure.travis-ci.org/SchemaPlus/schema_plus_default_expr.svg)](http://travis-ci.org/SchemaPlus/schema_plus_default_expr)
[![Coverage Status](https://img.shields.io/coveralls/SchemaPlus/schema_plus_default_expr.svg)](https://coveralls.io/r/SchemaPlus/schema_plus_default_expr)
[![Dependency Status](https://gemnasium.com/lomba/schema_plus_default_expr.svg)](https://gemnasium.com/SchemaPlus/schema_plus_default_expr)

# SchemaPlus::DefaultExpr

SchemaPlus::DefaultExpr extends ActiveRecord's migrations to allow you to set the default value of a column to an SQL expression.   

This gem works with PostgreSQL and Sqlite3, but not MySQL; see [Compatibility](#compatibility)below.

SchemaPlus::DefaultExpr is part of the [SchemaPlus](https://github.com/SchemaPlus/) family of Ruby on Rails ActiveRecord extension gems.

## Installation

<!-- SCHEMA_DEV: TEMPLATE INSTALLATION - begin -->
<!-- These lines are auto-inserted from a schema_dev template -->
As usual:

```ruby
gem "schema_plus_default_expr"                # in a Gemfile
gem.add_dependency "schema_plus_default_expr" # in a .gemspec
```

<!-- SCHEMA_DEV: TEMPLATE INSTALLATION - end -->

## <a name="compatibility"></a>Compatibility

SchemaPlus::DefaultExpr is tested on:

<!-- SCHEMA_DEV: MATRIX - begin -->
<!-- These lines are auto-generated by schema_dev based on schema_dev.yml -->
* ruby **2.1.5** with activerecord **4.2**, using **sqlite3** and **postgresql**

<!-- SCHEMA_DEV: MATRIX - end -->

MySQL only supports SQL expression defaults for `TIMESTAMP` column types, which ActiveRecord does not use.  So SchemaPlus::DefaultExpr does not work with MySQL.

## Usage

SchemaPlus::DefaultExpr augments the syntax for setting column defaults, to support expressions or constant values:

    t.datetime :seen_at, default: { expr: 'NOW()' }
    t.datetime :seen_at, default: { value: "2011-12-11 00:00:00" }

The standard syntax will still work as usual:

    t.datetime :seen_at, default: "2011-12-11 00:00:00"

Also, as a convenience

    t.datetime :seen_at, default: :now

resolves to:

    NOW()                 # PostgreSQL
    (DATETIME('now'))     # SQLite3
    invalid               # MySQL

### Note on PostgreSQL & json:

If you are using Postgresql with a `json` column, ActiveRecord allows you to use a Hash for a default value.  Be aware that if the hash contains just one key which is `:expr` or `:value`, then SchemaPlus::DefaultExpr will interpret, and use the corresponding value for the default.  That is, these are equivalent:

	t.json :fields, default: { value: { field1: 'a', field2: 'b' } }
    t.json :fields, default: { field1: 'a', field2: 'b' }


## History

* 0.1.2 - Explicit gem dependencies
* 0.1.1 - Fix dumping bug (#1); upgrade to schema_plus_core 1.0
* 0.1.0 - Initial release, extracted from schema_plus 1.x

## Development & Testing

Are you interested in contributing to SchemaPlus::DefaultExpr?  Thanks!  Please follow
the standard protocol: fork, feature branch, develop, push, and issue pull
request.

Some things to know about to help you develop and test:

<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_DEV - begin -->
<!-- These lines are auto-inserted from a schema_dev template -->
* **schema_dev**:  SchemaPlus::DefaultExpr uses [schema_dev](https://github.com/SchemaPlus/schema_dev) to
  facilitate running rspec tests on the matrix of ruby, activerecord, and database
  versions that the gem supports, both locally and on
  [travis-ci](http://travis-ci.org/SchemaPlus/schema_plus_default_expr)

  To to run rspec locally on the full matrix, do:

        $ schema_dev bundle install
        $ schema_dev rspec

  You can also run on just one configuration at a time;  For info, see `schema_dev --help` or the [schema_dev](https://github.com/SchemaPlus/schema_dev) README.

  The matrix of configurations is specified in `schema_dev.yml` in
  the project root.


<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_DEV - end -->

<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_PLUS_CORE - begin -->
<!-- These lines are auto-inserted from a schema_dev template -->
* **schema_plus_core**: SchemaPlus::DefaultExpr uses the SchemaPlus::Core API that
  provides middleware callback stacks to make it easy to extend
  ActiveRecord's behavior.  If that API is missing something you need for
  your contribution, please head over to
  [schema_plus_core](https://github.com/SchemaPlus/schema_plus_core) and open
  an issue or pull request.

<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_PLUS_CORE - end -->

<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_MONKEY - begin -->
<!-- These lines are auto-inserted from a schema_dev template -->
* **schema_monkey**: SchemaPlus::DefaultExpr is implemented as a
  [schema_monkey](https://github.com/SchemaPlus/schema_monkey) client,
  using [schema_monkey](https://github.com/SchemaPlus/schema_monkey)'s
  convention-based protocols for extending ActiveRecord and using middleware stacks.

<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_MONKEY - end -->
