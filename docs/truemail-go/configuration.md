# Configuration

## Configurable options

- verifier email *(required)*
- verifier domain
- email pattern
- connection timeout
- response timeout
- connection attempts
- default validation type
- validation type for domains
- whitelisted domains
- whitelist validation
- blacklisted domains
- blacklisted mx ip-addresses
- custom DNS gateway(s)
- RFC MX lookup flow
- SMTP port number
- SMTP error body pattern
- SMTP fail fast
- SMTP safe check

## Creating configuration

To have an access for library features, you must create configuration struct first. Please use `truemail.NewConfiguration()` built-in constructor to create a valid configuration as in the example below:

```go
import "github.com/truemail-rb/truemail-go"

configuration := truemail.NewConfiguration(
  ConfigurationAttr{
    // Required parameter. Must be an existing email on behalf of which verification will be
    // performed
    VerifierEmail: "verifier@example.com",

    // Optional parameter. Must be an existing domain on behalf of which verification will be
    // performed. By default verifier domain based on verifier email
    VerifierDomain: "somedomain.com",

    // Optional parameter. You can override default regex pattern
    EmailPattern: `\A.+@(.+)\z`,

    // Optional parameter. You can override default regex pattern
    SmtpErrorBodyPattern: `.*(user|account).*`,

    // Optional parameter. Connection timeout in seconds.
    // It is equal to 2 by default.
    ConnectionTimeout: 1,

    // Optional parameter. A SMTP server response timeout in seconds.
    // It is equal to 2 by default.
    ResponseTimeout: 1,

    // Optional parameter. Total of connection attempts. It is equal to 2 by default.
    // This parameter uses in mx lookup timeout error and smtp request (for cases when
    // there is one mx server).
    ConnectionAttempts: 1,

    // Optional parameter. You can predefine default validation type for
    // truemail.Validate("email@email.com", configuration) call without type-parameter
    // Available validation types: "regex", "mx", "mx_blacklist", "smtp"
    ValidationTypeDefault: "mx",

    // Optional parameter. You can predefine which type of validation will be used for domains.
    // Also you can skip validation by domain.
    // Available validation types: "regex", "mx", "smtp"
    // This configuration will be used over current or default validation type parameter
    // All of validations for "somedomain.com" will be processed with regex validation only.
    // And all of validations for "otherdomain.com" will be processed with mx validation only.
    // It is equal to empty map of strings by default.
    ValidationTypeByDomain: map[string]string{"somedomain.com": "regex", "otherdomain.com": "mx"},

    // Optional parameter. Validation of email which contains whitelisted domain always will
    // return true. Other validations will not processed even if it was defined in
    // validationTypeByDomain. It is equal to empty slice of strings by default.
    WhitelistedDomains: []string{"somedomain1.com", "somedomain2.com"},

    // Optional parameter. With this option Truemail will validate email which contains whitelisted
    // domain only, i.e. if domain whitelisted, validation will passed to Regex, MX or SMTP
    // validators. Validation of email which not contains whitelisted domain always will return
    // false. It is equal false by default.
    WhitelistValidation: true,

    // Optional parameter. Validation of email which contains blacklisted domain always will
    // return false. Other validations will not processed even if it was defined in
    // validationTypeByDomain. It is equal to empty slice of strings by default.
    BlacklistedDomains: []string{"somedomain3.com", "somedomain4.com"},

    // Optional parameter. With this option Truemail will filter out unwanted mx servers via
    // predefined list of ip addresses. It can be used as a part of DEA (disposable email
    // address) validations. It is equal to empty slice of strings by default.
    BlacklistedMxIpAddresses: []string{"1.1.1.1", "2.2.2.2"},

    // Optional parameter. This option will provide to use custom DNS gateway when Truemail
    // interacts with DNS. Valid port number is in the range 1-65535. If you won't specify
    // nameserver port Truemail will use default DNS TCP/UDP port 53. It means that you can
    // use ip4 addres as DNS gateway, for example "10.0.0.1". By default Truemail uses
    // DNS gateway from system settings and this option is equal to empty string.
    Dns: "10.0.0.1:5300",

    // Optional parameter. This option will provide to use not RFC MX lookup flow.
    // It means that MX and Null MX records will be cheked on the DNS validation layer only.
    // By default this option is disabled and equal to false.
    NotRfcMxLookupFlow: true,

    // Optional parameter. SMTP port number. It is equal to 25 by default.
    // This parameter uses for SMTP session in SMTP validation layer.
    SmtpPort: 2525,

    // Optional parameter. This option will provide to use smtp fail fast behaviour. When
    // smtpFailFast = true it means that Truemail ends smtp validation session after first
    // attempt on the first mx server in any fail cases (network connection/timeout error,
    // smtp validation error). This feature helps to reduce total time of SMTP validation
    // session up to 1 second. By default this option is disabled and equal to false.
    SmtpFailFast: true,

    // Optional parameter. This option will be parse bodies of SMTP errors. It will be helpful
    // if SMTP server does not return an exact answer that the email does not exist
    // By default this option is disabled, available for SMTP validation only.
    SmtpSafeCheck: true,
  },
)
```

## Using configuration

```go
import "github.com/truemail-rb/truemail-go"

configuration := truemail.NewConfiguration(truemail.ConfigurationAttr{VerifierEmail: "verifier@example.com"})

truemail.Validate("some@email.com", configuration)
truemail.IsValid("some@email.com", configuration, "regex")
```
