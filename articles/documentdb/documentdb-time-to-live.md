<properties
   pageTitle="Aegumise DocumentDB andmete reaalajas aega | Microsoft Azure'i"
   description="TTL, pakub Microsoft Azure'i DocumentDB võimalus dokumendid automaatselt kaustadest süsteemi pärast teatud aja jooksul."
   services="documentdb"
   documentationCenter=""
   keywords="reaalajas aeg"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Aegumise DocumentDB saidikogumite automaatselt koos aega reaalajas andmed

Rakendusi saate aedvili ja suure hulga andmete talletamiseks. Osa andmeid, nt arvuti loodud sündmuse andmeid, logid ja kasutajale seansi teave on kasulik ainult piiratud aja jooksul. Pärast andmete muutub liigsete vajadustele rakendus on ohutu likvideerite andmed ja vähendamiseks rakenduse salvestusruumi vajadustele.

"Aeg live" või TTL, pakub Microsoft Azure'i DocumentDB võimalus dokumendid automaatselt kaustadest andmebaasi pärast teatud aja jooksul. Vaikimisi time to live saate saidikogumi tasemel ja alistada kohta dokumendi alusel. Kui TTL on määratud, kas saidikogumi vaikimisi või dokumendi tasemel automaatselt eemaldada DocumentDB dokumendid, mis on olemas pärast selle aja jooksul sekundites, kuna need on muudetud.

Aega elavad DocumentDB kasutab mõnda nihke suhtes dokumenti viimati muudetud. Selleks kasutab _ts välja, mis on olemas kõik dokumendid. Väli _ts on unix laadis epohhi ajatempli tähistav kuupäev ja kellaaeg. Väli _ts värskendatakse iga kord, kui dokumenti muudetakse. 

## <a name="ttl-behavior"></a>TTL käitumine

Funktsiooni TTL kontrollib TTL atribuudid kahel viisil – saidikogumi tasemel ja dokumendi tasemel. Väärtused määratakse sekundites ja käsitletakse delta dokumenti viimati muudeti veebisaidil _ts.

 1.  DefaultTTL kogumiseks
  * Kui puuduvad (või Sea väärtuseks null), dokumente ei kustutata automaatselt.
  
  * Kui esineb ja väärtus on "-1" = lõpmatu – dokumente ei aeguks vaikimisi
  
  * Kui esitus ja väärtus on mõni arv ("n") – dokumentide aegumise "n" sekundit pärast viimast muutmist

 2.  TTL dokumentide: 
  * Atribuut saab rakendada ainult siis, kui DefaultTTL esineb ema saidikogumi jaoks.
  
  * Alistab ema saidikogumi DefaultTTL väärtuse.

Kui dokument on aegunud (ttl + _ts > = Serveri praeguse kellaaja), "aegunud" on märgitud dokument. Pole toiming on lubatud need dokumendid selle aja ja välistatakse küsimusi sooritatud tulemustele. Dokumentide kustutatakse füüsilise süsteemi ja kustutatakse taustal opportunistically hiljem. See ei hõlma [Taotlemine üksused (RUs)](documentdb-request-units.md) eelarvest saidikogumi.

Ülaltoodud loogika saate näidata maatriksi järgmine:

|       | DefaultTTL puuduvad ei pannud kogumine | DefaultTTL = -1 kogumine | DefaultTTL = "n" saidikogumi|
| ------------- |:-------------|:-------------|:-------------|
| TTL puuduvad dokumendis| Midagi dokumendi tasemel alistada, kuna nii dokumendis kui ka saidikogumi ei ole mõiste TTL. | Selle saidikogumi pole üldse dokumente aegub. | Selle saidikogumi dokumentidele aegub kui intervall n saabumisega. |
| TTL = -1 dokumendis | Midagi, kuna selle saidikogumi tasemel dokumendi alistamiseks ei määratleda DefaultTTL atribuudi, saate dokumendi alistada. TTL dokumendis on UN tõlgendada süsteem. | Selle saidikogumi pole üldse dokumente aegub. | Dokumendi TTL =-1 selle kogumi kunagi aegub. Kõik muud dokumendid aegub pärast "n" intervalli. |
|  TTL = n dokumendis | Midagi alistamiseks dokumendi tasemel. Klõpsake dokumendi TTL un tõlgendada süsteem. | Dokumendi TTL = n aegub pärast intervall n, sekundites. Muude dokumentide ei päri intervall-1 ja aegumatuks. | Dokumendi TTL = n aegub pärast intervall n, sekundites. Muude dokumentide pärivad "n" intervall kogumisest. |


## <a name="configuring-ttl"></a>TTL konfigureerimine

Vaikimisi on keelatud aega live vaikimisi kõik DocumentDB saidikogumite ja kõik dokumendid.

## <a name="enabling-ttl"></a>TTL lubamine

TTL kogumi või kogumi dokumentideks lubamiseks peate kogumi DefaultTTL atribuudi, kas -1 või null positiivne arv. Sätte on DefaultTTL-1 tähendab, et kõik selle saidikogumi dokumentide elavad terve igavik vaikimisi, kuid selle DocumentDB teenuse jälgima selle saidikogumi jaoks dokumente, mis on see vaikimisi alistada.

## <a name="configuring-default-ttl-on-a-collection"></a>Vaike-TTL kogumi konfigureerimine

Teil on võimalik Vaikeaja Live'i saidikogumi tasemel konfigureerida. 

Kogumi TTL määramiseks peate pakuvad null positiivne arv, mis näitab aeg, sekundites aegumatuks kõigi dokumentide kogumine viimati muudetud ajatempli pärast dokumendi (_ts).

Või saate seada vaike -1, mis tähendab, et kõik dokumendid kogumi lisada elavad lõputult vaikimisi.

## <a name="setting-ttl-on-a-document"></a>Sätte TTL dokumendis

Vaikimisi TTL kogumi kohta, Lisaks saate seada teatud TTL dokumendi tasemel. Sellega alistab vaikimisi kogumist.

Määrake TTL dokumendiga, peate esitada null positiivne arv, mis näitab aeg, sekundites aegumatuks dokumenti viimati muudetud ajatempli dokument (_ts) pärast.

See aegumise offset seadmine väljal TTL dokumendiga.

Kui dokument on TTL väli puudub, rakendatakse vaikimisi kogumist.

Kui TTL on keelatud saidikogumi tasemel, ignoreeritakse TTL välja dokumendiga kuni TTL on lubatud uuesti kogumist.


## <a name="extending-ttl-on-an-existing-document"></a>Olemasoleva dokumendi TTL laiendamine

Saate lähtestada TTL dokumendiga, tehes dokumendi tehte kirjutamine. Seejuures seab selle _ts praeguse kellaaja ja selle dokumendi lõppu, on määranud ttl, loendust alustab uuesti.

Kui te ei soovi muuta dokumendi ttl, saate värskendada välja nagu saate teha kõigest välja.


## <a name="removing-ttl-from-a-document"></a>TTL eemaldamine dokumendist

Kui TTL on määratud dokumendi ja te ei soovi enam selle dokumendi kaoks, siis saate tuua dokumendi, TTL välja eemaldada ja asendada dokumendi serveris.

Väljal TTL eemaldamisel dokumendist rakendatakse vaikimisi kogumist.

Dokumendi aeguv lõpetada ja selle saidikogumi päri siis peate seatud TTL väärtuse -1.


## <a name="disabling-ttl"></a>TTL keelamine

TTL keelamine täiesti kogumi ja tausta otsite aegunud dokumentide DefaultTTL atribuut kogumise protsessi lõpetamine tuleks kustutada.

Selle atribuudi kustutamisel erineb sätteks -1. Atribuudi-1 tähendab uusi dokumente lisada kogumist elavad terve igavik, kuid võite ignoreerida teatud dokumentidega kogumist.

Selle atribuudi täiesti eemaldamine kogumine tähendab, et aegub pole üldse dokumente, isegi juhul, kui dokumente, mille olete konkreetselt alistada eelmise vaikimisi.


## <a name="faq"></a>KKK

**Mis on TTL mulle maksab?**

On täiendavaid tasuta säte TTL dokumendiga.

**Kui kaua võtab kustutamiseks dokumendi kui TTL on üles?**

Dokumentide märgitakse saadaval, kui dokument on aegunud (ttl + _ts > = Serveri kellaaeg). Pole toiming on lubatud need dokumendid on pärast seda ja välistatakse küsimusi sooritatud tulemustele. Dokumentide füüsilise kustutatakse süsteem taustal. See pole tarbib kõik selle saidikogumi eelarvest RUs.

**Kui see võtab teatud aja jooksul kustutamiseks dokumente, nad lähevad minu kvoodi (ja arve) seni, kuni need kustutada?**

Ei, kui dokument on aegunud teile pole arveid nende dokumentide säilitamiseks ja dokumentide suurust ei arvestata kogumise talletusmahu piirmäära poole.

**TTL dokumendis on mis tahes mõju RU kulude?**

Ei, tekib ei mõjuta RU erinevatest iga dokumendi sees DocumentDB toimingutest.

**Kas dokumentide mõju läbilaskevõime, mul on ette valmistatud sisse oma saidikogumi kustutamisel?**

Ei, serveeritakse taotlusi vastu teie saidikogumi saate prioriteet üle kustutamiseks dokumendi tausta protsess. Iga dokumendi TTL lisamine ei mõjuta see.

**Dokumendi aegumisel kaua see jääb minu saidikogumi kuni kustutamiseni?**

Kui dokument aegub see pole enam juurdepääsetav. Täpse kellaaja dokumendi jäävad teie saidikogumi enne tegelikult kustutatud on tarkadeks ja põhineb kui taust on võimalik kustutada dokumendi.

**Aegunud dokumendid kustutatakse kõik sõlmed üle või oleks "lõpuks consistent?"**

Dokumendi muutub saadaval korraga üle kõik sõlmed ja kõikides piirkondades.

**Kas kasutada RU eest TTL jälgimine tausta ülesannete jaoks?**

Ei, on see eest maksma RU.

**Kui sageli TTL expirations kontrollida?**

TTL expirations kontrollimine ei juhtu, kui taust protsessi. Taotluse vastates kirjutamata teenuse tehke kontrollib teksti sees ja välistada kõik dokumendid, mis on aegunud. Füüsilise dokumendi kustutamise on ainult asünkroonselt töötab taustal. Selle protsessi sagedus määratakse saadaval Venemaa kogumist.

**Kas funktsiooni TTL rakenduvad ainult kogu dokumentide või saate ma aegumise dokumendi kinnisvarahindade?**

TTL kehtib terve dokument. Kui soovite lihtsalt dokumendi osa kaoks, siis on soovitatav, et osa eraldi "lingitud" dokumendi ekstraktida põhidokumenti sisse ja kasutage seejärel TTL ekstraktitud dokument.

**Kas funktsiooni TTL on erinõuetega, indekseerimise?**

Jah. Selle saidikogumi peab olema kas Lazy või järjepidev [indekseerimine poliitika määramine](documentdb-indexing-policies.md) . Proovite DefaultTTL pannud kogumik indekseerimine on seatud pole tulemuseks viga, kui proovite indekseerimise kogumi, mis sisaldab DefaultTTL, mis on juba välja lülitada.


## <a name="next-steps"></a>Järgmised sammud

Azure'i DocumentDB kohta lisateabe saamiseks vaadake [*dokumentatsiooni*](https://azure.microsoft.com/documentation/services/documentdb/) teenus.




