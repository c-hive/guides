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
