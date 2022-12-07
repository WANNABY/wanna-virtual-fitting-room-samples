# WANNA Virtual Fitting Room

WANNA Virtual Fitting Room (VFR) is a low-code solution with a customizable prebuilt UI. To integrate it into your website, you only need to add an iframe to the page.

<!-- TOC -->
- [Prerequisites](#prerequisites)
- [Mobile: build in an iframe](#mobile-build-in-an-iframe)
	- [Iframe settings](#iframe-settings)
	- [Get the current model ID](#get-the-current-model-id)
- [Desktop: display a QR code](#desktop-display-a-qr-code)
<!-- /TOC -->

## Prerequisites

In this tutorial, you will be integrating a fitting room that was already configured for you by WANNA engineers. You should have:

* a link to your personal fitting room
* the list of model IDs that will be tried on (you will use it to select which models to display and which one to start with)

Virtual Fitting Room could only be comfortably used [on a smartphone](#mobile-build-in-an-iframe). You probably already have different versions of your website for desktop and mobile devices, but if not, you will need to implement the selection on your side. For [desktop version](#desktop-display-a-qr-code) we recommend displaying a QR code with the link to smoothly guide the user to the try-on page.

## Mobile: build in an iframe

The essential part of integration is simply to load Virtual Fitting Room into an iframe on the page or just open it in a separate page. Choose the look and user experience you would prefer. There are three main options:

* **[recommended]** build the fitting room into your page, leaving the original header and footer in place: </br> ![Virtual Fitting Room built into the source page](images/integration_partscreen.png)
* show the fitting room in a pop-up modal that overlays the page from which it was started: </br> ![Virtual Fitting Room loaded as a modal](images/integration_modal_popup.png)
* redirect to another page with the fullscreen view of the fitting room: </br> ![Virtual Fitting Room fullscreen view](images/integration_fullscreen.png)

We recommend the first option, as it ensures the best user experience and more likely conversion of try-on into purchase. Let's walk through the process.

First, create an iframe element on the page and put the link to your Virtual Fitting Room into its `src` attribute:

```javascript
iframe.src = vfrLink
```

### Iframe settings

Add query parameters to the link to configure the optional settings:

* `modelid` — a comma-separated list of model identifiers for the models that should be displayed for try-on.
* `startwithid` — the identifier of the model that should be loaded first. Note that the order of the models won't change.
* `locale` — the two-letter code for the Virtual Fitting Room UI language and metadata. If the specified locale isn't available, the English locale will be loaded instead.

For example, the following link will show only two models to be tried on, start with the second one, and display the interface and page metadata in German: 

`https://demo.ar.wanna.fashion/?modelid=wanna01,wanna02&startwithid=wanna02&locale=de`

### Get the current model ID

It should be easy for the user to buy or add to favorites the model they're trying on. You may want to add, for example, a "shop now" button to the page footer. To do that, get the model ID from Virtual Fitting Room. To do that, listen to events from the fitting room and get the ID of the model that is currently loaded. Here's a code snippet that shows how to subscribe to events:

```javascript
window.addEventListener('message', event => {
  if (event.origin === vfrOrigin /* the origin of WANNA VFR, for example https://demo.ar.wanna.fashion */) {
    const eventName = event.data.event
    const data = event.data.data
	const model = data.modelId
    // use the model identifier, for example to find the link to the product page
  }
})
```

The `data` property of event has following properties:

- `event` (type `string`) — the name of the event
- `data` (type `object`) — the event data

Virtual Fitting Room only raises one kind of an event:

| Event name    | Description                                    | Data properties                                  |
|---------------|------------------------------------------------|--------------------------------------------------|
| `MODEL_SET`   | The model that is currently loaded for try-on. | `modelId` (type `string`) — the model identifier |

**Important:** Events are only available for iframe. If you're simply redirecting to Virtual Fitting Room, you won't be able to listen to events and obtain the model ID using this method.

Consult also [our demo sample](samples/iframe_mobile.html) which shows the simplest way of loading the fitting room as a pop-up modal that opens on button click.

## Desktop: display a QR code

On the product page from which you would like the client to start virtual try-on, display a QR code that the client can scan with their phone and go directly to Virtual Fitting Room on mobile. [Our sample](samples/desktop.html) uses the open-source [QR-Code-generator](https://github.com/nayuki/QR-Code-generator) library for demo purposes. Choose any QR code generating tool that suits you.