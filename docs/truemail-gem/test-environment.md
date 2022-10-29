# Test environment

!> Be careful! Don't do SMTP-requests to real SMTP-providers from your test environment. Please use one of approaches from examples below.

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

?> Please note, this way is preferable instead of using stubs.

## Using stubs

You can stub out email validation for your test environment like in RSpec example below:

```ruby
RSpec.describe 'some test' do
  subject(:test_target) { some_class.call }

  before { allow(Truemail).to receive(:valid?).and_return(boolean) }
  # or
  before { allow(Truemail).to receive(:validate).and_return(boolean) }
  # or
  before { allow(Truemail).to receive_message_chain(:validate, :result, :valid?).and_return(boolean) }

  context 'some test context' do
    let(:boolean) { true }

    it { is_expected.to be(boolean) }
  end
end
```

## Using mocks

You can mock DNS and SMTP services with [`dns_mock`](https://github.com/mocktools/ruby-dns-mock), [`smtp_mock`](https://github.com/mocktools/ruby-smtp-mock) and use end-to-end approach for your test environment like in example below:

```ruby
# Gemfile

group :test do
  gem 'dns_mock', require: false
  gem 'smtp_mock', require: false
end

# spec/spec_helper.rb

require 'dns_mock/test_framework/rspec'
require 'smtp_mock/test_framework/rspec'

RSpec.configure do |config|
  config.include DnsMock::TestFramework::RSpec::Helper
  config.include SmtpMock::TestFramework::RSpec::Helper
end

# spec/integration_spec.rb

RSpec.describe 'integration tests' do
  let(:target_email) { random_email }
  let(:dns_mock_records) { dns_mock_records_by_email(target_email, dimension: 2) }

  before do
    dns_mock_server.assign_mocks(dns_mock_records)
    smtp_mock_server(**smtp_mock_server_options)

    Truemail.configuration.tap do |config|
      config.dns = %W[127.0.0.1:#{dns_mock_server.port}]
      config.smtp_port = smtp_mock_server.port
    end
  end

  context 'when checks real email' do
    let(:smtp_mock_server_options) { {} }

    it { expect(Truemail.validate(target_email).result).to be_valid }
  end

  context 'when checks fake email' do
    let(:smtp_mock_server_options) { { not_registered_emails: [target_email] } }

    it { expect(Truemail.validate(target_email).result).not_to be_valid }
  end
end
```

> Truemail RSpec helpers for configuration, auditor & validator instances are available in [`truemail-rspec`](https://truemail-rb.org/truemail-rspec ':target=_self') as independent gem.
