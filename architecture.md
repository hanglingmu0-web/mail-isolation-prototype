# Architecture and data flow

## Separate the email environment

Ubuntu inside Hyper-V provides the email environment alongside the everyday Windows workspace. Thunderbird runs in Ubuntu for reading and composing messages, including Japanese text.

## Extract the required body text

The guest application extracts the body from a message selected by the user. HTML parsing takes place inside Ubuntu, and the resulting text is transferred to Windows.

Text Only Gate on Windows validates incoming data, normalizes characters, modifies URL-like notation, and displays the result in a plain-text field. The workflow does not pass the message body to a browser or an HTML renderer.

This channel is used for extracted body text. Attachments, images, and original message files remain in the guest environment. Body transfer does not use shared folders or clipboard sharing.

## Make saving an explicit action

The transferred text is displayed for the user to review. When the user chooses to save it, the Windows application generates a filename and creates a new UTF-8 `.txt` file. Original message filenames and subjects are not used to specify the destination.

## Distinguish encoding from interpretation

UTF-8 is a character encoding. HTML can also be written in UTF-8, so specifying the encoding alone does not restrict the content being exported. This design combines body extraction, validation on receipt, plain-text display, and creation of a new text file.
