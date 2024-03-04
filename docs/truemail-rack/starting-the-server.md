# Starting the server

Before run application you must configure it through env vars first.

## Configurable options

List of available env vars names and values:

| Var name | Value example | Required | Description |
| --- | :-: | :-: | --- |
| `VERIFIER_EMAIL` | `your_email@example.com` | ***+*** | Must be an existing email on behalf of which verification will be performed. |
| `ACCESS_TOKENS` | `some_token` | ***+*** | These tokens will be used for token-based authentication. Accepts one ore more values separated by commas. |
| `VERIFIER_DOMAIN` | `somedomain.com` | - | Must be an existing domain on behalf of which verification will be performed. By default verifier domain based on verifier email. |
| `EMAIL_PATTERN` | `/regex_pattern/` | - | You can override [default Truemail regex pattern](https://github.com/truemail-rb/truemail/blob/dee39d5aeda5ffee4c268f0847ab4167c2a13f09/lib/truemail/core.rb#L35). |
| `SMTP_ERROR_BODY_PATTERN` | `/regex_pattern/` | - | You can override [default Truemail regex pattern](https://github.com/truemail-rb/truemail/blob/dee39d5aeda5ffee4c268f0847ab4167c2a13f09/lib/truemail/core.rb#L38). |
| `CONNECTION_TIMEOUT` | `1` | - | Connection timeout is equal to `2` ms by default. |
| `RESPONSE_TIMEOUT` | `1` | - | A SMTP server response timeout is equal to `2` ms by default. |
| `CONNECTION_ATTEMPTS` | `3` | - | Total of connection attempts. It is equal to `2` by default. This parameter uses in mx lookup timeout error and smtp request (for cases when there is one mx server). |
| `DEFAULT_VALIDATION_TYPE` | `mx` | - |  You can predefine Truemail default validation type (`smtp`). Available validation types: [`regex`](https://truemail-rb.org/truemail-gem/#/validations-layers?id=regex-validation), [`mx`](https://truemail-rb.org/truemail-gem/#/validations-layers?id=mx-validation), [`mx_blacklist`](https://truemail-rb.org/truemail-gem/#/validations-layers?id=mx-blacklist-validation), [`smtp`](https://truemail-rb.org/truemail-gem/#/validations-layers?id=smtp-validation). |
| `VALIDATION_TYPE_FOR` | `somedomain.com:regex` | - | You can predefine which type of validation will be used for domains. Available validation types: `regex`, `mx`, `mx_blacklist`, `smtp`. This configuration will be used over `DEFAULT_VALIDATION_TYPE`. All of validations for `somedomain.com` will be processed with `regex` validation only. Accepts one or more key:value separated by commas. |
| `WHITELISTED_EMAILS` | `user@somedomain1.com` | - | Validation of email which [contains whitelisted email](https://truemail-rb.org/truemail-gem/#/validations-layers?id=whitelist-case) always will return `true`. Other validations will not processed even if it was defined in `VALIDATION_TYPE_FOR`. Accepts one ore more values separated by commas. |
| `BLACKLISTED_EMAILS` | `user@somedomain2.com` | - | Validation of email which [contains blacklisted email](https://truemail-rb.org/truemail-gem/#/validations-layers?id=blacklist-case) always will return `false`. Other validations will not processed even if it was defined in `VALIDATION_TYPE_FOR`. Accepts one ore more values separated by commas. |
| `WHITELISTED_DOMAINS` | `somedomain1.com` | - | Validation of email which [contains whitelisted domain](https://truemail-rb.org/truemail-gem/#/validations-layers?id=whitelist-case) always will return `true`. Other validations will not processed even if it was defined in `VALIDATION_TYPE_FOR`. Accepts one ore more values separated by commas. |
| `BLACKLISTED_DOMAINS` | `somedomain2.com` | - | Validation of email which [contains blacklisted domain](https://truemail-rb.org/truemail-gem/#/validations-layers?id=blacklist-case) always will return `false`. Other validations will not processed even if it was defined in `VALIDATION_TYPE_FOR`. Accepts one ore more values separated by commas. |
| `WHITELIST_VALIDATION` | `true` | - | With this option Truemail will validate email which [contains whitelisted domain only](https://truemail-rb.org/truemail-gem/#/validations-layers?id=whitelist-validation-case), i.e. if domain whitelisted, validation will passed to Regex, MX or SMTP validators. Validation of email which not contains whitelisted domain always will return `false`. It is equal `false` by default. |
| `BLACKLISTED_MX_IP_ADDRESSES` | `127.0.1.1,127.0.1.2` | - | With this option Truemail will filter out unwanted mx servers via predefined list of ip addresses. It can be used as a part of DEA (disposable email address) validations. Accepts one ore more values separated by commas. |
| `DNS` | `8.8.8.8,8.8.4.4:53` | - | This option will provide to use custom DNS gateway when Truemail interacts with DNS. If you won't specify nameserver's ports Truemail will use default DNS TCP/UDP port 53. Accepts one ore more values separated by commas. |
| `NOT_RFC_MX_LOOKUP_FLOW` | `true` | - | This option will provide to use not RFC MX lookup flow. It means that MX and Null MX records will be cheked on the DNS validation layer only. By default [this option is disabled](https://truemail-rb.org/truemail-gem/#/validations-layers?id=not-rfc-mx-lookup-flow). |
| `SMTP_PORT` | `2525` | - | SMTP port number. It is equal to `25` by default. |
| `SMTP_FAIL_FAST` | `true` | - | This option will provide to use SMTP fail fast behaviour. When [smtp_fail_fast is enabled](https://truemail-rb.org/truemail-gem/#/validations-layers?id=smtp-fail-fast-enabled) it means that Truemail ends smtp validation session after first attempt on the first mx server in any fail cases (network connection/timeout error, smtp validation error). By default this option is disabled, available for SMTP validation only. |
| `SMTP_SAFE_CHECK` | `true` | - | This option will be parse bodies of SMTP errors. It will be helpful if SMTP server does not return an exact answer that the email does not exist. By default [this option is disabled](https://truemail-rb.org/truemail-gem/#/validations-layers?id=smtp-safe-check-disabled), available for SMTP validation only. |
| `LOG_STDOUT` | `true` | - | This option will be enable log all http requests to stdout. By default this option is disabled. |

## Configure and run

### Simple configuration example

```bash
VERIFIER_EMAIL=your_email@example.com ACCESS_TOKENS=a262d915-15bc-417c-afeb-842c63b54154 rackup
```

```
# =>
# Thin web server (v1.8.1 codename Infinite Smoothie)
# Maximum connections set to 1024
# Listening on localhost:9292, CTRL+C to stop
```

### Advanced configuration example

```bash
VERIFIER_EMAIL=your_email@example.com \
ACCESS_TOKENS=a262d915-15bc-417c-afeb-842c63b54154,f44cd67e-aaa0-4e6c-aa6c-d52cf61f84ac \
EMAIL_PATTERN="/\A.+@.+\z/" \
SMTP_ERROR_BODY_PATTERN="/(?=.*550)(?=.*(user|account|customer|mailbox|something_else)).*/" \
VALIDATION_TYPE_FOR=somedomain1.com:regex,somedomain2.com:mx \
WHITELISTED_EMAILS=whitelisted@example.com \
BLACKLISTED_EMAILS=blacklisted1@example.com,blacklisted2@example.com \
WHITELISTED_DOMAINS=somedomain3.com \
BLACKLISTED_DOMAINS=somedomain4.com,somedomain5.com \
WHITELIST_VALIDATION=true \
BLACKLISTED_MX_IP_ADDRESSES=127.0.1.1,127.0.1.2 \
DNS=8.8.8.8,8.8.4.4:53 \
NOT_RFC_MX_LOOKUP_FLOW=true \
SMTP_PORT=2525 \
SMTP_FAIL_FAST=true \
SMTP_SAFE_CHECK=true \
LOG_STDOUT=true \
thin -R config.ru -a 0.0.0.0 -p 9292 -e production start
```

```
# =>
# Thin web server (v1.8.1 codename Infinite Smoothie)
# Maximum connections set to 1024
# Listening on localhost:9292, CTRL+C to stop
# 127.0.0.1 - - [26/Feb/2020:16:41:13 +0200] "GET /?email=admin%40bestweb.com.ua HTTP/1.1" 200 - 0.9195
```
