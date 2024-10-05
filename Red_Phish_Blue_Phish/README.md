# Red Phish Blue Phish

## Description
```
You are to conduct a phishing excercise against our client, Pyrch Data.

We've identified the Marketing Director, Sarah Williams (swilliams@pyrchdata.com), as a user susceptible to phishing.

Are you able to successfully phish her? Remember your OSINT ;)
```

## OSINT Analysis on Email Address

**Start an OSINT investigation on the email address** swilliams@pyrchdata.com **→ pyrchdata.com.**

To ensure the phishing email gets through, we need to put Sarah in a tricky situation. 

Theoretically, there are several approaches one could take:

- **Impersonate the CEO or another authoritative figure at Pyrchdata.**
- **Craft a highly convincing phishing email with credible content.**
- **Send a threatening email or ransom note.**

After some trial and error, the strategy evolved to impersonate another person.

I chose to impersonate **Joe Daveren**, the IT Security Manager at Pyrchdata (find more about him [here](https://pyrchdata.com/team)).

## Here’s how to proceed:

Use **Telnet** to connect to the host and port number.
  
telnet host port_number

```
HELO pyrchdata.com
MAIL FROM:jdaveren@pyrchdata.com
RCPT TO:swilliams@pyrchdata.com
DATA
Subject: IT Security warning, you need to reset your password.
Hello Sarah, your password have been leaked on the darkweb amd you will have to reset your password URGENT. You can find the password reset link here.

.
```
250 OK. flag{REDACTED}


