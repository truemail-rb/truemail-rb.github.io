# Quick Start

## Requirements

!> Ruby MRI 2.5.0+

## Verifier host preconditions

?> You should follow these simple rules for the best email SMTP validation outcome.

1. [Verifier email](terminology?id=verifier-email) should be existing
2. [Verifier domain](terminology?id=verifier-domain) should be existing and the same as in verifier email)
3. Verifier domain should have [MX records](terminology?id=mx-record)
4. Verifier domain should refer to [verefier host](terminology?id=verifier-host) IP address
5. Verefier host should have [PTR record](terminology?id=ptr-record)
6. Verefier host and verifier email shouldn't be in email blacklists databases

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'truemail'
```

And then execute:

```bash
bundle
```

Or install it yourself as:

```bash
gem install truemail
```

## DSL example

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
end

Truemail.validate('email@example.com') # => returns Truemail::Validator instance
Truemail.valid?('email@example.com')   # => returns Boolean
```

It's very generalized example, more details and real usecase examples of Truemail usage you can find in next sections.
