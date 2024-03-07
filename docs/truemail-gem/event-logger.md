# Event logger

Truemail gem allows to output tracking events to stdout/file or both of these. Please note, at least one of the outputs (stdout or file path) must exists. Tracking event by default is `:error`.

```ruby
Truemail.configure do |config|
  config.logger = {
    tracking_event: :all,
    stdout: true,
    log_absolute_path: '/home/app/log/truemail.log'
  }
end
```

## Using custom logger

By default Truemail uses `Logger`, default logger instance from Ruby stdlib. But you can override this behavior passing your logger instance in event logger configuration. Please note, your own logger instance should have the same interface as builtin stdlib `Logger` instance. In this case `custom_logger` is only one required field for logger configuration (you don't have to use `stdout` and `log_absolute_path`).

```ruby
Truemail.configure do |config|
  config.logger = { custom_logger: MyCustomLogger.new }
end
```

## Available tracking events

- `:all`, all detected events including success validation cases
- `:unrecognized_error`, unrecognized errors only (when `smtp_safe_check = true` and SMTP server does not return an exact answer that the email does not exist)
- `:recognized_error`, recognized errors only
- `:error`, recognized and unrecognized errors only
