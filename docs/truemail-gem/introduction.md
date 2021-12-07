# ![Truemail - configurable framework agnostic plain Ruby email validator](https://truemail-rb.org/assets/images/truemail_logo.png)

[![Maintainability](https://api.codeclimate.com/v1/badges/0fea6d2e64d78d66b149/maintainability)](https://codeclimate.com/github/truemail-rb/truemail/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/0fea6d2e64d78d66b149/test_coverage)](https://codeclimate.com/github/truemail-rb/truemail/test_coverage)
[![CircleCI](https://circleci.com/gh/truemail-rb/truemail/tree/master.svg?style=svg)](https://circleci.com/gh/truemail-rb/truemail/tree/master)
[![Gem Version](https://badge.fury.io/rb/truemail.svg)](https://badge.fury.io/rb/truemail)
[![Downloads](https://img.shields.io/gem/dt/truemail.svg?colorA=004d99&colorB=0073e6)](https://rubygems.org/gems/truemail)
[![In Awesome Ruby](https://raw.githubusercontent.com/sindresorhus/awesome/main/media/mentioned-badge.svg)](https://github.com/markets/awesome-ruby)
[![GitHub](https://img.shields.io/github/license/truemail-rb/truemail-rack)](https://github.com/truemail-rb/truemail/blob/master/LICENSE.txt)
[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-v1.4%20adopted-ff69b4.svg)](https://github.com/truemail-rb/truemail/blob/master/CODE_OF_CONDUCT.md)
[![Gitter](https://badges.gitter.im/truemail-rb/community.svg)](https://gitter.im/truemail-rb/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

> Configurable **framework agnostic** plain Ruby email validator/verifier. Verify email via Regex, DNS, SMTP and even more. Be sure that email address valid and exists.

## Synopsis

Email validation is a tricky thing. There are a number of different ways to validate an email address and all mechanisms must conform with the best practices and provide proper validation. The Truemail gem helps you to validate emails via regex pattern (email syntax checking), presence of DNS records (mail-server existence checking), and real existence of email account on a current email server. Also Truemail gem allows performing an audit of the host in which runs.

## Features

1. Configurable layer-designed validator, validate only what you need
2. Only one runtime dependency
3. Supporting of internationalized emails ([EAI](https://en.wikipedia.org/wiki/Email_address#Internationalization))
4. Whitelist/blacklist validation's layers
5. Ability to configure different MX/SMTP validation flows
6. Ability to configure [DEA](https://en.wikipedia.org/wiki/Disposable_email_address) validation flow
7. Simple SMTP debugger
8. Event logger
9. Host auditor tools (helps to detect common host problems interfering to proper email verification)
10. JSON serializers for audition and validation results
11. Ability to use the library as independent stateless microservice ([Truemail Server](https://truemail-rb.org/truemail-rack))

## License

The `truemail` gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Versioning

`truemail` uses [Semantic Versioning 2.0.0](https://semver.org)
