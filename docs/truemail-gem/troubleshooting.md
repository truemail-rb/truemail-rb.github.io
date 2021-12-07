# Troubleshooting

## SMTP validation fails

First at all you should check is your verifier host keeps this [preconditions](https://truemail-rb.org/truemail-gem/#/quick-start?id=verifier-host-preconditions). You should follow these simple rules for the best email SMTP validation outcome.

## Long SMTP validation time

To reduce SMTP validation time for failure cases you can enable fail fast feature.

## Reducing SMTP validation time

Example of Truemail SMTP validation delay time optimized configuration. It's a mix of minimal networks timeouts and enabling of SMTP fail fast feature.

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
  config.connection_timeout = 1
  config.response_timeout = 1
  config.connection_attempts = 1
  config.smtp_fail_fast = true
end
```

## Using in non Ruby project

You can use Truemail outside of your Ruby project via [Truemail Server](https://truemail-rb.org/truemail-rack). For today we have 3 native clients ([Ruby](https://truemail-rb.org/truemail-ruby-client), [Crystal](https://truemail-rb.org/truemail-crystal-client), [Java](https://truemail-rb.org/truemail-java-client)) for this implementation.
