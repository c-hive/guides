# Rails / Best practices

#### Fail on missing translations in test env

Expectations in specs might be set using `I18n.t` meaning that the missing translation error texts would be compared successfully. Add a [custom handler](https://guides.rubyonrails.org/i18n.html#using-different-exception-handlers) that raises an error in test environment when keys are missing.

`lib/i18n_raise_exception_handler.rb`
```ruby
module I18n
  class RaiseExceptionHandler < ExceptionHandler
    def call(exception, locale, key, options)
      raise exception.to_exception if exception.is_a?(I18n::MissingTranslation)

      super
    end
  end
end
```

`config/environments/test.rb`
```ruby
require "#{Rails.root}/lib/i18n_raise_exception_handler.rb"

Rails.application.configure do
  config.i18n.exception_handler = I18n::RaiseExceptionHandler.new
end
```

#### Use lambda literals for consecutive `after_commit` hooks

There can only be a single `after_commit` hook defined for a model with the exception of using lambda literals. See also:
- https://github.com/rubocop-hq/rubocop-rails/issues/52
- https://github.com/rails/rails/issues/19590
- https://github.com/rails/rails/issues/29554

BAD
```ruby
after_create_commit :log_create
after_update_commit :log_update

after_commit :callback, :on => :create
after_commit :callback, :on => :update, :if => :condition
```

GOOD
```
after_create_commit -> { foo }
after_update_commit -> { foo }
```

See:
- https://github.com/rubocop-hq/rubocop-rails/issues/52

