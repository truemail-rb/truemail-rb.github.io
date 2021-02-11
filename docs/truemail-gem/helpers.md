# Helpers

## .valid?

You can use the `.valid?` helper for quick validation of email address. It returns a boolean:

```ruby
Truemail.valid?('email@example.com')
=> true
```

It is shortcut for `Truemail.validate('email@example.com').result.valid?`

## #as_json

You can use `#as_json` helper for represent `Truemail::Auditor` or `Truemail::Validator` instances as json. Under the hood it uses internal json `Truemail::Log::Serializer::AuditorJson` and `Truemail::Log::Serializer::ValidatorJson` [serializers](json-serializers):

### Auditor example

```ruby
Truemail.host_audit.as_json
```

Returns serialized `Truemail::Auditor` instance as JSON-object:

```json
{
  "date": "2020-08-31 22:33:43 +0300",
  "current_host_ip": "127.0.0.1",
  "warnings": {
    "dns": "A-record of verifier domain not refers to current host ip address",
    "ptr": "PTR-record does not reference to current verifier domain"
  },
 "configuration": {
    "blacklisted_domains": null,
    "dns": null,
    "email_pattern": "default gem value",
    "not_rfc_mx_lookup_flow": false,
    "smtp_error_body_pattern": "default gem value",
    "smtp_fail_fast": false,
    "smtp_safe_check": false,
    "validation_type_by_domain": null,
    "whitelist_validation": false,
    "whitelisted_domains": null
  }
}
```

### Validator example

```ruby
Truemail.validate('nonexistent_email@bestweb.com.ua').as_json
```

Returns serialized `Truemail::Validator` instance as JSON-object:

```json
{
  "date": "2020-05-10 10:00:00 +0200",
  "email": "nonexistent_email@bestweb.com.ua",
  "validation_type": "smtp",
  "success": false,
  "errors": {
    "smtp": "smtp error"
  },
  "smtp_debug": [
    {
      "mail_host": "213.180.193.89",
      "port_opened": true,
      "connection": true,
      "errors": {
        "rcptto": "550 5.7.1 No such user!\n"
      }
    }
  ],
  "configuration": {
    "blacklisted_domains": null,
    "dns": null,
    "email_pattern": "default gem value",
    "not_rfc_mx_lookup_flow": false,
    "smtp_error_body_pattern": "default gem value",
    "smtp_fail_fast": false,
    "smtp_safe_check": false,
    "validation_type_by_domain": null,
    "whitelist_validation": false,
    "whitelisted_domains": null
  }
}
```
