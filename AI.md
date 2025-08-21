---
name: ww-input-verification-code
description: The ww-input-verification-code component provides a user-friendly interface for entering verification codes (usually OTP) by rendering multiple input fields that automatically advance focus after each character is entered, supporting both individual input and full code pasting, and emitting events for changes and completion.
keywords:
  - verification code input
  - automatic focus
  - character input fields
  - code pasting support
  - input field events
  - change event
  - complete event
  - initvaluechange event
  - ww-input-basic
  - nbrofcode property
---

#### ww-input-verification-code

Component Purpose: Renders input fields for verification code entry with automatic focus advancement.

Properties:
- nbrOfCode: number - Number of input fields to display. Default: 6.

Children:
- input: ww-input-basic - MANDATORY - Configuration for individual input fields. Styling is important to ensure proper alignment and spacing.

Features:
- Auto-focuses next field after character entry
- Supports full code pasting
- Automatic character distribution across fields

Events:
- change: Triggered when any input value changes. Payload: {value: string}
- initValueChange: Triggered when initial value changes. Payload: {value: string}
- complete: Triggered when full code is entered. Payload: {value: string}

Variables:
- value: string - Current value of the verification code input (readonly, resettable)

Example:
<elements>
{"uid":0,"tag":"ww-input-verification-code","name":"Verification Code Input","props":{"default":{"nbrOfCode":6,"fieldName":"verification-code","required":true}},"styles":{"default":{"width":"100%","display":"flex"}},"children":{"input":{"uid":1}}}
{"uid":1,"tag":"ww-input-basic","props":{"default":{"value":"","type":"text","displayPassword":false,"readonly":false,"required":true,"precision":"0.1","step":1,"timePrecision":1,"rows":4,"resize":false,"min":"0","max":"10000","minDate":"","maxDate":"","hideArrows":false,"debounce":false,"debounceDelay":"500ms","autocomplete":false,"fieldName":"","customValidation":false,"validation":""}},"styles":{"default":{"border":"2px solid #999999","borderRadius":"5px","width":"40px","height":"40px","aspectRatio":"unset","margin":"0px 5px","customCss":{},"fontSize":"20px"}}}
</elements>
