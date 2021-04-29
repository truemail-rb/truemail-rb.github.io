# JSON serializers

Truemail has built in JSON serializers for `Truemail::Auditor` and `Truemail::Validator` instances, so you can represent your host audition or email validation result as json.

> Please note, the preferred way is to use [#as_json](helpers?id=as_json) helper instead of calling serializer instance directly.

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
    "blacklisted_domains": null,
    "blacklisted_mx_ip_addresses": null,
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
    "blacklisted_domains": null,
    "blacklisted_mx_ip_addresses": null,
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
