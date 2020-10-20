# Usage

To have an access for `Truemail::Client` you must configure it first as in the example below:

## Global configuration

### Setting configuration

```ruby
require 'truemail/client'

Truemail::Client.configure do |config|
  # Optional parameter (Boolean). By default it is equal false
  config.secure_connection = true

  # Required parameter (String). It should be a hostname or an ip address where Truemail server runs
  config.host = 'example.com'

  # Optional parameter (Integer). By default it is equal 9292
  config.port = 80

  # Required parameter (String). It should be valid Truemail server access token
  config.token = 'token'
end
```

### Reading configuration

After successful configuration, you can read current `Truemail::Client` configuration instance anywhere in your application.

```ruby
Truemail::Client.configuration
=> #<Truemail::Client::Configuration:0x000055eafc588878
  @host="example.com",
  @port=80,
  @secure_connection=true,
  @token="token">
```

### Updating configuration

```ruby
Truemail::Client.configuration.port = 8080
=> 8080

Truemail::Client.configuration
=> #<Truemail::Client::Configuration:0x000055eafc588878
  @host="example.com",
  @port=8080,
  @secure_connection=true,
  @token="token">
```

### Reseting configuration

Also you can reset Truemail::Client configuration.

```ruby
Truemail::Client.reset_configuration!
=> nil
Truemail::Client.configuration
=> nil
```

## Email validation/verification

After successful configuration, you can establish connection with Truemail server and validate an email.

```ruby
Truemail::Client.validate('admin@bestweb.com.ua')
=>  {
      "date": "2020-02-26 17:00:56 +0200",
      "email": "admin@bestweb.com.ua",
      "validation_type": "smtp",
      "success": true,
      "errors": null,
      "smtp_debug": null,
      "configuration": {
        "validation_type_by_domain": null,
        "whitelist_validation": false,
        "whitelisted_domains": null,
        "blacklisted_domains": null,
        "smtp_safe_check": false,
        "email_pattern": "default gem value",
        "smtp_error_body_pattern": "default gem value",
        "not_rfc_mx_lookup_flow": false
      }
    }
```

`Truemail::Client.validate` always returns JSON data. If something goes wrong you will receive JSON with error details:

```json
{
  "truemail_client_error": "error details"
}
```

## Checking server health status

After successful configuration, you can check health-status of Truemail server. It always returns a `boolean`.

```ruby
Truemail::Client.server_healthy?

=> true
```
