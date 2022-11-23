# System Handler for Aribitrary Resource Exfiltration

A specification and sample implementation for a sharing architecture on the freedesktop.

## Need

Across most modern OSes, users are familiar with the concept of a Share button that they can click to share the content they're viewing without having to leave the current context or manually copy and paste data or links into another application.

The classic example is wanting to share the current web page with a friend, but there are a number of different use-cases for the Share button:

* Sharing a photo with a friend over text, using the default messaging app
* Sharing a link to a web page over email, through the default email app
* Sending a video from a web browser to a dedicated media player
* Saving the current web page to a reading app (for example, Instapaper or Pocket)

Currently, there is not a standard way of doing this on the free desktop. Some apps implement their own sharing mechanism (usually by interacting with a web service's API), or by expecting users to copy and paste.

Instead, there needs to be a standard at the OS level that makes it possible to take content from an app and securely share it with another app that can then handle it in an appropriate way (share it with a contact, save it for later, etc.)

## Glossary

* `Content`: a generic term for the items a user might wish to share. Generally, this would be in the form of text, links, videos, audio, and photos, although the specification is intended to be broad enough to allow for any arbitrary types of data (provided there is a responder on the system that understands how to work with that data type).
* `Share button`: A button within an application that, when clicked, allows a user to take content out of the current app and use it in some context inside of another app.
* `ShareActionSource`: An interface that allows an application to offer the ability to share data.
* `ShareActionHandler`: An interface that allows a service to accept share requests from a source, and connect it to an app that can respond to the request.
* `ShareActionResponder`: An interface that allows an application to offer the ability to respond to share requests in some way.
* `Implementation`: An application or service that concretely implements a given interface.

## Architecture

The SHARE systems is based on the idea of three roles, in software.

The most important piece is a background service, ideally one that gets started during system boot automatically. This service waits and listens for requests for an application to share something. This service in the middle is referred to as the **ShareActionHandler**.

For an application to be able to share content, it would need to implement the **ShareActionSource** interface.

For an application to be able to respond to the share request, it would need to implement the ** ShareActionResponder** interface. The responding application would also need to "register" itself to the handler, and identify the types of files (MIMETypes) that it is capable of handling.

## Recommended Implementation Details

### Hypothetical Example

Let's say that you're using a photo sharing application, perhaps _Shotwell_, and want to share a picture directly to a contact via a chat application.

Pressing a share button in _Shotwell_ (a `ShareActionSource`) sends a message that a Share Action has been triggered.

The application, let's call it GtkActionHandler for the sake of argument, receives the message and presents a graphical display to the user showing a list of possible share targets (applications, users, etc.) that can accept content of that type. 

The user can then select the chat application, which closes the handler, and sends another signal directly to the application that was selected (a `ShareActionResponder`). From there, the responding application receives the data and the share request, and the user proceeds based on the in-app workflow.

