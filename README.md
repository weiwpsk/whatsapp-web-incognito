# Fork of WAIncognito
This is a fork of the [browser extension WAIncognito from tomer8007](https://github.com/tomer8007/whatsapp-web-incognito).
In addition to disabling read receipts and presence updates on WhatsApp Web, this also adds a battery percentage indicator.

<img src="https://raw.githubusercontent.com/philklc/whatsapp-web-incognito/master/promotion.jpg" >

## Installing from GitHub directly
To install, clone this repository into a directory, and add it to Chrome using the 'Load unpacked extension' option when developer mode is turned on.

## How it works
This extension works by intercepting the WebSocket frames between chrome and WhatsApp's servers using a modified `WebSocket` constructor (see [wsHook](https://github.com/skepticfx/wshook)). 

Those frames are then decrypted if needed using the local encryption keys (stored in `localStorage`), and decoded from a binary form using a javascript code from WhatsApp's original implementation. 

The resulting "nodes" are then simply checked to see if WhatsApp tries to send out a `read`  or `presence` action, and if so, the extension blocks it and fakes a failure response from the server.

#### Battery status
WhatsApp Web receives the battery percentage of the device upon initial connection and periodically to determine whether to display a "Phone Battery Low" message or not. This fork displays the raw battery percentage and charging status at all times.

## Organization & Internals
The main code of the extension is located in `core/interception.js` and in `core/ui.js`. 

Other files inside the `core` folder deal with the infrastructure that makes the interception and the decoding works. There is also an additional `parsing/` code for parsing messeges (such as `message_types.js`) that is rarely used in the extension.
`background.js` mainly keeps track of the saved prefrences using `localStorage`.

If you want to see what kind of messages WhatsApp is sending and receiving over WebSocket in real-time, you can type `WAdebugMode = true` in the javascript console. Incoming and outgoing payloads (after decryption) will be printed out.

## Privacy
No data is ever transmitted to anywhere. Privacy policy [here](https://github.com/tomer8007/whatsapp-web-incognito/wiki/Chrome-Extension-Privacy-Policy).

