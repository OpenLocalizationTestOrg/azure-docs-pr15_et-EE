<properties 
   pageTitle="Azure'i mobiilsideseadmete kaasamine Tõrkeotsingujuhendi - teenus" 
   description="Tõrkeotsingu juhised Azure mobiilsideseadmete kaasamine" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-service-issues"></a>Tõrkeotsingujuhendi teenuseprobleemide otsimine

Järgnevalt on võimalikud probleemid võivad ilmneda koos Azure Mobile kaasamine töö.

## <a name="service-outages"></a>Teenuse katkestuste

### <a name="issue"></a>Probleem
- Probleemid, mis kuvatakse olevat põhjustatud Azure Mobile kaasamine teenuse katkestuste.

### <a name="causes"></a>Põhjused
- Probleemid, mis kuvatakse olevat põhjustatud Azure Mobile kaasamine teenuse katkestuste võib olla põhjustatud mitut tüüpi probleemid:
    - Isoleeritakse probleemid, mis kuvatakse algselt süsteemne kõigile Azure Mobile kaasamine
    - Teadaolevad probleemid tingitud serveri katkestuste (mitte alati näitab serveri olek):
    - Kavandamine viivitused, sihtimine tõrkeid, märgi update probleemid statistika Peata kogumiseks, Push ei tööta, API Peata töö, uue rakendused või kasutajad ei saa luua, DNS-i tõrked ja ajalõpp tõrgete UI, API või rakenduste seadmes.
    - Pilveteenuse sõltuvus katkestuste [Azure teenuse olek](http://status.azure.com/)
    - Tõuketeatised teatis Services (PNS) sõltuvus katkestuste
    - Rakenduste poest katkestuste

1) Kas probleem on süsteemne testimiseks saate testida erinevat sama funktsiooni
   
   - Azure'i Mobile kaasamine integreeritud rakenduste
   - Test seade
   - Test seadme OS versioon
   - Turunduskampaania
   - Administraatori kasutajakonto
   - Brauseri (st, Firefox, Chrome jne)
   - Arvuti

2) Kui probleem mõjutab ainult UI või selle API testimiseks tehke järgmist.

   - Testige Azure Mobile kaasamine Kasutajaliidese ja Azure Mobile kaasamine API sama funktsiooni.

3) Kui probleem on mobiiltelefon võrgu testimine

   - Testige ajal Interneti kaudu Wi-Fi- ja 3G mobiiltelefon võrgu kaudu ühendatud.
   - Veenduge, et teie tulemüür on blokeerimine Azure Mobile kaasamine IP-aadresside ja portide.

4) Kui probleem on seadmega testimiseks tehke järgmist.

   - Kui teie seade on Azure Mobile kaasamine ühendust luua teise Azure Mobile kaasamine integreeritud rakenduse testida.
   - Test, mida saate luua sündmuste, töö ja jookseb telefonist, et näha Azure Mobile kaasamine UI. 
   - Kui saate saata Tõuketeatiste Azure Mobile kaasamine kasutajaliidese seadme põhjal seadme ID. 

5) Kui probleem on oma rakendusega testimiseks tehke järgmist.

   - Installimine ja testige oma rakenduse emulaator asemel füüsilise seadme kaudu.
   
6) Kui probleem on OS uuendamine, et lõppkasutaja seadmetes, mis nõuavad lahendamiseks versiooniuuenduse SDK testimiseks tehke järgmist.

   - Katsetage erinevaid versioone OS erinevate seadmete rakenduse.
   - Veenduge, et kasutate SDK uusima versiooni.
 
## <a name="connectivity-and-incorrect-information-issues"></a>Ühenduvus ja vale teabe probleemid

### <a name="issue"></a>Probleem
- Logige Azure Mobile kaasamine UI probleeme.
- Azure Mobile kaasamine API ühenduse tõrkeid.
- Rakenduse teave siltide API seadme kaudu üles probleeme.
- Azure'i Mobile kaasamine logid või eksporditud andmete allalaadimisega probleeme.
- Vale teave Azure Mobile kaasamine UI.
- Vale teabe näidatud Azure Mobile kaasamine logid.

### <a name="causes"></a>Põhjused
* Veenduge, et teie kasutajakontol on selle toimingu sooritamiseks piisavaid õigusi.
* Veenduge, et probleem ei ole isoleeritakse ühes arvutis või teie kohalikus võrgus.
* Veenduge, et et Azure Mobile kaasamine teenus ei ole teatatud katkestuste.
* Veenduge, et rakendus teave sildi failide järgige kõiki järgmisi reegleid:
    - Kasutage ainult UTF8 märgistiku (ANSI-märgistiku ei toetata).
    - Kasutada komasid "," nagu eraldusmärk (saate avada teenuse päringut muuta CSV eraldusmärk semikoolon "," muu märgiga, nt semikooloniga ";").
    - Kasutage kõigi väiketähtedes kahendmuutujaga väärtuste "true" ja "false".
    - Kasutage faili, mis on väiksem kui 35MB mahu ülempiir.
 
