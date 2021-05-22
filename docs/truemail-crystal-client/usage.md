# Usage

First you have to import `Truemail::Client`:

```crystal
require "truemail-client"
```

To have an access for `Truemail::Client` you must configure it first as in the example below:

## Creating configuration instance

```crystal
configuration = Truemail::Client::Configuration.new do |config|
  # Required parameter (String). It should be a hostname or an ip address where Truemail server runs
  config.host = "example.com"

  # Required parameter (String). It should be valid Truemail server access token
  config.token = "token"

  # Optional parameter (Boolean). By default it is equal false
  config.secure_connection = false

  # Optional parameter (Int32). By default it is equal 9292
  config.port = 80
end
```

## Establishing connection with Truemail API

After successful configuration, you can establish connection with Truemail server.

```crystal
Truemail::Client.validate("admin@bestweb.com.ua", configuration)

=>

{
  "date": "2020-05-05 19:30:42 +0200",
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
    "blacklisted_mx_ip_addresses": null,
    "dns": null,
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
