# Configuration

> Please note, you should have [global](#global-configuration) or [custom independent configuration](#independent-configuration) for using Truemail gem.

## Configurable options

- verifier email *(required)*
- verifier domain
- email pattern
- SMTP error body pattern
- connection timeout
- response timeout
- connection attempts
- default validation type
- validation type for domains
- whitelisted domains
- whitelist validation
- blacklisted domains
- different MX lookup scenarios
- SMTP safe check
- event logger

## Global configuration

### Setting configuration

To have an access for `Truemail.configuration` and gem configuration features, you must configure it first as in the example below:

```ruby
require 'truemail'

Truemail.configure do |config|
  # Required parameter. Must be an existing email on behalf of which verification
  # will be performed
  config.verifier_email = 'verifier@example.com'

  # Optional parameter. Must be an existing domain on behalf of which verification
  # will be performed. By default verifier domain based on verifier email
  config.verifier_domain = 'somedomain.com'

  # Optional parameter. You can override default regex pattern
  config.email_pattern = /regex_pattern/

  # Optional parameter. You can override default regex pattern
  config.smtp_error_body_pattern = /regex_pattern/

  # Optional parameter. Connection timeout is equal to 2 ms by default.
  config.connection_timeout = 1

  # Optional parameter. A SMTP server response timeout is equal to 2 ms by default.
  config.response_timeout = 1

  # Optional parameter. Total of connection attempts. This parameter uses in
  # mx lookup timeout error and smtp request for cases when there is one mx server.
  # It is equal to 2 by default.
  config.connection_attempts = 3

  # Optional parameter. You can predefine default validation type for
  # Truemail.validate('email@email.com') call without with-parameter
  # Available validation types: :regex, :mx, :smtp
  config.default_validation_type = :mx

  # Optional parameter. You can predefine which type of validation will be used
  # for domains. Also you can skip validation by domain.
  # Available validation types: :regex, :mx, :smtp
  # This configuration will be used over current or default validation type
  # parameter. All of validations for 'somedomain.com' will be processed with
  # regex validation only. And all of validations for 'otherdomain.com' will be
  # processed with mx validation only. It is equal to empty hash by default.
  config.validation_type_for = {
    'somedomain.com' => :regex,
    'otherdomain.com' => :mx
  }

  # Optional parameter. Validation of email which contains whitelisted domain
  # always will return true. Other validations will not processed even if it
  # was defined in validation_type_for. It is equal to empty array by default.
  config.whitelisted_domains = ['somedomain1.com', 'somedomain2.com']

  # Optional parameter. With this option Truemail will validate email which
  # contains whitelisted domain only, i.e. if domain whitelisted, validation
  # will passed to Regex, MX or SMTP validators. Validation of email which
  # not contains whitelisted domain always will return false.
  # It is equal false by default.
  config.whitelist_validation = true

  # Optional parameter. Validation of email which contains blacklisted domain
  # always will return false. Other validations will not processed even if it
  # was defined in validation_type_for. It is equal to empty array by default.
  config.blacklisted_domains = ['somedomain1.com', 'somedomain2.com']

  # Optional parameter. This option will provide to use not RFC MX lookup flow.
  # It means that MX and Null MX records will be cheked on the DNS validation
  # layer only. By default this option is disabled.
  config.not_rfc_mx_lookup_flow = true

  # Optional parameter. This option will be parse bodies of SMTP errors. It will
  # be helpful if SMTP server does not return an exact answer that the email does
  # not exist. By default this option is disabled, available for SMTP
  # validation only.
  config.smtp_safe_check = true

  # Optional parameter. This option will enable tracking events. You can print
  # tracking events to stdout, write to file or both of these. Available tracking
  # event: :all, :unrecognized_error, :recognized_error, :error
  # Tracking event by default is :error
  config.logger = {
    tracking_event: :all,
    stdout: true,
    log_absolute_path: '/home/app/log/truemail.log'
  }
end
```

### Reading configuration

After successful configuration, you can read current Truemail configuration instance anywhere in your application.

```ruby
Truemail.configuration

=> #<Truemail::Configuration:0x000055590cb17b40
 @connection_timeout=1,
 @email_pattern=/regex_pattern/,
 @smtp_error_body_pattern=/regex_pattern/,
 @response_timeout=1,
 @connection_attempts=3,
 @validation_type_by_domain={},
 @whitelisted_domains=[],
 @whitelist_validation=true,
 @blacklisted_domains=[],
 @verifier_domain="somedomain.com",
 @verifier_email="verifier@example.com",
 @not_rfc_mx_lookup_flow=true,
 @smtp_safe_check=true,
 @logger=#<Truemail::Logger:0x0000557f837450b0
   @event=:all, @file="/home/app/log/truemail.log", @stdout=true>>
```

### Updating configuration

```ruby
Truemail.configuration.connection_timeout = 3
=> 3
Truemail.configuration.response_timeout = 4
=> 4
Truemail.configuration.connection_attempts = 1
=> 1

Truemail.configuration
=> #<Truemail::Configuration:0x000055590cb17b40
 @connection_timeout=3,
 @email_pattern=/regex_pattern/,
 @smtp_error_body_pattern=/regex_pattern/,
 @response_timeout=4,
 @connection_attempts=1,
 @validation_type_by_domain={},
 @whitelisted_domains=[],
 @whitelist_validation=true,
 @blacklisted_domains=[],
 @verifier_domain="somedomain.com",
 @verifier_email="verifier@example.com",
 @not_rfc_mx_lookup_flow=true,
 @smtp_safe_check=true,
 @logger=#<Truemail::Logger:0x0000557f837450b0
   @event=:all, @file="/home/app/log/truemail.log", @stdout=true>>
```

### Reseting configuration

Also you can reset Truemail configuration.

```ruby
Truemail.reset_configuration!
=> nil
Truemail.configuration
=> nil
```

## Independent configuration

You can use independent configuration for each validation/audition instance. When using this feature you do not need to have [Truemail global configuration](#global-configuration).

```ruby
custom_configuration = Truemail::Configuration.new do |config|
  config.verifier_email = 'verifier@example.com'
end

Truemail.validate('email@example.com', custom_configuration: custom_configuration)
Truemail.valid?('email@example.com', custom_configuration: custom_configuration)
Truemail.host_audit('email@example.com', custom_configuration: custom_configuration)
```
