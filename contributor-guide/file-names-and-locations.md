<properties title="" pageTitle="Failide nimed ja Azure tehnilise artiklite asukohad" description="Failistruktuur artikleid ja nimereeglid, peate järgima kui loote uue artiklis selgitatakse." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>Failide nimed ja Azure tehnilise artiklite asukohad

Meie tehnilist sisu hoidla, kasutame kõigi artiklid ühte kausta ( **artiklid** kausta). Puudub kausta hierarhia – kõik artiklid live lamefaili struktuuri. Kui loote artiklite neid kaustu, ei saa avaldada oma artikleid.

Asemel faili struktuur on korraldamise põhimõte, kasutame range faili nimereeglistik mis tuvastab selgelt teemade ja mis aitab leitavus veebis.

Siin on, mida on vaja teada:

+ [Reeglid]
+ [Mustri]
+ [Standardse näited]
+ [Turuplatsi sisu]
+ [Faili nimi kinnitamine]

##<a name="rules"></a>Reeglid

- Failide nimed võivad sisaldada ainult väiketähed, numbreid ja sidekriipse. 
- Tühikute või kirjavahemärke. Kaks sidekriipsu abil sõnade ja numbritega faili nimi.
- Rohkem kui 80 märki – see on avaldamise süsteemi limiit
- Kasutamine toimingu verbide, mis on omased, nt töötada, osta, koostada, tõrkeotsing. Ole - se sõnu.
- Ei sisalda ühtegi väike sõna - a ja, tekstivormingus või, jne.
- Kõik failid peavad olema allahindlusest ja kasutada .md faililaiendit.
- Õigekirja sõnad Vältige kinnitamata või mittevajalike lühendite failinimedes

Lühendeid ja lühendid failinimedes - teatud juhised:

- Lühendada Azure'i teenuste nimed - faili nime esimesed sõnad peaks olema standardit kirjutatud Azure teenuse või tehnoloogia nimi. 
-   Luba rm või arm nimega lühendeid, mis tahes kohas faili nimi
- Muud industry-standard lühendeid on lubatud vastavalt vajadusele failinimedes. 

##<a name="pattern"></a>Mustri

Siin on põhimõtteliselt.

 **Service-Platform-Language-content-Product-Version.MD**

Kasutage muster osad, et rakendada, ja vaadake artikleid hoidla saada aimu, milline olemasolevate nimede loend. Nimed, mis ei alga arendaja platvormi või teenuse nimi on tõenäoliselt kahtlaste ja libises kaudu.

##<a name="standard-examples"></a>Standardse näited

Siin on mõned näited sobivad nimed, mis järgivad. :

- pilveteenuse-teenuste-dotnet-pidev-delivery.md
- Mobile teenuste-ios-get-started.md
- documentdb haldamine account.md
- Mobile-Services-DotNet-backend-Get-Started-Settings-sync.MD
- Active-Directory-Java-Authenticate-users-Access-Control-Eclipse.MD
- Virtual-Machines-install-Windows-Server-2008R2.MD
- vahemälu aspnet-seansi-oleku-pakkuja
- Azure-SDK-DotNet-Release-Notes-2-8
- StorSimple-Disaster-Recovery-using-Azure-Site-Recovery

##<a name="marketplace-content"></a>Turuplatsi sisu

Eristada sisu, mis keskendub partneri osakaalu Azure'i turuplats, käivitage "turuplatsi" faili nimedega. Selle sisutüübiga ei tohiks liiga levinud, nagu luuakse partnerite oma veebisaitidel enamik partner.

- Marketplace-mongodb-Virtual-Machines-install-Windows-Server-2008R2.MD

##<a name="file-name-approval"></a>Faili nimi kinnitamine

See on meie rühma pull taotluse läbivaatajate failinimedes üle vaadata, kui uue faili esitatakse hoidla esimest korda töö. Pull taotluse läbivaatajad tuleks läbi vaadata selle asemel faili nime ja tagasiside pull taotluse kommentaari voo kaudu, kui on vaja muuta. Faili nimi peab enne pull kutse on aktsepteeritud korrigeerida. Osaliste saate hõlpsalt push värskenduse ootel pull kutse.

##<a name="folder-names-in-the-repo"></a>Kaustanimed repo

Kaustade, tuleks luua ainult teenuste ja faili nimi peaksid olema samad teenuse nälkjas. Kasutage ainult tähti ja sidekriipsu, ning väiketähti. Enne kui saate luua uue kausta, mis pole välja teenuse saada kinnitamise hoidla administraator.

##<a name="changing-case-in-file-names"></a>Faili nimede Täheregistri muutmine

Windowsi opsüsteemid pole tõstutundlikud, nii et kui teil on vaja määrata kest faili nime muuta, on parem teha muudatusi, kui te ei saa muuta Linux või Mac-arvutisse Näiteks:

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Faili ümbernimetamine järgmise käsu abil:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Osaliste juhend lingid

- [Artikli ülevaade](./../README.md)
- [Juhised artiklite register](./contributor-guide-index.md)


<!--Anchors-->
[Reeglid]: #rules
[Mustri]: #pattern
[Standardse näited]: #standard-examples
[Turuplatsi sisu]: #marketplace-content
[Faili nimi kinnitamine]: #file-name-approval
