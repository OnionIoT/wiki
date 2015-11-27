# Sending Emails With the Omega using Python

Since the Omega runs Linux, you can run an SMTP client on it and get it to send emails. For example, you can use this feature on connected hardware projects like a switch or a door bell, it can email you if there's any new activity.

## Step 1: Install mailsend

```
opkg update
opkg install mailsend
```

## Step 2: Send email with the command line with `mailsend`

This step assumes that you have setup an SMTP server or have access to services such as Amazon's Simple Email Service (SES).

```
mailsend -to recipient@email.com -from sender@email.com -ssl -port 465 -auth-login -smtp email-smtp.us-east-1.amazonaws.com -sub test +cc +bc -v -user smtp_username@smtp_server.com -pass “p@ssw0rd” -M "Your message here"
```

## Step 3: Sending Emails via Python Script

### Install Python

```
opkg update
opkg install python
```

### Write the script to send an email

```python
#!/usr/bin/python

import smtplib

sender = 'youruserid@yourhost.com'
toaddrs = 'recipient@gmail.com'

message = """From: Onion Omega Onion@onionomega.com
To: Recipient < recipient@gmail.com >
Subject: SMTP e-mail test

This is a test e-mail message.
"""

# Credentials
password = 'yourpasswordhere'

# The actual mail send
server = smtplib.SMTP_SSL('email-smtp.us-east-1.amazonaws.com:465')		# (‘Host:Port’)
server.login(sender,password)
server.sendmail(sender, toaddrs, message)
server.quit()

print "done"
```

## Links:

http://www.tutorialspoint.com/python/python_sending_email.htm

**Another example to send email using SSL via python**

http://stackoverflow.com/questions/24672079/send-email-using-smtp-ssl-port-465

## Credit

Thanks to Shamyl Mansoor for writing this awesome tutorial!