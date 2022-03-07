# ![Truemail - configurable Golang email validator](https://truemail-rb.org/assets/images/truemail_logo.png)

[![Go Report Card](https://goreportcard.com/badge/github.com/truemail-rb/truemail-go)](https://goreportcard.com/report/github.com/truemail-rb/truemail-go)
[![Codecov](https://codecov.io/gh/truemail-rb/truemail-go/branch/master/graph/badge.svg)](https://codecov.io/gh/truemail-rb/truemail-go)
[![CircleCI](https://circleci.com/gh/truemail-rb/truemail-go/tree/master.svg?style=svg)](https://circleci.com/gh/truemail-rb/truemail-go/tree/master)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/truemail-rb/truemail-go)](https://github.com/truemail-rb/truemail-go/releases)
[![PkgGoDev](https://pkg.go.dev/badge/github.com/truemail-rb/truemail-go)](https://pkg.go.dev/github.com/truemail-rb/truemail-go)
[![In Awesome Go](https://awesome.re/mentioned-badge.svg)](https://github.com/avelino/awesome-go)
[![GitHub](https://img.shields.io/github/license/truemail-rb/truemail-go)](https://github.com/truemail-rb/truemail-go/blob/master/LICENSE.txt)
[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-v1.4%20adopted-ff69b4.svg)](https://github.com/truemail-rb/truemail-go/blob/master/CODE_OF_CONDUCT.md)

> Configurable Golang email validator/verifier. Verify email via Regex, DNS, SMTP and even more. Be sure that email address valid and exists.

## Synopsis

Email validation is a tricky thing. There are a number of different ways to validate an email address and all mechanisms must conform with the best practices and provide proper validation. The `truemail-go` package helps you to validate emails via regex pattern (email syntax checking), presence of DNS records (mail-server existence checking), and real existence of email account on a current email server.

## Features

1. Configurable layer-designed validator, validate only what you need
2. Zero runtime dependency
3. Supporting of internationalized emails ([EAI](https://en.wikipedia.org/wiki/Email_address#Internationalization))
4. Whitelist/blacklist validation layers
5. Ability to configure different MX/SMTP validation flows
6. Ability to configure [DEA](https://en.wikipedia.org/wiki/Disposable_email_address) validation flow
7. Simple SMTP debugger

## License

The `truemail-go` package is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Versioning

`truemail-go` uses [Semantic Versioning 2.0.0](https://semver.org)
