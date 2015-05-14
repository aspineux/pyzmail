:orphan:

pyzinfomail manual page
=======================

Synopsis
--------

**pyzinfomail** <*filename*>

Description
-----------

**pyzinfomail** parse and display some data from an email. This is mostly a
sample on how to use the **pyzmail** library.


Sample
------

usage::

    $ pyzinfomail mail.eml
    Subject: u'The subject'
    From: (u'Sender', 'sender@example.com')
    To: [(u'Recipient', 'recipient@example.com')]
    Cc: []
    Date: 'Tue, 7 Jun 2011 16:32:17 +0200'
    Message-Id: '20110830190805.5096.33348.pyzsendmail@host.example.com'
       *filename=None type=text/plain charset=ISO-8859-1 desc=None size=13
           > Hello World
       *filename=None type=text/html charset=ISO-8859-1 desc=None size=23


See also
--------

:manpage:`pyzsendmail(1)`

Author
------

Alain Spineux <alain.spineux@gmail.com>
