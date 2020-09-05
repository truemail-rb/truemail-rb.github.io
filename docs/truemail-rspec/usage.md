# Usage

## Create configuration instance

Allows to create configuration instance with random or with predefined params.

### Configuration instance with default random params

```ruby
create_configuration

# => returns Truemail::Configuration instance with
# random verifier_email and default Truemail::Configuration params
```

### Configuration instance with predefined params

<!-- TODO: Need to update link when docs will be completed -->

?> All `Truemail::Configuration` available params you can find [here](https://truemail-rb.org/truemail-gem/#/configuration?id=configurable-options)

```ruby
create_configuration(verifier_email: 'email@domain.com', verifier_domain: 'other-domain.com')
# => returns Truemail::Configuration instance with custom settings
```

## Create auditor instance

Allows to create auditor instance with default random or with predefined params.

### create_auditor

```ruby
create_auditor(
  success: true, # optional, type:Bool, by default true
  current_host_ip: current_host_ip, # optional, type:String, by default random IPv4 address
  warnings: warnings, # optional, type:Hash, by default creates auditor result warnings
  configuration: create_configuration # optional, type:Truemail::Configuration, by default creates random configuration
)

# => returns Truemail::Auditor instance follow passed params
```

## Create validator instance

Allows to create validator instance with default random or with predefined params.

### create_servers_list

```ruby
create_servers_list # => returns array with random ip addresses
```

### create_validator

```ruby
create_validator(
  validation_type, # optional, type:Symbol, can be :regex, :mx or :smtp, by default creates :smtp validation
  email, # optional, type:String, by default random email
  mail_servers, # optional, type:Array(String), by default array with random ip addresses
  success: true, # optional, type:Bool, by default true
  configuration: create_configuration # optional, type:Truemail::Configuration, by default creates random configuration
)

# => returns Truemail::Validator instance follow passed params
```
