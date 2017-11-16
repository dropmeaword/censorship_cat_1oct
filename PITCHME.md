---
![santi vidal](http://res.cloudinary.com/zilogtastic/image/upload/v1510850104/cat_referendum/Screen_Shot_2017-11-16_at_17.28.18.png)
---
16 September 2017, a judge instructing the cause against Santi Vidal gave national telecoms 24h to block the 10 known websites hosting the website of the Catalonian Referendum
---

![web del referendum](http://res.cloudinary.com/zilogtastic/image/upload/v1510847839/cat_referendum/Screen_Shot_2017-11-16_at_15.59.43.png)

---

![estonia](http://res.cloudinary.com/zilogtastic/image/upload/v1510848703/cat_referendum/screencapture-politica-elpais-politica-2017-10-13-actualidad-1507916636_098849-html-1510848636384.png)

---

![telegram bot](http://res.cloudinary.com/zilogtastic/image/upload/v1510847838/cat_referendum/Screen_Shot_2017-11-16_at_15.57.06.png)

---

![papeleta en google drive](http://res.cloudinary.com/zilogtastic/image/upload/v1510847838/cat_referendum/Screen_Shot_2017-11-16_at_15.48.44.png)

---

![servidores en cloudflare](http://res.cloudinary.com/zilogtastic/image/upload/v1510847838/cat_referendum/Screen_Shot_2017-11-16_at_15.46.09.png)

---

![encouraging people to send the apps APK package via whatsapp](http://res.cloudinary.com/zilogtastic/image/upload/v1510847839/cat_referendum/Screen_Shot_2017-11-16_at_15.54.15.png)

---

![the referendums web](http://res.cloudinary.com/zilogtastic/image/upload/v1510847838/cat_referendum/Screen_Shot_2017-11-16_at_15.48.07.png)

---
## Telecoms
#### How telecoms implemented the order to censor the referendum

---
## DNS blokade 

This was what most telecoms did: Orange and other telecoms that use their network (Jazztel, SomConnexió. As well as other telecoms with their own network Euskaltel, Vodafone.
---
### Remember #direntwitter
![#direntwitter](http://res.cloudinary.com/zilogtastic/image/upload/v1510850656/cat_referendum/Alternative_DNS.jpg)
---
## Movistar
#### Telefonica's mobile network.

They used a primitive form of DPI (deep packet inspection).
	- For regular HTTP connections they used hostname and IP blacklists.
	- For HTTP*S* connections they look up the field SNI (unencrypted hostname in the TLS message) and the IPs.

![movistar DPI](http://res.cloudinary.com/zilogtastic/image/upload/v1510850566/cat_referendum/movistar-wireshark-mitm.png)

---
### Parlem telefonia
They announced that no content would be censored on their network

---
### More information

https://twitter.com/censura1oct?lang=en
http://pirata.cat/bloc/evidencia-de-censura-en-internet-durante-el-referendum-de-independencia-de-cataluna/

---
### You can find these materials here

Slides - https://gitpitch.com/dropmeaword/censorship_cat_1oct#/
Sources - https://github.com/dropmeaword/censorship_cat_1oct