<properties 
    pageTitle="StorSimple 8000 Update 0.1 väljalaskemärkmed | Microsoft Azure'i"
    description="Uued funktsioonid ja parandused, avatud probleemid ja saadaval lahendused kirjeldatakse oktoober 2014 Microsoft Azure'i StorSimple release (värskendus 0,1)."
    services="storsimple"
    documentationCenter="NA"
    authors="alkohli"
    manager="carmonm"
    editor="" />
 <tags 
    ms.service="storsimple"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="TBD"
    ms.date="09/21/2016"
    ms.author="alkohli" />

# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000 seeria Update 0.1 väljalaskemärkmed – oktoober 2014  

## <a name="overview"></a>Ülevaade

Järgmised väljalaskemärkmed tuvastada kriitiliste avatud probleemide StorSimple 8000 sarja värskenduse 0,1 välja oktoober 2014. Sisaldavad ka see väljaanne sisaldab StorSimple tarkvara ja püsivara värskenduste loendit. See on esimene väljaanne pärast StorSimple 8000 sarja väljaande versioon on üldiselt kättesaadavaks juuli 2014 ja tarkvara versiooni 6.3.9600.17312 vastab.  

Soovitame, et otsida ja rakendada saadaolevad värskendused kohe pärast installimist seade. Saate ka sisse automaatvärskendused alla laadima ja installima Microsofti värskenduste kohe, kui nad on välja lülitada. Lisateabe saamiseks lugege teemat Kuidas [värskendada StorSimple seadme](storsimple-update-device.md).  

Palun vaadake üle enne juurutamist värskendusi oma StorSimple lahendus on väljalaskemärkmed sisalduvat teavet.  

>[AZURE.IMPORTANT]
> 
-   Oktoober värskenduste installimine StorSimple halduri teenuse ja StorSimple pole Windows PowerShelli abil.  
-   Värskenduste tavaliselt võtab umbes 3 tundi lõpuleviimiseks.  
-   StorSimple oktoober versiooni ei sisalda StorSimple virtuaalse seadmele värskendusi. Saate ikka rakendada saadaolevad Windowsi värskendused, tehtud paranduste turvalisus, sealhulgas, kuid ei kuvata versiooni virtuaalse seadme muudatuste.  

Veenduge, et enne uuendamist StorSimple seadme oleksid täidetud järgmised tingimused.  

- Veenduge, et mõlema seadme kontrollerid töötavad saate otsida värskendusi. Kui kumbki kontrolleril ei tööta, skannimine nurjub. Veenduge, et kontrolörid on terve olekus, liikuge **Riistvara oleku** jaotises lehe **hooldustööd** . Kui **vajate tähelepanu**selle on komponendid, pöörduge Microsoft Support enne täiendavate.  
- Tagada fikseeritud IP-d jaoks nii kontrolleril 0 ja 1 kontrolleril marsruuditavaid ja saate Interneti-ühenduse nagu neid kasutatakse teenindamine seadmele värskendusi. [Testi ühendust cmdlet-käsu](https://technet.microsoft.com/library/hh849808.aspx) abil saate ping teadaolevad aadress väljaspool võrku, nt outlook.com, kinnitamaks, et selle domeenikontrolleri on väljaspool võrgu Ühenduvus.  
- Veenduge, et vajalikud Väljaminev pordid oleks seadme StorSimple väljaminev suhtlemiseks saadaval. Lisateavet leiate teemast [Networking StorSimple seadme nõuded](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
- Kui seadme tarkvara versioon on vanem 6.3.9600.17312 (oktoober 2014 update), keelake andmete 2 ja 3 andmete pordid, kui enne uuenduste lubatud. Kui jätate andmed 2 või 3 andmete pordid lubatud, kui värskenduse rakendamist, võib see põhjustada seadme kontrolleril taastamise režiimi minna. Pange tähele, et kui võrgu liidesed keelamiseks seotud mahud on võrguühenduseta ja selle I/O hakkavad värskenduse ajaks katkenud.  

## <a name="whats-new-in-the-october-release"></a>Mis on uut versioonis oktoober

Selle värskenduse sisaldab järgmisi täiustusi.

- Nüüd saate hallata oma seadme kontrollerid StorSimple halduri teenuse kasutajaliides. Haldamise toiminguid, mis sisaldavad taaskäivitamiseks sulgumist, või mõne selle domeenikontrolleri sisse lülitada. Lisateabe saamiseks minge [haldamine StorSimple seadme kontrollerid](storsimple-manage-device-controller.md).  
- Saate ajastada WAN läbilaskevõimet eraldatud vastavalt nädalapäeva ja kellaaja päeva kombinatsioone. See võimaldab teil paremini WAN läbilaskevõime aegsasti ajal kasutada. Erinevate läbilaskevõime mallide uus draiv ümbriste jaoks lubatud. Lisateabe saamiseks minge [oma StorSimple läbilaskevõime levirühmade haldamine](storsimple-manage-bandwidth-templates.md).  
- Saate konfigureerida meiliteatised soovitud administraatorid ja teistel olemasolevatele või võimalik, et eelseisvad probleemid aktiivselt teavitama. Lisateabe saamiseks minge [Teatiste sätete konfigureerimine](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Fikseeritud oktoober väljaanne probleemid


Järgmises tabelis antakse ülevaade probleeme, mis olid selle värskenduse.  

| Ei. | Funktsioon | Probleem | Kehtib seadme | Kehtib virtuaalse seade |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Võrgu liidesed | Eelmise versiooni võrgu liidesed andmete 2 ja 3 andmete olid vahetasin tarkvara. See on fikseeritud selle värskenduse. Tühjendage sätete ja keelake võrgu liideste enne värskenduse installimist. Pärast värskenduse installimist on teil konfigureerida liideste. | Jah | Ei |
| 2 | Tugiteenuste pakett | Eelmises versioonis, kui käivitasite selle domeenikontrolleri (BMC) logid toomiseks Windows PowerShelli **Ekspordi-HcsSupportPackagePõrandaliist** cmdlet toiming nurjus, järgmine hoiatus: "toiming selle domeenikontrolleri õnnestus, kuid tõttu järgmised tõrked kontrolleril omavahelistes nurjus. Kontrollige, kas selle partneri on terve ja kas praegusest saab ühendada funktsiooni omavahelistes." See probleem on nüüd lahendatud. | Jah | Ei |
| 3 | Seadme Tõrkesiirde | Eelmisest versioonist, on võimalus andmete vastuolu kui seadme Tõrkesiirde ajal nurjus **avastamine varundamise** töö. See probleem on nüüd lahendatud. | Jah | Ei |
| 4 | Seadme Tõrkesiirde | Pärast seadet Tõrkesiirde, eelmist versiooni varukoopiaid olid nähtavad, kuid seotud helitugevuse ümbris ei esine target seadmes. See probleem on nüüd lahendatud. | Jah | Ei |
| 5 | Seadme Tõrkesiirde | Eelmise versiooni oli viga pilveteenuste varukoopiate loendamise mis põhjustada andmete vastuolu oleks olemas cloud ühenduvusprobleemide registri taastamine selle toimingu käigus. | Jah | Ei |
| 6 | Püsivara värskendus | Eelmise versiooni seadme püsivara värskenduse töö nurjus ja kuvatakse tõrge, mis märgitud, et cmdlet-käsud ei ole ja värskendamine nurjus tulemusena. Selle domeenikontrolleri siis läks taastamise režiimi. See probleem on nüüd lahendatud. | Jah | Ei |
| 7 | Installimine | Nüüd on kinnitatud seade ei on imaged õigesti installimise ajal põhjustatud vead. | Jah | Ei |
| 8 | Factory lähtestamine | Saate nüüd võite vahele jätta factory reset püsivara sisse. See on eelmisest versioonist muudatus. | Jah | Ei |
| 9 | Factory lähtestamine | Eelmise versiooni factory reset cmdlet-käsu käitamisel püsivara versioon kontrollid toimus vaid mõned riistavara jaoks. Täiendavad püsivara kontrollid toimus pärast esimest taaskäivitamist protsessi, mis võivad põhjustada nurjumise lähtestamine. See parandus tagab, et kõik selle püsivara kontrollimine lähtestamisel cmdlet-käsk selle factory käitatakse ja enne esimest süsteemi taaskäivitage. | Jah | Ei |
| 10 | Salvestusruumi konto võtme pööre | Pööramiseks salvestusruumi konto võtmed kohe kasutada **Invoke-HcsmServiceDataEncryptionKeyChange** cmdlet, mis palub kasutajal sisestada teenuse andmete krüptovõtme. See on eelmisest versioonist, kus teenuse andmete krüptovõtme võeti vastu tekstisisene parameetrina muudatus. | Jah | Ei |
| 11 | Failback 24 tunni jooksul | Tõrkejärgseks ajal ei juhtu Kettapuhastus allikas seadme puhtalt, põhjustab failback nurjumise. See on fikseeritud selles versioonis. | Jah | Ei |

## <a name="known-issues-in-the-october-release"></a>Oktoober väljaande teadaolevate probleemide

Järgmises tabelis antakse ülevaade selle väljaande teadaolevate probleemide.

| Ei. | Funktsioon | Probleem | Kommentaaride/lahendus | Kehtib seadme | Kehtib virtuaalse seade |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Factory lähtestamine | Mõnel juhul, kui lähtestamist factory, StorSimple seade võib kinni ja kuvada teade: **Reset to factory on pooleli (etapp 8)**. See juhtub siis, kui vajutate klahvikombinatsiooni CTRL + C cmdlet ajal. | Vajutage klahvikombinatsiooni CTRL + C pärast alustamist factory lähtestamine. Kui olete juba selles olekus, pöörduge Microsoft Support järgmised toimingud. | Jah | Ei |
| 2 | Factory lähtestamine | Tehke ei factory reset StorSimple seade, mida on värskendatud GA: oktoober 2014 versioon. | Selle toimingu toimib ainult siis, kui paik on installitud. Võtke ühendust saada see nõutav paik Microsoft Support. | Jah | Ei | 
| 3 | Ketas kvoorumi | Harvaesinevate eksemplari, kui ketta jaotise EBOD ruumi 8600 seadme enamik ei saa tulemuseks kvoorumit kettale, siis selle talletusmahtu saab ühenduseta. See jääb ühenduseta isegi juhul, kui selle ketast ühendatakse. | Peate taaskäivitage seade. Kui probleem ei lahene, pöörduge Microsoft Support järgmised toimingud. | Jah | Ei |
| 4 | Pilveteenuse hetktõmmise tõrked | Harvaesinevate juhtudel ei pruugi hetktõmmise cloud tõrkega **varukoopia piirmäära jõudnud**. See juhtub, kui te ületada 255 online kloonid samasse seadmesse sama algse mahtu, mis on kustutatud. | | Jah | Jah |
| 5 | Vale kontrolleril ID | Kui selle domeenikontrolleri asendamine toimub, domeenikontrolleri 0 võib kuvada selle domeenikontrolleri 1. Ajal kontrolleril väljavahetamine, pildi laadimisel omavahelistes sõlme kontrolleril ID kuvada algselt omavahelistes kontrolleril ID. Harvaesinevate juhtudel selline käitumine võib näha ka pärast taaskäivitada. |Kasutajatoimingut ei vaja. Olukord lahendab ise, kui selle domeenikontrolleri asendamine on lõpule jõudnud. | Jah | Ei |
| 6 | Seadme jälgimise diagrammid | StorSimple halduri teenus, seadme jälgimisega seotud diagrammid ei tööta, kui lihtne või puhverserveri server Configuration seadme jaoks on lubatud NTLM autentimine. | Muutke veebi Puhverserveri konfiguratsioon seadme registreeritud teie haldur StorSimple teenusega, nii et autentimine sätteks pole. Selleks käivitada soovitud Windows PowerShelli cmdlet-käsk StorSimple Set-HcsWebProxy. | Jah | Jah |
| 7 | Salvestusruumi kontod | Salvestusruumi teenuse abil konto salvestusruumi kustutada on mõni mittetoetatud stsenaariumi. See viib olukorda, kus kasutaja andmeid ei saa tuua. | | Jah | Jah |
| 8 | Seadme Tõrkesiirde | Mitme failovers helitugevuse container sama allika seadmest ja muu target seadmed ei toetata. | Helitugevuse ümbriste esimesel üle seadme nurjus ilma andmete omaniku teeb Tõrkesiirde ühe surnud seadmest, mitu seadet. Pärast Tõrkesiirde, nende helitugevuse ümbriste kuvada või toimivad teisiti Azure klassikaline portaali kuvatava. | Jah | Ei |
| 9 | Installimine | Ajal StorSimple adapterit SharePointi installi, tuleb sisestada seadme IP selleks installi lõpule.    | | Jah | Ei |
| 10 | Veebiteenuse puhverserveri | Kui teie web puhverserveri konfigureerimine on määratud protokoll HTTPS, mõjutab oma seadme-teenus side ja seadme katkestada. Tugiteenuste pakettide luuakse ka protsessi, tarbimine märgatavalt ressursid oma seadmes. | Veenduge, et web puhverserveri URL on määratud protokoll HTTP. Lisateavet [konfigureerimine veebiteenuse puhverserveri teie seadme](storsimple-configure-web-proxy.md)kohta. | Jah | Ei |
| 11 | Veebiteenuse puhverserveri | Kui konfigureerimine ja lubada veebiteenuse puhverserveri registreeritud seadmes, siis pead taaskäivitage aktiivne kontrolleril seadmes. | | Jah | Ei |
| 12 | Kõrge cloud latentsus ja suur I/O töökoormus | Väga kõrge cloud latentsused (järjestuse sekundites) ja suur I/O töökoormus kombinatsiooni käivitamise StorSimple seadme seadme mahud minema halvenenud olekus ja selle väljundtoimingute võib nurjuda tõrketeatega "seade ei ole valmis" tõrge. | Peate käsitsi taaskäivitage seade kontrollerid või seadme Tõrkesiirde taastamine juhul teha. | Jah | Ei |

## <a name="physical-device-updates-in-the-october-release"></a>Seadme värskendused oktoober versioon

Kui värskendused on rakendatud füüsilise seadmega, muutub tarkvara versiooni 6.3.9600.17312. Teisiti need vabastage märkmete kõik mudelid StorSimple seadme rakendada. Nende värskenduste kohta leiate lisateavet teemast [oktoober 2014 füüsilise seadme tarkvara värskendus Microsoft Azure StorSimple seadme](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Vabastage järjestikust manustatud SCSI (SAS) domeenikontrolleri ja oktoober püsivara värskendused

See väljaanne värskendab juht- ja SAS kontrolleril füüsilise seadme püsivara. Lisateavet SAS kontrolleril update, vt [oktoober 2014 värskendada Microsoft Azure'i StorSimple seadme LSI SAS kontrollerid](http://support.microsoft.com/kb/2987020).   

Selles versioonis on olemas ka kumulatiivse püsivara värskenduse, mis käsitleb töökindluse probleeme seadme riistvara. Püsivara värskenduste kohta leiate lisateavet teemast [oktoober 2014 püsivara värskendada Microsoft Azure'i StorSimple seadme](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Virtuaalne seadme värskendused oktoober versioon

Selles versioonis ei sisalda virtuaalse seadme värskendused. Selle värskenduse rakendamist ei muutu virtuaalne seadme tarkvara versioon.
 
