# RSpec Rails Examples [![Build Status](https://travis-ci.org/eliotsykes/rspec-rails-examples.svg?branch=master)](https://travis-ci.org/eliotsykes/rspec-rails-examples) [![Join the chat at https://gitter.im/eliotsykes/rspec-rails-examples](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/eliotsykes/rspec-rails-examples?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

> An RSpec cheatsheet in the form of a Rails app. Learn how to expertly test Rails apps from a model codebase


The repo contains examples of various spec types such as feature, mailer, and model. See the [spec/](spec/) directory for all the example specs and types.

See the well-commented files in the [spec/support](spec/support) directory for walkthroughs on how to configure popular testing gems, such as DatabaseCleaner, Capybara, and FactoryGirl.

**PS. Interested in growing your skills *and* supporting this project?** Learn with the [TDD Masterclass](https://eliotsykes.com/#tdd), get [Test Coverage First Aid](https://eliotsykes.com/#coverage) for your app, or grow with [one-to-one coaching for Rails developers](https://eliotsykes.com/#coach).

---

<!-- MarkdownTOC depth=0 autolink=true bracket=round -->

- [Support Configuration](#support-configuration)
- [Run Specs in a Random Order](#run-specs-in-a-random-order)
- [Testing Production Errors](#testing-production-errors)
- [Testing Rake Tasks with RSpec](#testing-rake-tasks-with-rspec)
- [Pry-rescue debugging](#pry-rescue-debugging)
- [Time Travel Examples](#time-travel-examples)
- [Database Cleaner Examples](#database-cleaner-examples)
- [Factory Girl Examples](#factory-girl-examples)
- [Shoulda-Matchers Examples](#shoulda-matchers-examples)
- [Custom Matchers](#custom-matchers)
- [RSpec-Expectations Docs](#rspec-expectations-docs)
- [RSpec-Mocks Specs & Docs](#rspec-mocks-specs--docs)
- [RSpec-Rails](#rspec-rails)
  - [Matchers](#matchers)
  - [Generators](#generators)
  - [Controller Specs & Docs](#controller-specs--docs)
  - [View Specs & Docs](#view-specs--docs)
- [Validator Specs](#validator-specs)

<!-- /MarkdownTOC -->

# Support Configuration

The tests rely on this configuration being uncommented in `spec/rails_helper.rb`, you probably want it uncommented in your app too:
```ruby
# Loads `.rb` files in `spec/support` and its subdirectories:
Dir[Rails.root.join("spec/support/**/*.rb")].each { |f| require f }
```
(The rspec-rails installer generates this line, but it will be commented out.)


# Run Specs in a Random Order

In a dependable, repeatable automated test suite, data stores (such as database, job queues, and sent email on `ActionMailer::Base.deliveries`) should return to a consistent state between each spec, regardless of the order specs are run in.

For a maintainable, predictable test suite, one spec should not set up data (e.g. creating users) needed by a later spec to pass. Each spec should look after its own test data and clear up after itself. (NB. If there is reference data that all tests need, such as populating a `countries` table, then this can go in `db/seeds.rb` and be run once before the entire suite).

The specs run in a random order each time the test suite is run. This helps prevent the introduction of run order and test data dependencies between tests, which are best avoided.

Random order test runs are configured using the `config.order = :random` and `Kernel.srand config.seed` options in [spec/spec_helper.rb](spec/spec_helper.rb).


# Testing Production Errors

When errors are raised, the Rails test environment may not behave as in production. The test environment may mask the actual error response you want to test. To help with this, you can disable test environment exception handling temporarily, [spec/support/error_responses.rb](spec/support/error_responses.rb) provides `respond_without_detailed_exceptions`. See it in use in [spec/api/v1/token_spec.rb](spec/api/v1/token_spec.rb) to provide production-like error responses in the test environment.


# Pry-rescue debugging
pry-rescue can be used to debug failing specs, by opening pry's debugger whenever a test failure is encountered. For setup and usage see [pry-rescue's README](https://github.com/ConradIrwin/pry-rescue).


# Time Travel Examples

[`ActiveSupport::Testing::TimeHelpers`](http://api.rubyonrails.org/classes/ActiveSupport/Testing/TimeHelpers.html) provides helpers for manipulating and freezing the current time reported within tests. These methods are often enough to replace the time-related testing methods that the `timecop` gem is used for.

`TimeHelpers` configuration how-to and examples:
- [spec/support/time_helpers.rb](spec/support/time_helpers.rb)
- [spec/models/subscription_spec.rb](spec/models/subscription_spec.rb)


# Database Cleaner Examples

[Database Cleaner](https://github.com/DatabaseCleaner/database_cleaner) is a set of strategies for cleaning your database in Ruby, to ensure a consistent environment for each test run.

Database Cleaner configuration how-to:
- [spec/support/database_cleaner.rb](spec/support/database_cleaner.rb)


# Factory Girl Examples

[Factory Girl](https://github.com/thoughtbot/factory_girl) is a library to help setup test data. [factory_girl_rails](https://github.com/thoughtbot/factory_girl_rails) integrates Factory Girl with Rails.

Factory Girl configuration how-to and examples:
- [spec/support/factory_girl.rb](spec/support/factory_girl.rb)
- [spec/factories](spec/factories)
- [spec/factories/users.rb](spec/factories/users.rb)
- [spec/models/subscription_spec.rb](spec/models/subscription_spec.rb)


# Shoulda-Matchers Examples

[Shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers) make light work of model specs.

shoulda-matchers configuration how-to and examples:
- [spec/support/shoulda_matchers.rb](spec/support/shoulda_matchers.rb)
- [spec/models/subscription_spec.rb](spec/models/subscription_spec.rb)


# Custom Matchers

You can write your own custom RSpec matchers. Custom matchers can help you write more understandable
specs.

Custom matchers configuration how-to and examples:
- [spec/support/matchers.rb](spec/support/matchers.rb)
- [spec/matchers](spec/matchers)
- [spec/matchers/be_pending_subscription_page.rb](spec/matchers/be_pending_subscription_page.rb)
- Chainable matcher: [spec/matchers/be_confirm_subscription_page.rb](spec/matchers/be_confirm_subscription_page.rb)
- [spec/matchers/have_error_messages.rb](spec/matchers/have_error_messages.rb)
- [spec/features/subscribe_to_newsletter_spec.rb](spec/features/subscribe_to_newsletter_spec.rb)
- Lightweight matcher with `satisfy`: [spec/api/v1/token_spec.rb](spec/api/v1/token_spec.rb)


# RSpec-Expectations Docs
- [RSpec-Expectations API](http://www.rubydoc.info/gems/rspec-expectations/frames)
- [RSpec-Expectations matchers](https://www.relishapp.com/rspec/rspec-expectations/docs/built-in-matchers)
- [Expectations matchers cheatsheet](https://gist.github.com/hpjaj/ef5ba70a938a963332d0)


# RSpec-Mocks Specs & Docs
- [spec/controllers/subscriptions_controller_spec.rb](spec/controllers/subscriptions_controller_spec.rb)
- [spec/models/subscription_spec.rb](spec/models/subscription_spec.rb)
- [RSpec Mocks API](https://relishapp.com/rspec/rspec-mocks/docs)

# RSpec-Rails
See [RSpec Rails](https://relishapp.com/rspec/rspec-rails/docs) for installation instructions.

## Matchers
- https://relishapp.com/rspec/rspec-rails/docs/matchers

## Generators
- https://relishapp.com/rspec/rspec-rails/docs/generators

## Controller Specs & Docs
- [spec/controllers/subscriptions_controller_spec.rb](spec/controllers/subscriptions_controller_spec.rb)
- [Controller specs API](https://relishapp.com/rspec/rspec-rails/docs/controller-specs)
- [Controller specs cheatsheet](https://gist.github.com/eliotsykes/5b71277b0813fbc0df56)

## View Specs & Docs
- [The Big List of View Specs](https://eliotsykes.com/view-specs)
- [View specs API](https://relishapp.com/rspec/rspec-rails/docs/view-specs)

# Validator Specs

To test a custom validator you've written, refer to these validator specs from other Rails projects. These specs each follow a similar pattern where the validator is tested with a dummy model that is defined and used within the spec only. Using a dummy model is usually preferable to writing a validator spec that is dependent on a real model.

- [blacklist_validator_spec.rb](https://github.com/calagator/calagator/blob/b5fb7098fb94627b9791a0e40686be4d80c9c0c9/spec/lib/calagator/blacklist_validator_spec.rb) from Calagator
- [quality_title_validator_spec.rb](https://github.com/discourse/discourse/blob/00342faff9593a78d4c27c774ff75e1dd8819f34/spec/components/validators/quality_title_validator_spec.rb) from Discourse
- [phone_number_validator_spec.rb](https://github.com/netguru/people/blob/410c8f9355b7295af9711aeade8210a1a97e0a0c/spec/validators/phone_number_validator_spec.rb) from Netguru-People
- [no_empty_spaces_validator_spec.rb](https://github.com/danbartlett/opensit/blob/9d434bc6157b470c479f44c87c945c4652d37db1/spec/validators/no_empty_spaces_validator_spec.rb) from OpenSit

Related task: [Demonstrate Validator Specs within rspec-rails-examples](https://github.com/eliotsykes/rspec-rails-examples/issues/106)