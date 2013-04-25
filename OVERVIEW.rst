Here is some basic documentation on how to use python-mailer.

You probably want to skim this once before doing even the Pre-setup.

Pre-setup
---------

Do the following::

  git clone https://github.com/paulproteus/python-mailer.git
  cd python-mailer

Setup: email connection
-----------------------

Let me talk briefly about a few different settings and what they mean. These are
all within config.py.

-------------------------
Email connection settings
-------------------------

In config.py, notice:

* SMTP_HOST
* SMTP_PORT

These are how you connect to a SMTP server to actually send the emails it
generates. The tool is too simplistic to know how to authenticate to a SMTP
server.

Therefore there are two ways to make this code work nicely.

------------
The easy way
------------

Run it on a computer with a properly-configured local SMTP server. For example,
SSH into rose.makesad.us or linode.openhatch.org before doing the 'git clone'.

--------------------------------------
The easy way, with SSH to help you out
--------------------------------------

Run it on whatever computer you want, and use SSH to create a temporary tunnel
while you actually do the sending.

For example, 'git clone' to your laptop, and also in a new terminal window run::

 $ ssh -L 1125:localhost:25 rose.makesad.us

In that case, set SMTP_PORT to 1125 and SMTP_SERVER to localhost.

--------------------------
The approximately hard way
--------------------------

Install something like 'msmtp' on your computer, and configure it to do some sort of
smart thing where it sends emails out via your e.g. Gmail or other ISP SMTP service.
Then msmtp does the authentication.

I have never tested doing it this way.

Setup: Recipients
-----------------

----------------------
Where emails come from
----------------------

In config.py, notice:

* FROM_NAME
* FROM_EMAIL

This is where your emails will appear to come from.

If you don't want to be me (Asheesh) then set it to something else.

NOTE: I recommend setting it to something not @gmail.com, unless you want to spend
a bunch of time testing if gmail will detect that as a forgery. An @openhatch.org
email address should work fine.

-----------------------------
Where emails go, in test mode
-----------------------------

In config.py, notice:

* TEST_RECIPIENTS

This is a list of people that will receive emails when you send them in 'test'
mode. Again, if you don't want Asheesh to get these, change that to be your
own email address.

---------------------------------------
Where emails get BCC:d to, in live mode
---------------------------------------

In pymailer.py you will find a Python list entitled::

 recipient_list_with_bcc

I recommend removing the Asheeshy email address from that list and replacing it with
your own. That way, you'll get a copy of every email. This means your mailbox will
get a lot of mail, but on the bright side you'll know what all you sent, and threading
(when people reply to you) will work properly.

How to use with a Google Docs spreadsheet
-----------------------------------------

-----------------------
Step 1. Create your CSV
-----------------------

In Google Docs, do something like File->Export as CSV.

Try to do something so you have the right columns, as documented in README.rst.

Tips:

* First column is the name, the way you will use it. I personally tend to manually edit the names we are given to remove stuff that seems like last name, so that way people get an email like "Hi Asheesh," not "Hi Asheesh Laroia,"

* Second column is the email address.

* There should be no header line.

-----------------------------
Step 2. Write your email text
-----------------------------

Even though python-mailer is designed for sending HTML emails,
I use it to send plain text emails. To do that, I draft the email
in nano like so::

 $ nano -r 72 mymail.txt

"-r 72" sets the maximum line width to 72 characters.

You can use the following placeholders in the message:

* <!--name-->

Actually, that's the only placeholder you can use. It gets replaced with the
name of the person from the CSV file.

-----------------------------------------
Step 3. Send actual emails, testing first
-----------------------------------------

Like README says::

 $ ./pymailer -t mymail.txt email-addresses.csv 'Email Subject Line'

Go look at it in your inbox.

If you like how it looks, replace "-t" with "-s" and it'll be really sent!


Other things to know
--------------------

When sending emails, the thing waits 0.25 seconds between emails.
