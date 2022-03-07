# Quick Start

## Requirements

!> Golang 1.15+

## Verifier host preconditions

?> You should follow these simple rules for the best email SMTP validation outcome.

1. [Verifier email](terminology?id=verifier-email) should be existing
2. [Verifier domain](terminology?id=verifier-domain) should be existing and the same as in verifier email)
3. Verifier domain should have [MX records](terminology?id=mx-record)
4. Verifier domain should refer to [verefier host](terminology?id=verifier-host) IP address
5. Verefier host should have [PTR record](terminology?id=ptr-record)
6. Verefier host and verifier email shouldn't be in email blacklists databases

## Installation

Install `truemail-go`:

```bash
go get github.com/truemail-rb/truemail-go
go install -i github.com/truemail-rb/truemail-go
```

Import `truemail-go` dependency into your code:

```go
package main

import "github.com/truemail-rb/truemail-go"
```

## DSL example


```go
import "github.com/truemail-rb/truemail-go"

configuration := truemail.NewConfiguration(truemail.ConfigurationAttr{VerifierEmail: "verifier@example.com"})

truemail.Validate("some@email.com", configuration) // returns pointer to validatorResult with validation details and error
truemail.IsValid("some@email.com", configuration) // returns boolean
```

It's very generalized example, more details and real usecase examples of Truemail usage you can find in next sections.
