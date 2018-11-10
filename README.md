# switfmailer-openpgp
OpenPGP signer for SwiftMailer
needs to change from php5-gnupg to the opengpg-php dependancy
https://github.com/singpolyma/openpgp-php

WIP, help welcome :)

Dependencies:
* openpgp-php needs to be installed

Setup:
* Copy the file to  lib / classes / Swift / Signers
* Generate GPG keys (without passphrases), in the examples below the keys are put in folder home subfolder .pgp

Usage example for e-mail signing:
```
$path = "~/.pgp";
$message = Swift_Message::newInstance()
->setSubject('Your subject')
->setBody('Here is the message itself','text/html');
$openpgpsigner = Swift_Signers_OpenPGPSigner::newInstance($signingKey = "<fill in e-mail address signer>", $recipientKeys = array(), $gnupgHome = $path);
$openpgpsigner->setGnupgHome($path);
$openpgpsigner->setEncrypt(false);
$openpgpsigner->addSignature("<fill in e-mail address signer>");
$message->attachSigner($openpgpsigner);
$transport = Swift_SmtpTransport::newInstance(<fill in smtp server>, <fill in smtp port>, 'tls')
  ->setUsername(<fill in smtp username>)
  ->setPassword(<fill in smtp password>)
  ;
/* Create the Mailer using your created Transport */
$mailer = Swift_Mailer::newInstance($transport);
/* Send the message */
$mailer->send($message);
```

Usage example for e-mail encryption:
```
$message = Swift_Message::newInstance()
->setSubject('Your subject')
->setFrom(array('<fill in e-mail address sender>' => '<name sender>'))
->setTo(array('<fill in e-mail address recipient>' => '<name recipient>'))
->setBody('<b>Here is the message itself</b>','text/html');

$openpgpsigner = Swift_Signers_OpenPGPSigner::newInstance();
$openpgpsigner->setGnupgHome($path);
$message->attachSigner($openpgpsigner);

$transport = Swift_SmtpTransport::newInstance(<fill in smtp server>, <fill in smtp port>, 'tls')
  ->setUsername(<fill in smtp username>)
  ->setPassword(<fill in smtp password>)
  ;
/* Create the Mailer using your created Transport */
$mailer = Swift_Mailer::newInstance($transport);
/* Send the message */
$mailer->send($message);
```
