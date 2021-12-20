---
description: Description of flags related to verifying phone numbers with Twilio
---

# Phone Number Verification

Note: All flags below are required in order for the phone number verification service to work properly. You'll need to setup an account with [Twilio](https://www.twilio.com)

## Twilio Account SID

`--twilio-account-sid`

Type: String

Default: None

Twilio account SID (string id). Twilio is used for sending verification texts. See [twilio documentation](https://www.twilio.com/docs/verify/api/verification) for more info.

## Twilio Auth Token

`--twilio-auth-token`

Type: String

Default: None

Twilio authentication token. See [twilio documentation](https://www.twilio.com/docs/verify/api/verification) for more info.

## Twilio Verify Service ID

`--twilio-verify-service-id`

Type: String

Default: None

ID for a verify service configured within Twilio (used for verification texts)
