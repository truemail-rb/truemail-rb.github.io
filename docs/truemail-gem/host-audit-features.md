# Host audit features

Truemail gem allows performing an audit of the host in which runs. It will help to detect common host problems interfering to proper email verification.

## IP audit

Checks is current Truemail host has proper internet connection and detects current host ip address.

## DNS audit

Checks is verifier domain refer to current IP address of Truemail host ([verefier host](terminology?id=verifier-host)).

## PTR audit

Checks is [PTR record](terminology?id=ptr-record) exists for your Truemail host ip address exists and refers to current verifier domain.

> Because generic domain names without a PTR are often associated with spammers, incoming mail servers identify email from hosts without PTR records as spam and you can't verify yours emails qualitatively.

## Example of using

```ruby
Truemail.host_audit

# Everything is good
=> #<Truemail::Auditor:0x00005580df358828
   @result=
     #<struct Truemail::Auditor::Result
       current_host_ip="127.0.0.1",
       warnings={}>,
       configuration=
        #<Truemail::Configuration:0x00005615e86327a8
         @blacklisted_domains=[],
         @connection_attempts=2,
         @connection_timeout=2,
         @default_validation_type=:smtp,
         @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
         @response_timeout=2,
         @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
         @not_rfc_mx_lookup_flow=false,
         @smtp_fail_fast=false,
         @smtp_safe_check=false,
         @validation_type_by_domain={},
         @verifier_domain="example.com",
         @verifier_email="verifier@example.com",
         @whitelist_validation=false,
         @whitelisted_domains=[]>

# Has audit warnings
=> #<Truemail::Auditor:0x00005580df358828
   @result=
     #<struct Truemail::Auditor::Result
       current_host_ip="127.0.0.1",
       warnings={
         :dns=>"A-record of verifier domain not refers to current host ip address",
         :ptr=>"PTR-record does not reference to current verifier domain"
       },
       configuration=
        #<Truemail::Configuration:0x00005615e86327a8
         @blacklisted_domains=[],
         @connection_attempts=2,
         @connection_timeout=2,
         @default_validation_type=:smtp,
         @email_pattern=/(?=\A.{6,255}\z)(\A([\p{L}0-9]+[\w|\-|\.|\+]*)@((?i-mx:[\p{L}0-9]+([\-\.]{1}[\p{L}0-9]+)*\.[\p{L}]{2,63}))\z)/,
         @response_timeout=2,
         @smtp_error_body_pattern=/(?=.*550)(?=.*(user|account|customer|mailbox)).*/i,
         @not_rfc_mx_lookup_flow=false,
         @smtp_fail_fast=false,
         @smtp_safe_check=false,
         @validation_type_by_domain={},
         @verifier_domain="example.com",
         @verifier_email="verifier@example.com",
         @whitelist_validation=false,
         @whitelisted_domains=[]>
```
