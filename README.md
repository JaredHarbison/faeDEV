# Fae Rails 6 reproduction app

This repository is a minimal Rails 6 application created in January 2020 to
reproduce a Fae installation bug.

Running Fae's installer generated migrations that inherited directly from
`ActiveRecord::Migration`. Rails 6 rejected those migrations because the Rails
version they targeted was not specified. Updating the generated classes to
inherit from `ActiveRecord::Migration[4.2]` allowed the installation and
migrations to complete.

The repository preserves the failing integration context and the verified
workaround. It is not a standalone product or a general-purpose Fae example.

## Reproduction context

- Ruby 2.6.1
- Rails 6.0.2
- `fae-rails` 1.7
- SQLite

The relevant history is captured in the initial setup commits:

1. Generate a Rails 6 application.
2. Add `fae-rails` and run `rails generate fae:install`.
3. Observe the generated migration compatibility failure.
4. Add `[4.2]` to the generated Fae migration base classes.
5. Run `rails db:create` and `rails db:migrate` successfully.
6. Add Fae's CSS and JavaScript assets to the Rails 6 manifest.

## Historical status

This is an archived reproduction fixture with an intentionally old dependency
lockfile. It should not be deployed or used as the starting point for a new
application. The open dependency alerts reflect the age of the fixture rather
than an actively maintained product.
