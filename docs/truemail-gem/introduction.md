![Truemail - configurable framework agnostic plain Ruby email validator](https://truemail-rb.org/assets/images/truemail_logo.png)

[![Maintainability](https://api.codeclimate.com/v1/badges/657aa241399927dcd2e2/maintainability)](https://codeclimate.com/github/rubygarage/truemail/maintainability) [![Test Coverage](https://api.codeclimate.com/v1/badges/657aa241399927dcd2e2/test_coverage)](https://codeclimate.com/github/rubygarage/truemail/test_coverage) [![CircleCI](https://circleci.com/gh/rubygarage/truemail/tree/master.svg?style=svg)](https://circleci.com/gh/rubygarage/truemail/tree/master) [![Gem Version](https://badge.fury.io/rb/truemail.svg)](https://badge.fury.io/rb/truemail) [![Downloads](https://img.shields.io/gem/dt/truemail.svg?colorA=004d99&colorB=0073e6)](https://rubygems.org/gems/truemail) [![Gitter](https://badges.gitter.im/truemail-rb/community.svg)](https://gitter.im/truemail-rb/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge) [![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-v1.4%20adopted-ff69b4.svg)](https://github.com/rubygarage/truemail/blob/master/CODE_OF_CONDUCT.md)

> Configurable **framework agnostic** plain Ruby email validator/verifier. Verify email via Regex, DNS and SMTP. Be sure that email address valid and exists.

## Synopsis

Email validation is a tricky thing. There are a number of different ways to validate an email address and all mechanisms must conform with the best practices and provide proper validation. The Truemail gem helps you to validate emails via regex pattern (email syntax checking), presence of DNS records (mail-server existence checking), and real existence of email account on a current email server. Also Truemail gem allows performing an audit of the host in which runs.

## Features

1. Configurable layer-designed validator, validate only what you need
2. Minimal runtime dependencies
3. Supporting of internationalized emails ([EAI](https://en.wikipedia.org/wiki/Email_address#Internationalization))
4. Whitelist/blacklist validation's layers
5. Simple SMTP debugger
6. Event logger
7. Host auditor tools (helps to detect common host problems interfering to proper email verification)
8. JSON serializers for audition and validation results

## License

The `truemail` gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Versioning

`truemail` uses [Semantic Versioning 2.0.0](https://semver.org)
