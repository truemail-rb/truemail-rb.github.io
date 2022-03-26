# Validation's layers

Truemail is a multi-layered email validator/verifier with configurable behavior for specified domain names/mx server ip addresses. So you can validate only what you need.

```mermaid
flowchart LR
A(Whitelist/Blacklist) --> B(Regex validation)
B(Regex validation) --> C(MX validation)
C(MX validation) --> D(MX blacklist validation)
D(MX blacklist validation) --> E(SMTP validation)
```

## Whitelist/Blacklist check

Whitelist/Blacklist check is zero validation level. You can define white and black list domains. It means that validation of email which contains whitelisted domain always will return `true`, and for blacklisted domain will return `false`.

Please note, other validations will not processed even if it was defined in `validation_type_for`.

**Sequence of domain list check:**

1. Whitelist check
2. Whitelist validation check
3. Blacklist check

Example of usage:

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
  config.whitelisted_domains = ['white-domain.com', 'somedomain.com']
  config.blacklisted_domains = ['black-domain.com', 'somedomain.com']
  config.validation_type_for = { 'somedomain.com' => :mx }
end
```

### Whitelist case

When email in whitelist, validation type will be redefined. Validation result returns `true`

```ruby
Truemail.validate('email@white-domain.com')

#<Truemail::Validator:0x000055b8429f3490
  @result=#<struct Truemail::Validator::Result
    success=true,
    email="email@white-domain.com",
    domain="white-domain.com",
    mail_servers=[],
    errors={},
    smtp_debug=nil>,
    configuration=#<Truemail::Configuration:0x00005629f801bd28
     @blacklisted_domains=["black-domain.com", "somedomain.com"],
     @blacklisted_mx_ip_addresses=[],
     @dns=[],
     @connection_attempts=2,
     @connection_timeout=2,
     @default_validation_type=:smtp,
     @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
     @response_timeout=2,
     @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
     @not_rfc_mx_lookup_flow=false,
     @smtp_port=25,
     @smtp_fail_fast=false,
     @smtp_safe_check=false,
     @validation_type_by_domain={"somedomain.com"=>:mx},
     @verifier_domain="example.com",
     @verifier_email="verifier@example.com",
     @whitelist_validation=false,
     @whitelisted_domains=["white-domain.com", "somedomain.com"]>,
  @validation_type=:whitelist>
```

### Whitelist validation case

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
  config.whitelisted_domains = ['white-domain.com']
  config.whitelist_validation = true
end
```

When email domain in whitelist and `whitelist_validation` is sets equal to `true` validation type will be passed to other validators. Validation of email which not contains whitelisted domain always will return `false`.

#### Email has whitelisted domain

```ruby
Truemail.validate('email@white-domain.com', with: :regex)

#<Truemail::Validator:0x000055b8429f3490
  @result=#<struct Truemail::Validator::Result
    success=true,
    email="email@white-domain.com",
    domain="white-domain.com",
    mail_servers=[],
    errors={},
    smtp_debug=nil>,
    configuration=
    #<Truemail::Configuration:0x0000563f0d2605c8
     @blacklisted_domains=[],
     @blacklisted_mx_ip_addresses=[],
     @dns=[],
     @connection_attempts=2,
     @connection_timeout=2,
     @default_validation_type=:smtp,
     @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
     @response_timeout=2,
     @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
     @not_rfc_mx_lookup_flow=false,
     @smtp_port=25,
     @smtp_fail_fast=false,
     @smtp_safe_check=false,
     @validation_type_by_domain={},
     @verifier_domain="example.com",
     @verifier_email="verifier@example.com",
     @whitelist_validation=true,
     @whitelisted_domains=["white-domain.com"]>,
  @validation_type=:regex>
```

#### Email hasn't whitelisted domain

```ruby
Truemail.validate('email@domain.com', with: :regex)

#<Truemail::Validator:0x000055b8429f3490
  @result=#<struct Truemail::Validator::Result
    success=false,
    email="email@domain.com",
    domain="domain.com",
    mail_servers=[],
    errors={},
    smtp_debug=nil>,
    configuration=
    #<Truemail::Configuration:0x0000563f0cd82ab0
     @blacklisted_domains=[],
     @blacklisted_mx_ip_addresses=[],
     @dns=[],
     @connection_attempts=2,
     @connection_timeout=2,
     @default_validation_type=:smtp,
     @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
     @response_timeout=2,
     @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
     @not_rfc_mx_lookup_flow=false,
     @smtp_port=25,
     @smtp_fail_fast=false,
     @smtp_safe_check=false,
     @validation_type_by_domain={},
     @verifier_domain="example.com",
     @verifier_email="verifier@example.com",
     @whitelist_validation=true,
     @whitelisted_domains=["white-domain.com"]>,
  @validation_type=:blacklist>
```

### Blacklist case

When email in blacklist, validation type will be redefined too. Validation result returns `false`.

```ruby
Truemail.validate('email@black-domain.com')

#<Truemail::Validator:0x000023y8429f3493
  @result=#<struct Truemail::Validator::Result
    success=false,
    email="email@black-domain.com",
    domain="black-domain.com",
    mail_servers=[],
    errors={},
    smtp_debug=nil>,
    configuration=
    #<Truemail::Configuration:0x0000563f0d36f4f0
     @blacklisted_domains=[],
     @blacklisted_mx_ip_addresses=[],
     @dns=[],
     @connection_attempts=2,
     @connection_timeout=2,
     @default_validation_type=:smtp,
     @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
     @response_timeout=2,
     @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
     @not_rfc_mx_lookup_flow=false,
     @smtp_port=25,
     @smtp_fail_fast=false,
     @smtp_safe_check=false,
     @validation_type_by_domain={},
     @verifier_domain="example.com",
     @verifier_email="verifier@example.com",
     @whitelist_validation=true,
     @whitelisted_domains=["white-domain.com"]>,
  @validation_type=:blacklist>
```

### Duplication case

Validation result for this email returns `true`, because it was found in whitelisted domains list first. Also `validation_type` for this case will be redefined.

```ruby
Truemail.validate('email@somedomain.com')

#<Truemail::Validator:0x000055b8429f3490
  @result=#<struct Truemail::Validator::Result
    success=true,
    email="email@somedomain.com",
    domain="somedomain.com",
    mail_servers=[],
    errors={},
    smtp_debug=nil>,
    configuration=
    #<Truemail::Configuration:0x0000563f0d3f8fc0
     @blacklisted_domains=[],
     @blacklisted_mx_ip_addresses=[],
     @dns=[],
     @connection_attempts=2,
     @connection_timeout=2,
     @default_validation_type=:smtp,
     @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
     @response_timeout=2,
     @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
     @not_rfc_mx_lookup_flow=false,
     @smtp_port=25,
     @smtp_fail_fast=false,
     @smtp_safe_check=false,
     @validation_type_by_domain={},
     @verifier_domain="example.com",
     @verifier_email="verifier@example.com",
     @whitelist_validation=true,
     @whitelisted_domains=["white-domain.com"]>,
  @validation_type=:whitelist>
```

## Regex validation

Validation with regex pattern is the first validation level. It uses whitelist/blacklist check before running itself.

> [[Whitelist/Blacklist]](validations-layers?id=whitelistblacklist-check) -> [Regex validation]

By default this validation not performs strictly following [RFC 5322](https://www.ietf.org/rfc/rfc5322.txt) standard, so you can override Truemail default regex pattern if you want. Example of usage:

### With default regex pattern

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
end

Truemail.validate('email@example.com', with: :regex)

=> #<Truemail::Validator:0x000055590cc9bdb8
  @result=
    #<struct Truemail::Validator::Result
      success=true,
      email="email@example.com",
      domain="example.com",
      mail_servers=[],
      errors={},
      smtp_debug=nil>,
      configuration=
      #<Truemail::Configuration:0x000055aa56a54d48
       @blacklisted_domains=[],
       @blacklisted_mx_ip_addresses=[],
       @dns=[],
       @connection_attempts=2,
       @connection_timeout=2,
       @default_validation_type=:smtp,
       @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
       @response_timeout=2,
       @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
       @not_rfc_mx_lookup_flow=false,
       @smtp_port=25,
       @smtp_fail_fast=false,
       @smtp_safe_check=false,
       @validation_type_by_domain={},
       @verifier_domain="example.com",
       @verifier_email="verifier@example.com",
       @whitelist_validation=false,
       @whitelisted_domains=[]>,
  @validation_type=:regex>
```

### With custom regex pattern

You should define your custom regex pattern in a [gem configuration](configuration?id=setting-configuration) before.

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
  config.email_pattern = /regex_pattern/
end

Truemail.validate('email@example.com', with: :regex)

=> #<Truemail::Validator:0x000055590ca8b3e8
  @result=
    #<struct Truemail::Validator::Result
      success=true,
      email="email@example.com",
      domain="example.com",
      mail_servers=[],
      errors={},
      smtp_debug=nil>,
      configuration=
      #<Truemail::Configuration:0x0000560e58d80830
       @blacklisted_domains=[],
       @blacklisted_mx_ip_addresses=[],
       @dns=[],
       @connection_attempts=2,
       @connection_timeout=2,
       @default_validation_type=:smtp,
       @email_pattern=/regex_pattern/,
       @response_timeout=2,
       @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
       @not_rfc_mx_lookup_flow=false,
       @smtp_port=25,
       @smtp_fail_fast=false,
       @smtp_safe_check=false,
       @validation_type_by_domain={},
       @verifier_domain="example.com",
       @verifier_email="verifier@example.com",
       @whitelist_validation=false,
       @whitelisted_domains=[]>,
  @validation_type=:regex>
```

## MX validation

In fact it's DNS validation because it checks not MX records only. DNS validation is the second validation level, historically named as MX validation. It uses [Regex validation](validations-layers?id=regex-validation) before running itself. When regex validation has completed successfully then runs itself.

> [[Whitelist/Blacklist]](validations-layers?id=whitelistblacklist-check) -> [[Regex validation]](validations-layers?id=regex-validation) -> [MX validation]

Please note, Truemail MX validator [not performs](https://github.com/truemail-rb/truemail/issues/26) strict compliance of the [RFC 5321](https://tools.ietf.org/html/rfc5321#section-5) standard for the best validation outcome.

```mermaid
flowchart TD
  Y[Domain name from email address] --> A(MX records resolver)
  A(MX records resolver) -->|False| B(CNAME records resolver)
  B(CNAME records resolver) -->|False| D(A records resolver)
  D(A records resolver) -->|False| E[Validation fails]
  A(MX records resolver) -->|not RFC MX lookup flow| E[Validation fails]
  A(MX records resolver) -->|True| C[Validation successful]
  B(CNAME records resolver) -->|True| C[Validation successful]
  D(A records resolver) -->|True| C[Validation successful]
  
  style A stroke:blue;
  style B stroke:blue;
  style D stroke:blue;
  style C color:green;
  style E color:red;
```

### RFC MX lookup flow

[Truemail MX lookup](https://slides.com/vladislavtrotsenko/truemail#/0/9) based on RFC 5321. It consists of 3 substeps: `MX`, `CNAME` and `A` record resolvers. The point of each resolver is attempt to extract the mail servers from email domain. If at least one server exists that validation is successful. Iteration is processing until resolver returns `true`. Example of usage:

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
end

Truemail.validate('email@example.com', with: :mx)

=> #<Truemail::Validator:0x000055590c9c1c50
  @result=
    #<struct Truemail::Validator::Result
      success=true,
      email="email@example.com",
      domain="example.com",
      mail_servers=["127.0.1.1", "127.0.1.2"],
      errors={},
      smtp_debug=nil>,
      configuration=
      #<Truemail::Configuration:0x0000559b6e44af70
       @blacklisted_domains=[],
       @blacklisted_mx_ip_addresses=[],
       @dns=[],
       @connection_attempts=2,
       @connection_timeout=2,
       @default_validation_type=:smtp,
       @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
       @response_timeout=2,
       @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
       @not_rfc_mx_lookup_flow=false,
       @smtp_port=25,
       @smtp_fail_fast=false,
       @smtp_safe_check=false,
       @validation_type_by_domain={},
       @verifier_domain="example.com",
       @verifier_email="verifier@example.com",
       @whitelist_validation=false,
       @whitelisted_domains=[]>,
  @validation_type=:mx>
```

### Not RFC MX lookup flow

Also Truemail has possibility to use not RFC MX lookup flow. It means that will be used only one MX resolver on the DNS validation layer. By default this option is disabled. Example of usage:

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
  config.not_rfc_mx_lookup_flow = true
end

Truemail.validate('email@example.com', with: :mx)

=> #<Truemail::Validator:0x000055590c9c1c50
  @result=
    #<struct Truemail::Validator::Result
      success=true,
      email="email@example.com",
      domain="example.com",
      mail_servers=["127.0.1.1", "127.0.1.2"],
      errors={},
      smtp_debug=nil>,
      configuration=
      #<Truemail::Configuration:0x0000559b6e44af70
       @blacklisted_domains=[],
       @blacklisted_mx_ip_addresses=[],
       @dns=[],
       @connection_attempts=2,
       @connection_timeout=2,
       @default_validation_type=:smtp,
       @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
       @response_timeout=2,
       @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
       @not_rfc_mx_lookup_flow=true,
       @smtp_port=25,
       @smtp_fail_fast=false,
       @smtp_safe_check=false,
       @validation_type_by_domain={},
       @verifier_domain="example.com",
       @verifier_email="verifier@example.com",
       @whitelist_validation=false,
       @whitelisted_domains=[]>,
  @validation_type=:mx>
```

## MX blacklist validation

MX blacklist validation is the third validation level. This layer provides checking extracted mail server(s) IP address from MX validation with predefined blacklisted IP addresses list. It can be used as a part of DEA ([disposable email address](https://en.wikipedia.org/wiki/Disposable_email_address)) validations.

> [[Whitelist/Blacklist]](validations-layers?id=whitelistblacklist-check) -> [[Regex validation]](validations-layers?id=regex-validation) -> [[MX validation](validations-layers?id=mx-validation)] -> [MX blacklist validation]

Example of usage:

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
  config.blacklisted_mx_ip_addresses = ['127.0.1.2']
end

Truemail.validate('email@example.com', with: :mx_blacklist)

=> #<Truemail::Validator:0x00007fca0c8aea70
 @result=
  #<struct Truemail::Validator::Result
   success=false,
   email="email@example.com",
   domain="example.com",
   mail_servers=["127.0.1.1", "127.0.1.2"],
   errors={:mx_blacklist=>"blacklisted mx server ip address"},
   smtp_debug=nil,
   configuration=
    #<Truemail::Configuration:0x00007fca0c8aeb38
     @blacklisted_domains=[],
     @blacklisted_mx_ip_addresses=["127.0.1.2"],
     @connection_attempts=2,
     @connection_timeout=2,
     @default_validation_type=:smtp,
     @dns=[],
     @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-.+]*)@((?i-mx:[\p{L}0-9]+([\-.]{1}[\p{L}0-9]+)*\.\p{L}{2,63}))\z)/,
     @not_rfc_mx_lookup_flow=false,
     @smtp_port=25,
     @response_timeout=2,
     @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
     @smtp_fail_fast=false,
     @smtp_safe_check=false,
     @validation_type_by_domain={},
     @verifier_domain="example.com",
     @verifier_email="verifier@example.com",
     @whitelist_validation=false,
     @whitelisted_domains=[]>>,
 @validation_type=:mx_blacklist>
```

## SMTP validation

SMTP validation is a final, fourth validation level. This type of validation tries to check real existence of email account on a current email server. This validation runs a chain of previous validations and if they're complete successfully then runs itself.

> [[Whitelist/Blacklist]](validations-layers?id=whitelistblacklist-check) -> [[Regex validation]](validations-layers?id=regex-validation) -> [[MX validation]](validations-layers?id=mx-validation) -> [[MX blacklist validation]](validations-layers?id=mx-blacklist-validation) -> [SMTP validation]

If `smtp_fail_fast` feature is disabled or total count of MX servers is equal to one, `Truemail::Smtp` validator will use value from `Truemail.configuration.connection_attempts` as connection attempts. By default it's equal to `2`.

?> You should follow [verifier host preconditions rules](quick-start?id=verifier-host-preconditions) for the best email SMTP validation outcome if you going to use this validation layer. Also you can reduce time of SMTP validation by enabling SMTP fail fast behaviour.

```mermaid
flowchart TD  
  A[Mail server] --> B{Is the 25-port opened?}
  B{Is the 25-port opened?} -->|False| D{Next mail-server exists?}
  D{Next mail-server exists?} -->|False| Y[Validation fails]
  D{Next mail-server exists?} -->|True| A[Mail server]
  B{Is the 25-port opened?} -->|True| C(Run SMTP-session)
  C(Run SMTP-session) -->|False| D{Next mail-server exists?}
  C(Run SMTP-session) -->|True| Z[Validation successful]
  
  style Y color:red;
  style Z color:green;
```

By default, you don't need pass with-parameter to use it. Example of usage is specified below:

### SMTP fail fast enabled

Truemail can use fail fast behaviour for SMTP validation layer. When `smtp_fail_fast = true` it means that `truemail` ends smtp validation session after first attempt on the first mx server in any fail cases (network connection/timeout error, smtp validation error). This feature helps to reduce total time of SMTP validation session up to 1 second.

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
  config.smtp_fail_fast = true
end

Truemail.validate('email@example.com')

# SMTP validation failed, smtp fail fast validation scenario
=> #<Truemail::Validator:0x00007fdc4504f460
    @result=
      #<struct Truemail::Validator::Result
        success=false,
        email="email@example.com",
        domain="example.com",
        mail_servers=["127.0.1.1", "127.0.1.2", "127.0.1.3"], # there are 3 mail servers in a row
        errors={:smtp=>"smtp error"},
        smtp_debug=
          [#<Truemail::Validate::Smtp::Request:0x00007fdc43150b90 # but iteration has been stopped after the first failure
            @attempts=nil,
            @configuration=
              #<Truemail::Validate::Smtp::Request::Configuration:0x00007fdc43150b18
                @connection_timeout=2,
                @response_timeout=2,
                @verifier_domain="example.com",
                @verifier_email="verifier@example.com">,
            @email="email@example.com",
            @host="127.0.1.1",
            @response=
              #<struct Truemail::Validate::Smtp::Response
                port_opened=false,
                connection=nil,
                helo=nil,
                mailfrom=nil,
                rcptto=nil,
                errors={}>>],
        configuration=
          #<Truemail::Configuration:0x00007fdc4504f5c8
            @blacklisted_domains=[],
            @blacklisted_mx_ip_addresses=[],
            @dns=[],
            @connection_attempts=2,
            @connection_timeout=2,
            @default_validation_type=:smtp,
            @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-.+]*)@((?i-mx:[\p{L}0-9]+([\-.]{1}[\p{L}0-9]+)*\.\p{L}{2,63}))\z)/,
            @not_rfc_mx_lookup_flow=false,
            @smtp_port=25,
            @response_timeout=2,
            @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
            @smtp_fail_fast=true,
            @smtp_safe_check=false,
            @validation_type_by_domain={},
            @verifier_domain="example.com",
            @verifier_email="verifier@example.com",
            @whitelist_validation=false,
            @whitelisted_domains=[]>>,
      @validation_type=:smtp>
```

### SMTP safe check disabled

With `smtp_safe_check = false`

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
end

Truemail.validate('email@example.com')

# Successful SMTP validation
=> #<Truemail::Validator:0x000055590c4dc118
  @result=
    #<struct Truemail::Validator::Result
      success=true,
      email="email@example.com",
      domain="example.com",
      mail_servers=["127.0.1.1", "127.0.1.2"],
      errors={},
      smtp_debug=nil>,
      configuration=
      #<Truemail::Configuration:0x00005615e87b9298
       @blacklisted_domains=[],
       @blacklisted_mx_ip_addresses=[],
       @dns=[],
       @connection_attempts=2,
       @connection_timeout=2,
       @default_validation_type=:smtp,
       @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
       @response_timeout=2,
       @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
       @not_rfc_mx_lookup_flow=false,
       @smtp_port=25,
       @smtp_fail_fast=false,
       @smtp_safe_check=false,
       @validation_type_by_domain={},
       @verifier_domain="example.com",
       @verifier_email="verifier@example.com",
       @whitelist_validation=false,
       @whitelisted_domains=[]>,
  @validation_type=:smtp>

# SMTP validation failed
=> #<Truemail::Validator:0x0000000002d5cee0
    @result=
      #<struct Truemail::Validator::Result
        success=false,
        email="email@example.com",
        domain="example.com",
        mail_servers=["127.0.1.1", "127.0.1.2"],
        errors={:smtp=>"smtp error"},
        smtp_debug=
          [#<Truemail::Validate::Smtp::Request:0x0000000002d49b10
            @configuration=
              #<Truemail::Validate::Smtp::Request::Configuration:0x00005615e8d21848
              @connection_timeout=2,
              @response_timeout=2,
              @verifier_domain="example.com",
              @verifier_email="verifier@example.com">,
            @email="email@example.com",
            @host="127.0.1.1",
            @attempts=nil,
            @response=
              #<struct Truemail::Validate::Smtp::Response
                port_opened=true,
                connection=true,
                helo=true,
                mailfrom=
                  #<Net::SMTP::Response:0x0000000002d5a618
                    @status="250",
                    @string="250 OK\n">,
                rcptto=false,
                errors={:rcptto=>"550 User not found\n"}>>]>,
          configuration=
            #<Truemail::Configuration:0x00005615e87b9298
             @blacklisted_domains=[],
             @blacklisted_mx_ip_addresses=[],
             @dns=[],
             @connection_attempts=2,
             @connection_timeout=2,
             @default_validation_type=:smtp,
             @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
             @response_timeout=2,
             @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
             @not_rfc_mx_lookup_flow=false,
             @smtp_port=25,
             @smtp_fail_fast=false,
             @smtp_safe_check=false,
             @validation_type_by_domain={},
             @verifier_domain="example.com",
             @verifier_email="verifier@example.com",
             @whitelist_validation=false,
             @whitelisted_domains=[]>,
    @validation_type=:smtp>
```

### SMTP safe check enabled

With `smtp_safe_check = true`

```ruby
require 'truemail'

Truemail.configure do |config|
  config.verifier_email = 'verifier@example.com'
  config.smtp_safe_check = true
end

Truemail.validate('email@example.com')

# Successful SMTP validation
=> #<Truemail::Validator:0x0000000002ca2c70
    @result=
      #<struct Truemail::Validator::Result
        success=true,
        email="email@example.com",
        domain="example.com",
        mail_servers=["127.0.1.1", "127.0.1.2"],
        errors={},
        smtp_debug=
          [#<Truemail::Validate::Smtp::Request:0x0000000002c95d40
            @configuration=
              #<Truemail::Validate::Smtp::Request::Configuration:0x00005615e8d21848
              @connection_timeout=2,
              @response_timeout=2,
              @verifier_domain="example.com",
              @verifier_email="verifier@example.com">,
            @email="email@example.com",
            @host="127.0.1.1",
            @attempts=nil,
            @response=
              #<struct Truemail::Validate::Smtp::Response
                port_opened=true,
                connection=false,
                helo=true,
                mailfrom=false,
                rcptto=nil,
                errors={:mailfrom=>"554 5.7.1 Client host blocked\n", :connection=>"server dropped connection after response"}>>,]>,
        configuration=
            #<Truemail::Configuration:0x00005615e87b9298
             @blacklisted_domains=[],
             @blacklisted_mx_ip_addresses=[],
             @dns=[],
             @connection_attempts=2,
             @connection_timeout=2,
             @default_validation_type=:smtp,
             @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
             @response_timeout=2,
             @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
             @not_rfc_mx_lookup_flow=false,
             @smtp_port=25,
             @smtp_fail_fast=false,
             @smtp_safe_check=false,
             @validation_type_by_domain={},
             @verifier_domain="example.com",
             @verifier_email="verifier@example.com",
             @whitelist_validation=false,
             @whitelisted_domains=[]>,
    @validation_type=:smtp>

# SMTP validation failed
=> #<Truemail::Validator:0x0000000002d5cee0
   @result=
    #<struct Truemail::Validator::Result
      success=false,
      email="email@example.com",
      domain="example.com",
      mail_servers=["127.0.1.1", "127.0.1.2"],
      errors={:smtp=>"smtp error"},
      smtp_debug=
        [#<Truemail::Validate::Smtp::Request:0x0000000002d49b10
          @configuration=
              #<Truemail::Validate::Smtp::Request::Configuration:0x00005615e8d21848
              @connection_timeout=2,
              @response_timeout=2,
              @verifier_domain="example.com",
              @verifier_email="verifier@example.com">,
          @email="email@example.com",
          @host="127.0.1.1",
          @attempts=nil,
          @response=
            #<struct Truemail::Validate::Smtp::Response
              port_opened=true,
              connection=true,
              helo=true,
              mailfrom=#<Net::SMTP::Response:0x0000000002d5a618 @status="250", @string="250 OK\n">,
              rcptto=false,
              errors={:rcptto=>"550 User not found\n"}>>]>,
      configuration=
            #<Truemail::Configuration:0x00005615e87b9298
             @blacklisted_domains=[],
             @blacklisted_mx_ip_addresses=[],
             @dns=[],
             @connection_attempts=2,
             @connection_timeout=2,
             @default_validation_type=:smtp,
             @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
             @response_timeout=2,
             @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
             @not_rfc_mx_lookup_flow=false,
             @smtp_port=25,
             @smtp_fail_fast=false,
             @smtp_safe_check=false,
             @validation_type_by_domain={},
             @verifier_domain="example.com",
             @verifier_email="verifier@example.com",
             @whitelist_validation=false,
             @whitelisted_domains=[]>,
    @validation_type=:smtp>
```
