# Rails / Tips & tricks

#### How to test Rails functionality in a non-Rails project

E.g. a gem providing `ActiveSupport::Concern`s

`lib/services/oauth2_scopes.rb`
```ruby
require 'active_support/concern'

module Services
  module Oauth2Scopes
    extend ActiveSupport::Concern

    included do
      # ...
    end
  end
end
```

`spec/services/oauth2_scopes_spec.rb`
```ruby
require 'rails'
require 'action_view' # rspec/rails depends on it
require 'action_controller'
require 'rspec/rails'

# In order to test the concerns around action a base rails setup is needed
# setup an application in order to draw route
class Application < Rails::Application; end

RSpec.describe Services::Oauth2Scopes, type: :controller do
  # A new controller can be defined per context if needed
  controller(ActionController::Base) do
    include Services::Oauth2Scopes

    def index
      render json: {}
    end
  end

  it 'allows action' do
    get :index

    expect(response).to be_successful
  end
end
```
