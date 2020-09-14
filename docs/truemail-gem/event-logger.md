# Event logger

Truemail gem allows to output tracking events to stdout/file or both of these. Please note, at least one of the outputs must exist. Tracking event by default is `:error`

```ruby
Truemail.configure do |config|
  config.logger = {
    tracking_event: :all,
    stdout: true,
    log_absolute_path: '/home/app/log/truemail.log'
  }
end
```

## Available tracking events

- `:all`, all detected events including success validation cases
- `:unrecognized_error`, unrecognized errors only (when `smtp_safe_check = true` and SMTP server does not return an exact answer that the email does not exist)
- `:recognized_error`, recognized errors only
- `:error`, recognized and unrecognized errors only
