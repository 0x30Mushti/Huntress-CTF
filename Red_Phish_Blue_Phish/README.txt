Starta en osint på mail-adressen swilliams@pyrchdata.com --> pyrchdata.com. 

För att phising mailet skall gå igenom behöver man sätta Sarah i en knipig sits. 

Rent teoretiskt kan man göra olika t.ex:

	- Skicka som CEO eller annan person på pyrchdata som har mandat.

	- Skicka en gediget bra phisingmail (innehåll) som är trovärdigt. 
	
	- Skicka ett hot-mejl / ransommail.

Efter lite errors så gick taktiken till att framstå som en annan person. 

Jag valde "Joe Daveren" som är IT Security Manager på pyrchdata (https://pyrchdata.com/team)


Så hur gör man:

Telnet host portnummer. 

HELO pyrchdata.com
MAIL FROM:jdaveren@pyrchdata.com
RCPT TO:swilliams@pyrchdata.com
DATA
Subject: IT Security warning, you need to reset your password.
Hello Sarah, your password have been leaked on the darkweb amd you will have to reset your password URGENT. You can find the password reset link here.

.

250 OK. flag{54c6ec05ca19565754351b7fcf9c03b2}