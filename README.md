[<img src="https://sling.apache.org/res/logos/sling.png"/>](https://sling.apache.org)

 [![Build Status](https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-commons-messaging-mail/job/master/badge/icon)](https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-commons-messaging-mail/job/master/) [![Test Status](https://img.shields.io/jenkins/tests.svg?jobUrl=https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-commons-messaging-mail/job/master/)](https://ci-builds.apache.org/job/Sling/job/modules/job/sling-org-apache-sling-commons-messaging-mail/job/master/test/?width=800&height=600) [![Coverage](https://sonarcloud.io/api/project_badges/measure?project=apache_sling-org-apache-sling-commons-messaging-mail&metric=coverage)](https://sonarcloud.io/dashboard?id=apache_sling-org-apache-sling-commons-messaging-mail) [![Sonarcloud Status](https://sonarcloud.io/api/project_badges/measure?project=apache_sling-org-apache-sling-commons-messaging-mail&metric=alert_status)](https://sonarcloud.io/dashboard?id=apache_sling-org-apache-sling-commons-messaging-mail) [![Maven Central](https://maven-badges.herokuapp.com/maven-central/org.apache.sling/org.apache.sling.commons.messaging.mail/badge.svg)](https://search.maven.org/#search%7Cga%7C1%7Cg%3A%22org.apache.sling%22%20a%3A%22org.apache.sling.commons.messaging.mail%22) [![JavaDocs](https://www.javadoc.io/badge/org.apache.sling/org.apache.sling.commons.messaging.mail.svg)](https://www.javadoc.io/doc/org.apache.sling/org.apache.sling.commons.messaging.mail) [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

# Apache Sling Commons Messaging Mail

This module is part of the [Apache Sling](https://sling.apache.org) project.

This module provides a simple layer on top of [Jakarta Mail](https://eclipse-ee4j.github.io/mail/) (former [JavaMail](https://javaee.github.io/javamail/)) including a message builder and a service to send mails via SMTPS.

* Mail Service: sends MIME messages.
* Message Builder: builds plain text and HTML messages with attachments and inline images 
* Message ID Provider: allows overwriting default message IDs by custom ones


## Example

```
    @Reference
    MailService mailService;

    String subject = "Rudy, A Message to You";
    String text = "Stop your messing around
Better think of your future
Time you straighten right out
Creating problems in town
…";
    String html = […];
    byte[] attachment = […];
    byte[] inline = […];

    MimeMessage message = mailService.getMessageBuilder()
        .from("dandy.livingstone@kingston.jamaica.example.net", "Dandy Livingstone")
        .to("the.specials@coventry.england.example.net", "The Specials")
        .replyTo("rocksteady@jamaica.example.net");
        .subject(subject)
        .text(text)
        .html(html)
        .attachment(attachment, "image/png", "attachment.png")
        .inline(inline, "image/png", "inline")
        .build();

    mailService.sendMessage(message);
```


## Integration Tests

Integration tests require a running SMTP server. By default a [GreenMail](http://www.icegreen.com/greenmail/) server is started.

An external SMTP server for validating messages with real mail clients can be used by setting required properties:

    mvn clean install\
      -Dsling.test.mail.smtps.server.external=true\
      -Dsling.test.mail.smtps.from=envelope-from@example.org\
      -Dsling.test.mail.smtps.host=localhost\
      -Dsling.test.mail.smtps.port=465\
      -Dsling.test.mail.smtps.username=username\
      -Dsling.test.mail.smtps.password=password\
      -Dsling.test.mail.from.address=from@example.org\
      -Dsling.test.mail.from.name=From\ Sender\
      -Dsling.test.mail.to.address=to@example.org\
      -Dsling.test.mail.to.name=To\ Recipient\
      -Dsling.test.mail.replyTo.address=replyto@example.org\
      -Dsling.test.mail.replyTo.name=Reply\ To
