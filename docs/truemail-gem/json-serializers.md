# JSON serializers

Truemail has built in JSON serializers for `Truemail::Auditor` and `Truemail::Validator` instances, so you can represent your host audition or email validation result as json. Also you can use [#as_json](truemail-helpers?id=as_json) helper for shortcuting.

## Auditor JSON serializer

```ruby
Truemail::Log::Serializer::AuditorJson.call(Truemail.host_audit)
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
    "validation_type_by_domain": null,
    "whitelist_validation": false,
    "whitelisted_domains": null,
    "blacklisted_domains": null,
    "not_rfc_mx_lookup_flow": false,
    "smtp_safe_check": false,
    "email_pattern": "default gem value",
    "smtp_error_body_pattern": "default gem value"
  }
}
```

## Validator JSON serializer

```ruby
Truemail::Log::Serializer::ValidatorJson.call(Truemail.validate('nonexistent_email@bestweb.com.ua'))
```

Returns serialized `Truemail::Validator` instance as JSON-object:

```json
{
  "date": "2019-10-28 10:15:51 +0200",
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
    "validation_type_by_domain": null,
    "whitelist_validation": false,
    "whitelisted_domains": null,
    "blacklisted_domains": null,
    "not_rfc_mx_lookup_flow": false,
    "smtp_safe_check": false,
    "email_pattern": "default gem value",
    "smtp_error_body_pattern": "default gem value"
  }
}
```
