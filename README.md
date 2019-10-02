# Twilito

[![Build Status](https://travis-ci.org/alexford/twilito.svg?branch=master)](https://travis-ci.org/alexford/twilito)

A tiny, zero dependency helper for sending text messages with Twilio. Just enough of a wrapper to abstract away Twilio's REST API for sending messages, without *anything* else.

## Why

Twilio's [full Ruby library](https://github.com/twilio/twilio-ruby) does a *lot*, and has a large memory footprint to go with it—too large for just sending a message. It's also more difficult to mock and verify in tests than I'd like.

Using [Twilio's REST API](https://www.twilio.com/docs/usage/api) directly is fine, but can be cumbersome.

You should consider using Twilito if the only thing you need to do is send text messages and you don't want to worry about making HTTP requests to Twilio yourself.

If you use more of Twilio, consider [twilio-ruby](https://github.com/twilio/twilio-ruby) or interact with the REST API in another way.

## Usage

#### Install the gem

```
gem 'twilito'
```

#### Simplest case

```ruby
# All options are required (but can be defaulted, see below)
result = Twilito.send_sms(
  to: '+15555555555',
  from: '+15554444444',
  content: 'This is my content'
  account_sid: '...', # Twilio Credentials
  auth_token: '...'
)

# Returns Twilito::Result struct

result.success? # => boolean
result.errors # => [] or error messages
result.sid #=> Twilio SID for Message (SM[...])
result.response # => Raw response (instance of Net::HTTPResponse)
result.data # => Hash of response data (parsed from JSON)
```

#### Use send_sms! to raise on error instead

```ruby
begin
  Twilito.send_sms!(
    to: '+15555555555',
    from: '+12333',
    body: 'This is my content',
    account_sid: '...',
    auth_token: '...'
  )
rescue Twilito::SendError => e
  e.message # => 'Error from Twilio API'
  e.response # => Raw response (instance of Net::HTTPResponse)
end
```

#### Every argument can be defaulted

```ruby
# In an initializer or something like that:

Twilito.configure do |config|
  # Store your secrets elsewhere
  config.account_sid = ENV['TWILIO_ACCOUNT_SID']
  config.auth_token = ENV['TWILIO_AUTH_TOKEN']

  config.from = '+16145555555'
end
```

```ruby
# Later, in your code:

Twilito.send_sms!(to: '+15555555555', body: 'Foo')
```

**Everything can be defaulted, including the message body, so that a bare `Twilio.send_sms!` can work in your code**

## Testing your code

_TODO: Add examples of mocking and/or test helpers for asserting your code sends an SMS_

## Contributing

_TODO_
