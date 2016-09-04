# Frege Email

[![Build Status](https://travis-ci.org/y-taka-23/frege-email.svg?branch=master)](https://travis-ci.org/y-taka-23/frege-email)

__Frege Email__ aims to provide a simple API for sending emails via SMTP. It will help you, for instance, to equip your web application written in Frege with notifications.

The library is built by wrapping Java's [Apache Commons Email](https://commons.apache.org/proper/commons-email/index.html). So you would be able to see it as a working example of Frege-Java interoperations.

## Examples

### A simple email

The following is the simplest self-contained example to send an email.

```
module Main where

import io.cheshirecat.frege.Email

server   = smtpServer.{ hostName = "smtp.example.com" }
addr     = address.{ email = "foo@example.com" }
testMail = email.{ subject  = "Test Mail"
                 , from     = addr
                 , to       = [addr]
                 , message  = "This is a test mail."
                 }

main _ = do
    flip catch handler $ do
        sendEmail server Nothing testMail
  where handler = \(e::EmailException) -> e.printStackTrace
```

The  `smtpServer` is provided by the library for simplicity, which sends emails via port 25.
To change the port,  update the `portNumber` field of `stmpServer`.

### Through your Gmail account

You can send emails through you Gmail account. Unlike the first example, you should:

* Use `sslSMTPServer` instead of `smtpServer`. It sends emails via port 465, SMTP over SSL.
* Set your Google account in an `authentication`.

```
...
server = sslSMTPServer.{ hostName = "smtp.gmail.com" }
auth   = authentication.{ userName = "yourusername"
                        , password = "yourpassword"
                        }
...

main _ = do
    flip catch handler $ do
        sendEmail server (Just auth) testMail
  where handler = \(e::EmailException) -> e.printStackTrace
```

Note that, for now, you have to allow less secure applications to access your Google accounts. See also [the official help page](https://support.google.com/accounts/answer/6010255).

### With attachments

Sending emails with attachments is accomplished by using the `attached` field like:

```
...
file = attachment.{ name = "No Title"
                  , path = "path/to/your/picture.jpg"
                  }
testMail = email.{ subject  = "Test Mail"
                 , from     = addr
                 , to       = [addr]
                 , message  = "This is a test mail."
                 , attached = [file]
                 }
...
```

## Build Settings

### Version compatibility

| Frege Email | Frege Compiler | Target JDK | Chinook (FYI) |
|:-:|:-:|:-:|:-:|
| 0.1.0-SNAPSHOT (HEAD) | [3.23.288-gaa3af0c](https://bintray.com/bintray/jcenter/org.frege-lang%3Afrege/3.23.288-gaa3af0c) | 1.8 | [0.2.0](https://bintray.com/januslynd/maven/chinook-core/0.2.0) |

## License

This project is released under the Apache 2.0 license. For more details, see [LICENSE](./LICENSE) file.
