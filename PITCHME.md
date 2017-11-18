
![web censurada del referendum](http://res.cloudinary.com/zilogtastic/image/upload/c_scale,h_600/v1510847839/cat_referendum/Screen_Shot_2017-11-16_at_15.59.43.png)

---

![the referendums web](http://res.cloudinary.com/zilogtastic/image/upload/c_scale,h_600/v1510847838/cat_referendum/Screen_Shot_2017-11-16_at_15.48.07.png)

---

![github repo with mirror](http://res.cloudinary.com/zilogtastic/image/upload/c_scale,h_600/v1510851285/cat_referendum/Screen_Shot_2017-11-16_at_17.54.19.png)

---

![servidores en cloudflare](http://res.cloudinary.com/zilogtastic/image/upload/v1510847838/cat_referendum/Screen_Shot_2017-11-16_at_15.46.09.png)

---

The _OnVotar_ census website asked for 3 pieces of information:
- Your national identity number (similar to the Dutch BSN Burgerserviesnummer)
- Your date of birth
- Your postal code

This information is highly sensitive given the context. The surveillance and retaliatory powers of the Spanish state made it possible to send every voter to court.

---

How do you build an electronic census online, on the open Internet, knowing that you must protect voter's private information and knowing that the Spanish state will do everything they can to retaliate?

---

Hmmm... tricky! The _Catalonian Govern's_ digital strategy for the day of the referendum is a great study case on countermeasures for digital censorship.

---

When you click on the button labelled "Search a polling station" the site makes a `GET` request, with the following content:

```
Request URL:https://onvotar.garantiespelreferendum.com/db/c3/a1.db
Request Method:GET
Status Code:200
Remote Address:104.18.36.145:443
Referrer Policy:no-referrer-when-downgrade
Origin:null
User-Agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.113 Safari/537.36
```

The form data *is not sent* to the server! This is unsual.

---

Fetching that file in the request reveals the following:

```
1febc7fc1f60ae314b424303ea516a290f305a36c0bab4f3dbc98d810f2ee162e59878057968a22bbaa69a9dc066414f1506a8d10dfa1d73130b3ee2f4bf9037c7ab8642528c9b0c5c157b4ea0dd9bbb1815c5b2bd4519852e59c4bac040599db36127f0f52530b7f45455f9d4fa55798a9918a65391c49a0181c677da360346da22e7b0c33b05bfcbb55a4ee75820fe1f9fbdd532b40ccac4c6ba85d610679b66bad2a43fbb2a418eec58c4772a42113b3fa25052c2435c5c4a5ffe8bcaf2896d220eaa4c446f19c62b26ccefd3
e4d353eeb673c351f84276fb8c159381ff641775b9e0a17925245f39bedd33661212588b3ca926ee31a49859055f901382279a24c291076660b3e80e65de8ec66e948bc64b5120ae09dd56f955302e1323ef8afdd29143269f2b7c3a8e6c02ebc70d0c9d949092ca19246d5cba3b0c69c314bf7d7a4222326fd7b78c8768e029e4e5ef1175ed5dd457ce3fc88840f9ce941eeb2bd28521ac5eb8be1be9faf1b40ee3f2f81683be3c3a93f1b806aaa5ff7761b873a725ff470a67983d3d827bbbdfcb228c4ec505b4347945c39ab9
2fd03efea47e19d24331a4b6b0ffa9988efda433d5ad5a31c10d2b0b849ee2013e7be6ccb75336147a600c7f542b702289d843d683156797e32e360a5e96f499c0350ba3de76fbe91694463381b9ff1fbf4820ab215b03da5cf604b5523530c9d21ca7f84e150e8ad3cd08d06f05c6cf81c9e87ee49ee60d709496a1ca35a5a742992cb9736bcbf64295a0103b44c3cb7ea0e923a0a1bd3540abb6a0ec78bf56f94967b2ec92ab8000bec75d4dddcb12f0651d062693c068714cd449e45692eb3ef3821db6c1302ff5e57d572d1d
...
```
This is some kind of encoded database. Not entirely clear what it is. Things are getting weird now.

---

The `index.html` file reveals what your browser does next:

```
<script type="text/javascript" src="../js/bundleV9.js"></script>
<script type="text/javascript">
function calcula() {
  document.querySelector("#stepLoad").style.display="inline"
  var dni = document.querySelector("#dni").value
  dni = dni.replace(/ /g,'').replace(/-/g,'').replace(/\./g,'');
  var dn_dia = document.querySelector("#dn_dia").value
  var dn_mes = document.querySelector("#dn_mes").value
  var dn_any = document.querySelector("#dn_any").value
  var cp = document.querySelector("#cp").value
  if (dn_dia.length == 1)
     dn_dia = "0"+ dn_dia
  if (dn_mes.length == 1)
     dn_mes = "0"+ dn_mes
  if (dn_any.length == 2)
     dn_any = "19" + dn_any
  onvoto.calcular(dni.slice(-6),dn_any+dn_mes+dn_dia,cp, function(i) {
    document.querySelector("#stepLoad").style.display="none"
    if (( typeof i === 'string' ) && ( i == 'not-found' ))  {
        document.querySelector("#response").innerHTML = "No s'ha trobat cap registre corresponent a les dades introduïdes."
    } else {
       document.querySelector("#stepOne").style.display="none";
       document.querySelector("#response").innerHTML = i[0] + "\n" + i[1] + "\n" + i[2] + "\n\n"
                                                     + "Districte:" + i[3] + "\n" + "Secció:" + i[4] + "\n"
                                                     + 'Mesa: ' + i[5];
    }
   })
}
</script>
```
This code seems to try to use your personal data to create some kind of key and that key is used again later in the code [...]

---

```
var calcular = function (d, dn, cp, cb){
    ...
    var key = dni + data_naixement + codi_postal;
    var firstSha256 = hash(bucleHash(key,BUCLE));
    var secondSha256 = hash(firstSha256);
    var dir = secondSha256.substring(0,DIRSHA);
    var file = secondSha256.substring(DIRSHA,DIRSHA+FILESHA);
    var url = window.location.href.replace('/index.html','/');
    ...
    url += '../db/' +dir + "/" + file + FILEEXTENSION;

    request( url, function (error, response, body) {
    ...
```
This calculates a _shard_ number. This means that they split the database into multiple tiny databases called _shards_.

---

The number of shards indicates that there are about 65000 of them, and each shard contains the personal data of about 70 Catalonians elligible to vote. 70 times 65000 is about 4.5 million, which seems about correct. 4.5 million people in Catalonia are elligible to 
vote.

---

Each _shard_ is encrypted, using the voter's own personal data hashed 17 times. It would take a very fast desktop computer about 31.000 years to _brute force_ that hash. A state like Spain can do it in much less, *but the expense is considerable*. If the police did break it (as they claimed on the press days later without any evidence), it probably cost the Spanish tax payer a lot of money to break that encryption.

---

## Where does all this data come from?

Although the precise details aren't really publicly known, here are some verifiable clues.

---

![santi vidal](http://res.cloudinary.com/zilogtastic/image/upload/c_scale,h_600/v1510850104/cat_referendum/Screen_Shot_2017-11-16_at_17.28.18.png)

---

13 September 2017, a judge instructing the cause against Santi Vidal gave national telecoms 24h to block the 10 known websites hosting the website of the Catalonian Referendum

---

![github repo of whatsapp bot](http://res.cloudinary.com/zilogtastic/image/upload/c_scale,h_600/v1510852160/cat_referendum/Screen_Shot_2017-11-16_at_18.08.46.png)

---

Website was Grade A according to SSLlabs, this is the highest ranking and certifies a proper implementation of HTTPS.

---

![estonia](http://res.cloudinary.com/zilogtastic/image/upload/v1510848703/cat_referendum/screencapture-politica-elpais-politica-2017-10-13-actualidad-1507916636_098849-html-1510848636384.png)

---

![telegram bot](http://res.cloudinary.com/zilogtastic/image/upload/c_scale,h_600/v1510847838/cat_referendum/Screen_Shot_2017-11-16_at_15.57.06.png)

---

![papeleta en google drive](http://res.cloudinary.com/zilogtastic/image/upload/c_scale,h_600/v1510847838/cat_referendum/Screen_Shot_2017-11-16_at_15.48.44.png)

---

![encouraging people to send the apps APK package via whatsapp](http://res.cloudinary.com/zilogtastic/image/upload/c_scale,h_600/v1510847839/cat_referendum/Screen_Shot_2017-11-16_at_15.54.15.png)

---

With a website, several hundred domains, servers in Spain and abroad, a Whatsapp bot, a Telegram bot and a self-dispatching census app as APK package. The _Govern's_ digital strategy is the most "all out" and overt digital campaign I am aware of.

---

## Telecoms
#### How telecoms implemented the order to censor the referendum

---

## DNS blokade 

This was what most telecoms did: Orange and other telecoms that use their network (Jazztel, SomConnexió. As well as other telecoms with their own network Euskaltel, Vodafone.

---

### Remember #direntwitter
![#direntwitter](http://res.cloudinary.com/zilogtastic/image/upload/c_scale,h_600/v1510850656/cat_referendum/Alternative_DNS.jpg)

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

- https://en.wikipedia.org/wiki/2017_Spanish_constitutional_crisis
- https://twitter.com/censura1oct?lang=en
- http://pirata.cat/bloc/evidencia-de-censura-en-internet-durante-el-referendum-de-independencia-de-cataluna/
- http://www.entredevyops.es/posts/referendum-votar.html
- https://censura1oct.github.io/en/2017/09/16/methods_en.html

---
### You can find these materials here

- Slides - https://gitpitch.com/dropmeaword/censorship_cat_1oct#/
- Sources - https://github.com/dropmeaword/censorship_cat_1oct
