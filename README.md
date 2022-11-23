# System Handler for Aribitrary Resource Exfiltration

A specification and sample implementation for a sharing architecture on the freedesktop.

## Need



## Glossary

* Content: a generic term for the items a user might wish to share. Generally, this would be in the form of text, links, videos, audio, and photos, although the specification is intended to be broad enough to allow for any arbitrary types of data (provided there is a responder on the system that understands how to work with that data type).
* Share button: A button within an application that, when clicked, allows a user to take content out of the current app and use it in some context inside of another app.
* ShareActionSource: An interface that allows an application to offer the ability to share data.
* ShareActionHandler: An interface that allows a service to accept share requests from a source, and connect it to an app that can respond to the request.
* ShareActionResponder: An interface that allows an application to offer the ability to respond to share requests in some way.
* Implementation: An application or service that concretely implements a given interface.

## Architecture

The SHARE systems is based on the idea of three roles, in software.

The most important piece is a background service, ideally one that gets started during system boot automatically. This service waits and listens for requests for an application to share something. This service in the middle is referred to as the **ShareActionHandler**.

For an application to be able to share content, it would need to implement the **ShareActionSource** interface.

For an application to be able to respond to the share request, it would need to implement the ** ShareActionResponder** interface. The responding application would also need to "register" itself to the handler, and identify the types of files (MIMETypes) that it is capable of handling.

## Recommended Implementation Details


