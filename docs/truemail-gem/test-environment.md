# Test environment

## Using whitelist/blacklist feature

With [whitelist/blacklist Truemail feature](validations-layers?id=whitelistblacklist-check) you can define validation behavior for test and staging environment:

```ruby
# config/initializers/truemail.rb

Truemail.configure do |config|
  config.verifier_email = 'your_verifier@email.com'

  unless YourApp.env.production?
    config.whitelisted_domains = Constants::Email::WHITE_DOMAINS
    config.blacklisted_domains = Constants::Email::BLACK_DOMAINS
  end
end
```

> Please note, this is the preferred way instead of using stubs.

## Using stubs

You can stub out email validation for your test environment. RSpec example:

```ruby
RSpec.describe 'some test' do
  subject(:test_terget) { some_class.call }

  before { allow(Truemail).to receive(:valid?).and_return(boolean) }
  # or
  before { allow(Truemail).to receive(:validate).and_return(boolean) }
  # or
  before do
    allow(Truemail).to receive_message_chain(:validate, :result, :valid?).and_return(boolean)
  end

  context 'some test context' do
    let(:boolean) { true }

    it { is_expected.to be(true) }
  end
end
```
