# Fae CMS Evaluation

This repository is a focused Rails prototype used to evaluate
[Fae](https://github.com/wearefine/fae) as an embedded content-management
system. It preserves the smallest useful integration: a Rails host application,
the Fae engine mounted under `/admin`, CMS authentication and roles, editable
content primitives, and the extension points for navigation and authorization.

## Why this exists

The goal was to understand the cost and shape of adding an opinionated admin CMS
to an existing Rails application. Keeping the host application deliberately
small made the framework itself easier to inspect: which tables and assets it
owns, how its engine is mounted, and where an application customizes access and
navigation.

This is an evaluation artifact rather than a deployed product. It is useful as
a record of a technical spike and as a compact example of Rails engine
integration.

## What the prototype covers

- Mounting `Fae::Engine` at `/admin`
- Fae-managed users and role-based administration
- Stage and production states for managed content
- Polymorphic text, image, and file content records
- Static pages and change tracking
- Application-level navigation and authorization concerns
- Fae asset-pipeline customization points

## Architecture

Fae runs as an engine inside the Rails host. The host owns the mount point and
extension concerns; the engine supplies the admin controllers, models, views,
and content schema.

```text
Rails host application
    -> /admin
    -> Fae::Engine
    -> Fae users, roles, pages, assets, and change history
```

The schema illustrates Fae's reusable-content approach. Text fields, text
areas, images, and files use polymorphic associations so the same CMS types can
be attached to different application records.

## Historical stack

- Ruby 2.6.1
- Rails 6.0
- Fae Rails 1.7
- SQLite
- Webpacker 4 and Turbolinks 5
- Minitest

These versions are intentionally documented as historical. Ruby 2.6 and this
dependency set are no longer appropriate for a new or internet-facing
application without a deliberate upgrade and security review.

## Running the archived prototype

Use a version manager capable of installing Ruby 2.6.1, then run:

```sh
bundle install
yarn install
bin/rails db:prepare
bin/rails server
```

Open `http://localhost:3000/admin`. Fae's seed and setup behavior is provided by
the engine version locked in `Gemfile.lock`.

Because the toolchain is old, installation may require an older compiler or
platform-specific dependency work. Treat upgrades as a separate modernization
project rather than silently changing the experiment's baseline.

## Verification

```sh
bin/rails test
```

The repository contains the Rails-generated test harness but no meaningful
application test coverage. That is acceptable for the original dependency
spike, but it is the first gap to close if the prototype is extended.

## What I learned

An engine can add a substantial administrative surface to Rails with very
little host code, but that convenience transfers important architectural
choices—content modeling, authentication, publishing state, and asset
management—to the dependency. The value of a prototype like this is making
those choices visible before adopting the CMS in a larger product.
