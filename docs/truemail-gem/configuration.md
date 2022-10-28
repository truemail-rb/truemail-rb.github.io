# Configuration

?> Please note, you should have [global](#global-configuration) or [custom independent configuration](#independent-configuration) for using Truemail gem.

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
- whitelisted emails
- blacklisted emails
- whitelisted domains
- blacklisted domains
- whitelist validation
- blacklisted mx ip-addresses
- custom DNS gateway(s)
- RFC MX lookup flow
- SMTP port number
- SMTP fail fast
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

  # Optional parameter. Connection timeout in seconds.
  # It is equal to 2 by default.
  config.connection_timeout = 1

  # Optional parameter. A SMTP server response timeout in seconds.
  # It is equal to 2 by default.
  config.response_timeout = 1

  # Optional parameter. Total of connection attempts. This parameter uses in
  # mx lookup timeout error and smtp request for cases when there is one mx server.
  # It is equal to 2 by default.
  config.connection_attempts = 3

  # Optional parameter. You can predefine default validation type for
  # Truemail.validate('email@email.com') call without with-parameter
  # Available validation types: :regex, :mx, :mx_blacklist, :smtp
  config.default_validation_type = :mx

  # Optional parameter. You can predefine which type of validation will be used
  # for domains. Also you can skip validation by domain.
  # Available validation types: :regex, :mx, :mx_blacklist, :smtp
  # This configuration will be used over current or default validation type
  # parameter. All of validations for 'somedomain.com' will be processed with
  # regex validation only. And all of validations for 'otherdomain.com' will be
  # processed with mx validation only. It is equal to empty hash by default.
  config.validation_type_for = {
    'somedomain.com' => :regex,
    'otherdomain.com' => :mx
  }

  # Optional parameter. Validation of email which contains whitelisted emails
  # always will return true. Other validations will not processed even if it
  # was defined in validation_type_for. It is equal to empty array by default.
  config.whitelisted_emails = %w[user@somedomain1.com user@somedomain2.com]

  # Optional parameter. Validation of email which contains blacklisted emails
  # always will return false. Other validations will not processed even if it
  # was defined in validation_type_for. It is equal to empty array by default.
  config.blacklisted_emails = %w[user@somedomain3.com user@somedomain4.com]

  # Optional parameter. Validation of email which contains whitelisted domain
  # always will return true. Other validations will not processed even if it
  # was defined in validation_type_for. It is equal to empty array by default.
  config.whitelisted_domains = ['somedomain1.com', 'somedomain2.com']

  # Optional parameter. Validation of email which contains blacklisted domain
  # always will return false. Other validations will not processed even if it
  # was defined in validation_type_for. It is equal to empty array by default.
  config.blacklisted_domains = ['somedomain3.com', 'somedomain4.com']

  # Optional parameter. With this option Truemail will validate email which
  # contains whitelisted domain only, i.e. if domain whitelisted, validation
  # will passed to Regex, MX or SMTP validators. Validation of email which
  # not contains whitelisted domain always will return false.
  # It is equal false by default.
  config.whitelist_validation = true

  # Optional parameter. With this option Truemail will filter out unwanted mx
  # servers via predefined list of ip addresses. It can be used as a part of
  # DEA (disposable email address) validations. It is equal to empty array by
  # default.
  config.blacklisted_mx_ip_addresses = ['1.1.1.1', '2.2.2.2']

  # Optional parameter. This option will provide to use custom DNS gateway when
  # Truemail interacts with DNS. Valid port numbers are in the range 1-65535.
  # If you won't specify nameserver's ports Truemail will use default DNS
  # TCP/UDP port 53. By default Truemail uses DNS gateway from system settings
  # and this option is equal to empty array.
  config.dns = ['10.0.0.1', '10.0.0.2:54']

  # Optional parameter. This option will provide to use not RFC MX lookup flow.
  # It means that MX and Null MX records will be cheked on the DNS validation
  # layer only. By default this option is disabled.
  config.not_rfc_mx_lookup_flow = true

  # Optional parameter. SMTP port number. It is equal to 25 by default.
  config.smtp_port = 2525

  # Optional parameter. This option will provide to use smtp fail fast behaviour.
  # When smtp_fail_fast = true it means that Truemail ends smtp validation session
  # after first attempt on the first mx server in any fail cases (network
  # connection/timeout error, smtp validation error). This feature helps to reduce
  # total time of SMTP validation session up to 1 second. By default this option
  # is disabled.
  config.smtp_fail_fast = true

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
 @default_validation_type=:mx,
 @validation_type_by_domain={"somedomain.com" => :regex, "otherdomain.com" => :mx},
 @whitelisted_emails=["user@somedomain1.com", "user@somedomain2.com"],
 @blacklisted_emails=["user@somedomain3.com", "user@somedomain4.com"],
 @whitelisted_domains=["somedomain1.com", "somedomain2.com"],
 @whitelist_validation=true,
 @blacklisted_domains=["somedomain3.com", "somedomain4.com"],
 @blacklisted_mx_ip_addresses=["1.1.1.1", "2.2.2.2"],
 @dns=["10.0.0.1", "10.0.0.2:54"],
 @verifier_domain="somedomain.com",
 @verifier_email="verifier@example.com",
 @not_rfc_mx_lookup_flow=true,
 @smtp_port=2525,
 @smtp_fail_fast=true,
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
 @default_validation_type=:mx,
 @validation_type_by_domain={"somedomain.com" => :regex, "otherdomain.com" => :mx},
 @whitelisted_emails=["user@somedomain1.com", "user@somedomain2.com"],
 @blacklisted_emails=["user@somedomain3.com", "user@somedomain4.com"],
 @whitelisted_domains=["somedomain1.com", "somedomain2.com"],
 @whitelist_validation=true,
 @blacklisted_domains=["somedomain3.com", "somedomain4.com"],
 @blacklisted_mx_ip_addresses=["1.1.1.1", "2.2.2.2"],
 @dns=["10.0.0.1", "10.0.0.2:54"],
 @verifier_domain="somedomain.com",
 @verifier_email="verifier@example.com",
 @not_rfc_mx_lookup_flow=true,
 @smtp_port=2525,
 @smtp_fail_fast=true,
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
