:orphan:

pyzsendmail manual page
=======================

Synopsis
--------

**pyzsendmail** [*options*]

Description
-----------

**pyzsendmail** compose and send mails.

Compose an email including *text* and/or *HTML* content, add *attachments*
of any kind, or *embed* images in the HTML content. Depending the need,
**pyzsendmail** adapts the MIME structure of the email.

**pyzsendmail** handle SSL, TLS and authentication.

**pyzsendmail** can send to one or multiple recipients, but also support *CC*
and *BCC* recipients.


Options
-------

.. program:: pyzsendmail

.. option:: -h, --help

    show this help message and exit

.. option:: -H name_or_ip, --smtp-host=name_or_ip

    SMTP host relay

.. option:: -p port, --smtp-port=port

    SMTP port (default=25)

.. option:: -L login, --smtp-login=login

    SMTP login (if authentication is required)

.. option:: -P password, --smtp-password=password

    SMTP password (if authentication is required)

.. option:: -m mode, --smtp-mode=mode

    smtp mode in 'normal', 'ssl', 'tls'. (default='normal')

.. option:: -A charset, --arg-charset=charset

    command line arguments charset (default=<depend of the host locals>)

.. option:: -C charset, --mail-charset=charset

    mail default charset (default=<depend of the host local>)

.. option:: -f sender, --from=sender

    sender address

.. option:: -t recipient, --to=recipient

    add one recipient address

.. option:: -c recipient, --cc=recipient

    add one CC address

.. option:: -b recipient, --bcc=recipient

    add one BCC address

.. option:: -s subject, --subject=subject

    message subject

.. option:: -T text, --text=text

    text content in the form
        [text_charset]:@filename
    or
        [text_charset]:"litteral content"

.. option:: -M html, --html=html

    html content in the form
        [text_charset]:@filename
    or
        [text_charset]:"literal content"

.. option:: -a file, --attach=file

    add an attachment in the form:
        maintype/subtype:filename:target_file[:text_charset]
    for example
        image/jpg:picture.jpg:thepicture.png
    or
        image/jpg:picture.jpg:C:\\thepicture.png:
        (notice the trailing ':' to disambiguate the : of the drive letter)
    or
        text/plain:file.txt:C:\\report.txt:windows-1252

.. option:: -e file, --embed=file

    add embedded data in the form:
        maintype/subtype:content-id:target_file[:text_charset]
    for example
        image/jpg:picture:thepicture.png

.. option:: -E, --eicar

    include eicar virus in attachments, for testing Anti-virus

Arguments
---------

**login** and **password** must be *utf-8* encoded if they contains non *us-ascii*
    characters.

**address** can be of the form:
    \"Foo Bar <foo.bar\@example.com>\"

    **or**

    \"foo.bar\@example.com\"

    Name can contain non us-ascii characters. They are supposed to use the
    command line charset encoding.

**text** and **HTML** content can be in the *literal* form:
    - :\"The text content\"
    - utf8:\"The text content\"

    In both samples, the content is encoded using the *encoding* of the
    command line argument. In first sample, notice the **:** at beginning,
    the content will be encoded using the mail default charset,
    in the second sample, the text will be re-encoded into utf8.

    **or** using content of a *file*:

    - :@"C:\\file.txt"
    - windows-1252:@"C:\\file.txt"

    In first sample, notice the **:** at beginning, the file is supposed to be
    encoded using mail default charset and will be encoded this way in the
    email. In second sample, the file is supposed to be encoded using
    *windows-1252* charset and will be encoded this way in the email.

**attachment** and **embedded** files
    In attachment and embedded content, the *text_charset* is used only if the
    *maintype* is \'text\'. The file is supposed to be encoded using
    *text_charset* and will be encoded using this charset in the email.

Samples
-------

::

    pyzsendmail -H localhost -p 25 -f "Me <me\@example.com>" -t "foo\@example.com" -t "Bar <bar\@example.com" -s "The subject" -T :"Hello" -a image/jpg:holiday.png:C:\\Holiday.png:


See also
--------

:manpage:`pyzmailinfo(1)`.

Author
------

Alain Spineux <alain.spineux@gmail.com>

