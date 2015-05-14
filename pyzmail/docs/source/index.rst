.. Python easy mail library documentation master file, created by
   sphinx-quickstart on Fri Aug 19 12:16:52 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. title:: pyzmail

pyzmail: Python easy mail library
=================================

**pyzmail** is a **high level** mail library for Python. It provides functions and
classes that help for **reading**, **composing** and **sending** emails. **pyzmail**
exists because their is no reasons that handling mails with Python would be more
difficult than with popular mail clients like Outlook or Thunderbird.
**pyzmail** hides the complexity of the MIME structure and MIME
encoding/decoding. It also make the problems of the internationalization
encoding/decoding simpler.

Download and Install
--------------------

**pyzmail** is available for Python **2.6+** and **3.2+**
from `pypi <http://pypi.python.org/pypi/pyzmail>`_ and can
be easily installed using the `easy_install <http://peak.telecommunity.com/DevCenter/EasyInstall>`_
successor named `distribute <http://packages.python.org/distribute/>`_
and `pip <http://pip.openplans.org/>`_ using ::

    $ pip install pyzmail

to quickly install **distribute** and **pip**, use ::

    curl -O http://python-distribute.org/distribute_setup.py
    python distribute_setup.py
    easy_install pip

**pyzmail** can be installed the old way from sources. Download the archive from
`pypi <http://pypi.python.org/pypi/pyzmail>`_ and extract its content
into a directory. *cd* into this directory and run::

    > cd pyzmail-X.X.X
    > python setup.py install

Binary version of the scripts for **Windows** pyzmail-|release|-win32.zip can
be downloaded from `here <http://www.magiksys.net/download/pyzmail>`__.

**pyzmail** sources are also available on **github**
`https://github.com/aspineux/pyzmail <https://github.com/aspineux/pyzmail>`_

Support for Python 3.x
----------------------
.. sidebar:: Python 3.2+ supported

    .. image:: /_static/python-3.png

Python **3.2** is supported and has been tested. Python 3.0 and 3.1 are not supported
because none of them provide functions to handle 8bits encoded emails like in **3.2**
( :py:func:`email.message_from_bytes` & :py:func:`email.message_from_binary_file` )

At installation time, **pyzmail** sources are automatically converted by
`distribute <http://packages.python.org/distribute/>`_ using **2to3**.

Unfortunately, **scripts** are not converted in the process. You can convert them
using **2to3** yourself *(adapt* **paths** *to fit you configuration)*::

    /opt/python-3.2.2/bin/2to3 --no-diffs --write --nobackups /opt/python-3.2.2/bin/pyzinfomail
    /opt/python-3.2.2/bin/2to3 --no-diffs --write --nobackups /opt/python-3.2.2/bin/pyzsendmail


Use pyzmail
-----------

The package is split into 3 modules:

* `generate <api/pyzmail.generate-module.html>`_: Useful functions to compose and send mail   s
* `parse <api/pyzmail.parse-module.html>`_: Useful functions to parse emails
* `utils <api/pyzmail.utils-module.html>`_: Various functions used by other modules

Most important functions are available from the top of the `pyzmail <api/index.html>`_ package.

usage sample::

    import pyzmail

    #access function from top of pyzmail
    ret=pyzmail.compose_mail('me@foo.com', [ 'him@bar.com'], u'subject', \
                             'iso-8859-1', ('Hello world', 'us-ascii'))
    payload=ret[0]
    print payload
    msg=pyzmail.PyzMessage.factory(payload)
    print msg.get_subject()

    #use more specific function from inside modules
    print pyzmail.generate.format_addresses([('John', 'john@foo.com') ], \
                                            'From', 'us-ascii')
    print pyzmail.parse.decode_mail_header('=?iso-8859-1?q?Hello?=')

More in the `Quick Example`_ section.


Documentation
-------------

You can find lots of sample inside the *docstrings* but also in the *tests*
directory.

The documentation, samples, docstring and articles are all fitted for python 2.x.
Some occasional hint give some tricks about Python 3.x.

Articles
^^^^^^^^

To understand how this library works, you will find these 3 articles very useful.
They have been written before the first release of **pyzmail** and the code has
changed a little since:

    - `Parsing email using Python part 1 of 2 : The Header <http://blog.magiksys.net/parsing-email-using-python-header>`_
    - `Parsing email using Python part 2 of 2 : The content  <http://blog.magiksys.net/parsing-email-using-python-content>`_
    - `Generate and send mail with python: tutorial <http://blog.magiksys.net/generate-and-send-mail-with-python-tutorial>`_


API documentation
^^^^^^^^^^^^^^^^^

The `API documentation <api/index.html>`_ in *epydoc* format contains a lot
of **samples** in *doctest* string. You will find them very useful too.


Support
-------

Ask your questions `here <http://forum.magiksys.net>`__

Quick Example
-------------

Lets show you how it works !

Compose an email
^^^^^^^^^^^^^^^^

::

    import pyzmail

    sender=(u'Me', 'me@foo.com')
    recipients=[(u'Him', 'him@bar.com'), 'just@me.com']
    subject=u'the subject'
    text_content=u'Bonjour aux Fran\xe7ais'
    prefered_encoding='iso-8859-1'
    text_encoding='iso-8859-1'

    payload, mail_from, rcpt_to, msg_id=pyzmail.compose_mail(\
            sender, \
            recipients, \
            subject, \
            prefered_encoding, \
            (text_content, text_encoding), \
            html=None, \
            attachments=[('attached content', 'text', 'plain', 'text.txt', \
                          'us-ascii')])

    print payload

Look a the output::

    Content-Type: multipart/mixed; boundary="===============1727493275=="
    MIME-Version: 1.0
    From: Me <me@foo.com>
    To: Him <him@bar.com> , just@me.com
    Subject: the subject
    Date: Fri, 19 Aug 2011 16:04:42 +0200

    --===============1727493275==
    Content-Type: text/plain; charset="iso-8859-1"
    MIME-Version: 1.0
    Content-Transfer-Encoding: quoted-printable

    Bonjour aux Fran=E7ais
    --===============1727493275==
    Content-Type: text/plain; charset="us-ascii"
    MIME-Version: 1.0
    Content-Transfer-Encoding: 7bit
    Content-Disposition: attachment; filename="text.txt"

    attached content
    --===============1727493275==--

Send an email
^^^^^^^^^^^^^

First take a look at the other values returned by ``pyzmail.compose_mail()``::

    print 'Sender address:', mail_from
    print 'Recipients:', rcpt_to

Here are the values I can reuse for my SMTP connection::

    Sender address: me@foo.com
    Recipients: ['him@bar.com', 'just@me.com']

I want to send my email via my Gmail account::

    smtp_host='smtp.gmail.com'
    smtp_port=587
    smtp_mode='tls'
    smtp_login='my.gmail.addresse@gmail.com'
    smtp_password='my.gmail.password'

    ret=pyzmail.send_mail(payload, mail_from, rcpt_to, smtp_host, \
            smtp_port=smtp_port, smtp_mode=smtp_mode, \
            smtp_login=smtp_login, smtp_password=smtp_password)

    if isinstance(ret, dict):
        if ret:
            print 'failed recipients:', ', '.join(ret.keys())
        else:
            print 'success'
    else:
        print 'error:', ret

Here ``pyzmail.send_mail()`` combine **SSL** and **authentication**.


Parse an email
^^^^^^^^^^^^^^

Now lets try to read the email we have just composed::

    msg=pyzmail.PyzMessage.factory(payload)

    print 'Subject: %r' % (msg.get_subject(), )
    print 'From: %r' % (msg.get_address('from'), )
    print 'To: %r' % (msg.get_addresses('to'), )
    print 'Cc: %r' % (msg.get_addresses('cc'), )

Take a look at the outpout::

    Subject: u'the subject'
    From: (u'Me', 'me@foo.com')
    To: [(u'Him', 'him@bar.com'), (u'just@me.com', 'just@me.com')]
    Cc: []


And a little further regarding the mail content and attachment::

    for mailpart in msg.mailparts:
        print '    %sfilename=%r alt_filename=%r type=%s charset=%s desc=%s size=%d' % ( \
            '*'if mailpart.is_body else ' ', \
            mailpart.filename,  \
            mailpart.sanitized_filename, \
            mailpart.type, \
            mailpart.charset, \
            mailpart.part.get('Content-Description'), \
            len(mailpart.get_payload()) )
        if mailpart.type.startswith('text/'):
            # display first line of the text
            payload, used_charset=pyzmail.decode_text(mailpart.get_payload(), mailpart.charset, None)
            print '        >', payload.split('\\n')[0]

And the output::

    *filename=None alt_filename='text.txt' type=text/plain charset=iso-8859-1 desc=None size=20
        > Bonjour aux Français
     filename=u'text.txt' alt_filename='text-01.txt' type=text/plain charset=us-ascii desc=None size=16
        > attached content

The first one, with a ***** is the *text* content, the second one is the attachment.

You also have direct access to the *text* and *HTML* content using::

    if msg.text_part!=None:
        print '-- text --'
        print msg.text_part.get_payload()

    if msg.html_part!=None:
        print '-- html --'
        print msg.html_part.get_payload()

And the output::

    -- text --
    Bonjour aux Français

Their is no *HTML* part !

Tricks
------


Embedding image in HTML email
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Image embedding differs from linked images in that the image itself is
encoded, and included inside the message. Instead of using a normal URL
in the *IMG* tag inside the HTML body, we must use a *cid:target* reference
and assign this *target* name to the *Content-ID* of the embedded file.

See this sample::

    import base64
    import pyzmail

    angry_gif=base64.b64decode(
    """R0lGODlhDgAOALMAAAwMCYAAAACAAKaCIwAAgIAAgACAgPbTfoR/YP8AAAD/AAAA//rMUf8A/wD/
    //Tw5CH5BAAAAAAALAAAAAAOAA4AgwwMCYAAAACAAKaCIwAAgIAAgACAgPbTfoR/YP8AAAD/AAAA
    //rMUf8A/wD///Tw5AQ28B1Gqz3S6jop2sxnAYNGaghAHirQUZh6sEDGPQgy5/b9UI+eZkAkghhG
    ZPLIbMKcDMwLhIkAADs=
    """)

    text_content=u"I'm very angry. See attached document."
    html_content=u'<html><body>I\'m very angry. ' \
                  '<img src="cid:angry_gif" />.\n' \
                  'See attached document.</body></html>'

    payload, mail_from, rcpt_to, msg_id=pyzmail.compose_mail(\
            (u'Me', 'me@foo.com'), \
            [(u'Him', 'him@bar.com'), 'just@me.com'], \
            u'the subject', \
            'iso-8859-1', \
            (text_content, 'iso-8859-1'), \
            (html_content, 'iso-8859-1'), \
            attachments=[('The price of RAM modules is increasing.', \
                          'text', 'plain', 'text.txt', 'us-ascii'), ],
            embeddeds=[(angry_gif, 'image', 'gif', 'angry_gif', None), ])

    print payload

And here is the *payload*::

    Content-Type: multipart/mixed; boundary="===============1435507538=="
    MIME-Version: 1.0
    From: Me <me@foo.com>
    To: Him <him@bar.com> , just@me.com
    Subject: the subject
    Date: Fri, 02 Sep 2011 01:40:52 +0200

    --===============1435507538==
    Content-Type: multipart/related; boundary="===============0638818366=="
    MIME-Version: 1.0

    --===============0638818366==
    Content-Type: multipart/alternative; boundary="===============0288407648=="
    MIME-Version: 1.0

    --===============0288407648==
    Content-Type: text/plain; charset="iso-8859-1"
    MIME-Version: 1.0
    Content-Transfer-Encoding: quoted-printable

    I'm very angry. See attached document.
    --===============0288407648==
    Content-Type: text/html; charset="iso-8859-1"
    MIME-Version: 1.0
    Content-Transfer-Encoding: quoted-printable

    <html><body>I'm very angry. <img src=3D"cid:angry_gif" />. See attached doc=
    ument.</body></html>
    --===============0288407648==--
    --===============0638818366==
    Content-Type: image/gif
    MIME-Version: 1.0
    Content-Transfer-Encoding: base64
    Content-ID: <angry_gif>
    Content-Disposition: inline

    R0lGODlhDgAOALMAAAwMCYAAAACAAKaCIwAAgIAAgACAgPbTfoR/YP8AAAD/AAAA//rMUf8A/wD/
    //Tw5CH5BAAAAAAALAAAAAAOAA4AgwwMCYAAAACAAKaCIwAAgIAAgACAgPbTfoR/YP8AAAD/AAAA
    //rMUf8A/wD///Tw5AQ28B1Gqz3S6jop2sxnAYNGaghAHirQUZh6sEDGPQgy5/b9UI+eZkAkghhG
    ZPLIbMKcDMwLhIkAADs=
    --===============0638818366==--
    --===============1435507538==
    Content-Type: text/plain; charset="us-ascii"
    MIME-Version: 1.0
    Content-Transfer-Encoding: 7bit
    Content-Disposition: attachment; filename="text.txt"

    The price of RAM module is increasing.
    --===============1435507538==--


Scripts
-------

Binary executables for Windows of these script are available in
the `Download`_ section below.

pyzsendmail
^^^^^^^^^^^

**pyzsendmail** is a command line script to compose and send simple and complex emails.

Features:

    - **SSL**, **TLS** , **authentication**
    - **HTML** content and *embedded images*
    - **attachments**
    - *Internationalisation*

Read the :doc:`manual <man/pyzsendmail>` for more.

Under *Windows* **pyzsendmail.exe** can replace the now old `blat.exe <http://www.blat.net/>`_ and
`bmail.exe <http://www.beyondlogic.org/solutions/cmdlinemail/cmdlinemail.htm>`_.


pyzinfomail
^^^^^^^^^^^

**pyzinfomail** is a command line script reading an email
from a file and printing most important information. Mostly to show how to use
**pyzmail** library. Read the :doc:`manual <man/pyzinfomail>` for more.

License
-------

**pyzmail** iis released under the GNU Lesser General Public License ( LGPL ).

Links
-----

More links about parsing and writing mail in python

    - `formataddr() and unicode <http://tillenius.me/blog/2011/02/11/formataddr-and-unicode/>`_
    - `Sending Unicode emails in Python <http://mg.pov.lt/blog/unicode-emails-in-python.html>`_
    - `Sending Email with smtplib <http://www.blog.pythonlibrary.org/2010/05/14/how-to-send-email-with-python/>`_


..
    Not used yet
    Contents:

    .. toctree::
       :maxdepth: 2

    man/pyzsendmail


    Indices and tables
    ==================

    * :ref:`genindex`
    * :ref:`modindex`
    * :ref:`search`

