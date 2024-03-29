# Endpoints

## Validation/verification email

```
GET /
Required URI params: email
Public: false
```

### 200: Existent email

```bash
curl -i -H "Accept: application/json" -H "Content-Type: application/json" -H "Authorization: a262d915-15bc-417c-afeb-842c63b54154" http://localhost:9292?email=admin@bestweb.com.ua
```

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 410
Connection: keep-alive
Server: thin
```

```json
{
  "date": "2021-02-07 10:00:56 +0200",
  "email": "admin@bestweb.com.ua",
  "validation_type": "smtp",
  "success": true,
  "errors": null,
  "smtp_debug": null,
  "configuration": {
    "validation_type_by_domain": null,
    "whitelisted_emails": null,
    "blacklisted_emails": null,
    "whitelisted_domains": null,
    "blacklisted_domains": null,
    "whitelist_validation": false,
    "blacklisted_mx_ip_addresses": null,
    "dns": null,
    "email_pattern": "default gem value",
    "not_rfc_mx_lookup_flow": false,
    "smtp_error_body_pattern": "default gem value",
    "smtp_fail_fast": false,
    "smtp_safe_check": false
  }
}
```

### 200: Nonexistent email

```bash
curl -i -H "Accept: application/json" -H "Content-Type: application/json" -H "Authorization: a262d915-15bc-417c-afeb-842c63b54154" http://localhost:9292?email=ololo@bestweb.com.ua
```

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 541
Connection: keep-alive
Server: thin
```

```json
{
  "date": "2021-02-07 10:10:42 +0200",
  "email": "ololo@bestweb.com.ua",
  "validation_type": "smtp",
  "success": false,
  "errors": {
    "smtp": "smtp error"
  },
  "smtp_debug": [
    {
      "connection": true,
      "errors": {
        "rcptto": "550 5.7.1 No such user!\n"
      },
      "mail_host": "213.180.204.89",
      "port_opened": true
    }
  ],
  "configuration": {
    "validation_type_by_domain": null,
    "whitelisted_emails": null,
    "blacklisted_emails": null,
    "whitelisted_domains": null,
    "blacklisted_domains": null,
    "whitelist_validation": false,
    "blacklisted_mx_ip_addresses": null,
    "dns": null,
    "email_pattern": "default gem value",
    "not_rfc_mx_lookup_flow": false,
    "smtp_error_body_pattern": "default gem value",
    "smtp_fail_fast": false,
    "smtp_safe_check": false
  }
}
```

### 422: Error

```bash
curl -i -H "Accept: application/json" -H "Content-Type: application/json" -H "Authorization: a262d915-15bc-417c-afeb-842c63b54154" http://localhost:9292
```

```
HTTP/1.1 422 Unprocessable Entity
Connection: close
Content-Type: application/json
Server: thin
```

```json
{
  "error": "email is required parameter"
}
```

## Version

```
GET /version
Public: false
```

### 200: OK

```bash
curl -i -H "Accept: application/json" -H "Content-Type: application/json" -H "Authorization: a262d915-15bc-417c-afeb-842c63b54154" http://localhost:9292/version
```

```
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 108
Content-Type: application/json
Server: thin

{
  "core": "3.3.0",
  "platform": "ruby 3.3.0 (2023-12-25 revision 5124f9ac75) [arm64-darwin22]",
  "version": "0.10.0"
}
```

## Healthcheck

```
GET /healthcheck
Public: true
```

### 200: OK

```bash
curl -i http://localhost:9292/healthcheck
```

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 0
Connection: keep-alive
Server: thin
```
