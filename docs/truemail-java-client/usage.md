# Usage

First you have to import `TruemailClient`:

```java
import org.truemail_rb.TruemailClient;
import org.truemail_rb.client.TruemailConfiguration;
```

To have an access for `TruemailClient#validate` you must create configuration instance first as in the example below:

## Creating configuration instance

```java
// Required parameter. It should be true or false
boolean secureConnection = true;

// Required parameter. It should be a hostname or an ip address where Truemail server runs
String host = "example.com";

// Required parameter. It should be valid Truemail server access token
String token = "token";

// Optional parameter. By default it is equal 9292
int port = 80;

TruemailConfiguration truemailConfiguration = new TruemailConfiguration(secureConnection, host, token, port);

// or without port, will be used 9292 port by default
// TruemailConfiguration truemailConfiguration = new TruemailConfiguration(secureConnection, host, token);
```

## Establishing connection with Truemail API

After successful configuration, you can establish connection with Truemail server.

```java
TruemailClient truemailClient = new TruemailClient(truemailConfiguration);
truemailClient.validate("admin@bestweb.com.ua")

// =>

{
  "date": "2020-10-26 10:42:42 +0200",
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

`TruemailClient#validate` always returns JSON data. If something goes wrong you will receive JSON with error details:

```json
{
  "truemail_client_error": "error details"
}
```
