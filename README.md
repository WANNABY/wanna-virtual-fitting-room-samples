# wanna-virtual-fitting-room-samples

**TODO: doc to be updated**

WANNA VFR can be integrated to any website easily by simple adding an iframe on mobile
or link (QR-code) to VFR on desktop.

## Mobile iframe integration

Integration on mobile is easy as adding an iframe to page.  
An example of integration can be found [here](./samples/iframe_mobile.html).

In the example WANNA VFR is loaded in iframe inside a modal after clicking the button.

If a modal dialog is not suitable for you -- iframe can be added into any other place of the page.

### Receive current model

If there is a need to get current model from WANNA VFR (e.g. to navigate to it on main site)
you need to subscribe to updates from WANNA VFR. The following code snippet demonstrates the subscription:

```javascript
window.addEventListener('message', event => {
  if (event.origin === vfrOrigin /* the origin of WANNA VFR e.g. https://demo.ar.wanna.fashion */) {
    const eventName = event.data.event
    const data = event.data.data
    // handle event as required
  }
})
```

The `data` property of event has following properties:

- `event` - the name of the event. Type `string`
- `data` - the event data. Type `object`

Currently supported events are:

| Event name  | Description                        | Data properties                            |
|-------------|------------------------------------|--------------------------------------------|
| MODEL_SET   | The model has been used for try-on | `modelId` - `string` - the id of the model |

## Desktop QR-code integration

On desktop QR-code code is displayed. All you need to do is to add a sections with QR-code to VFR on your page.
An example of integration can be found [here](./samples/desktop.html).

NOTE: in the example [QR-Code-generator](https://github.com/nayuki/QR-Code-generator) library is used for QR-code
generation only for demo purposes.
You can use any other library that suits your needs.



