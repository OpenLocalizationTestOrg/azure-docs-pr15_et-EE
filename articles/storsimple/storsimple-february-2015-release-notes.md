<properties 
   pageTitle="StorSimple 8000 Update 0.3 väljalaskemärkmed | Microsoft Azure'i"
   description="Uued funktsioonid ja parandused, avatud probleemid ja saadaval lahendused kirjeldatakse veebruar 2015 Microsoft Azure'i StorSimple release (Update 0,3)."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000 seeria värskendus 0.3 väljalaskemärkmed – veebruar 2015

## <a name="overview"></a>Ülevaade

Järgmised väljalaskemärkmed tuvastada kriitiliste avatud probleemide StorSimple 8000 sarja värskenduse 0,3 veebruar 2015 välja. Sisaldavad ka see väljaanne sisaldab StorSimple tarkvara ja püsivara värskenduste loendit. See on kolmas vabastamist pärast StorSimple 8000 sarja väljaande versioon on üldiselt kättesaadavaks juuli 2014.
  
Värskendust ei muuda Update'ilt jaanuar seadme tarkvara versioon. See endiselt versiooni 6.3.9600.17312. Saate kinnitada, et värskendus on installitud, märkides ruudu **Viimati värskendatud** kuupäeva. Kui kuupäev on 2015/2/10 või uuem versioon, siis värskenduse on edukalt installitud.  

Soovitame, et otsida ja rakendada saadaolevad värskendused kohe pärast installimist StorSimple seadme. Saate ka sisse automaatvärskendused alla laadima ja installima Microsofti värskenduste kohe, kui nad on välja lülitada. Lisateavet leiate teemast [StorSimple seadme](storsimple-update-device.md).  

Palun vaadake üle enne juurutamist värskenduse oma StorSimple lahendus on väljalaskemärkmed sisalduvat teavet.  

>[AZURE.IMPORTANT]   
>
> - Installige veebruari värskendus StorSimple halduri teenuse ja StorSimple pole Windows PowerShelli abil.   
> - Kulub umbes üks tund värskendust. Siiski värskendustest installimisel protsess võib kuluda umbes 3 tundi lõpuleviimiseks.  
> - StorSimple veebruar versiooni ei sisalda StorSimple virtuaalse seadmele värskendusi. Siiski saate rakendada saadaolevad värskendused Windows virtuaalse seadmes, sh turbe tehtud paranduste, kuid ei kuvata muudatuse virtuaalse seadme versioon.  

Veenduge, et enne uuendamist StorSimple seadme oleksid täidetud järgmised tingimused.  

- Veenduge, et mõlema seadme kontrollerid töötavad saate otsida värskendusi. Kui kumbki kontrolleril ei tööta, skannimine nurjub. Veenduge, et kontrolörid on terve olekus, liikuge **Riistvara oleku** jaotises lehe **hooldustööd** . Kui **vajate tähelepanu**selle on komponendid, pöörduge Microsoft Support enne täiendavate.
- Veenduge, et fikseeritud IP-d nii kontrolleril 0 ja 1 selle domeenikontrolleri on marsruuditavaid ja saate Interneti-ühenduse nagu neid kasutatakse teenindamine seadmele värskendusi. [Testi ühendust cmdlet-käsu](https://technet.microsoft.com/library/hh849808.aspx) abil saate ping teadaolevad aadress väljaspool võrku, nt outlook.com, kinnitamaks, et selle domeenikontrolleri on väljaspool võrgu Ühenduvus.
- Veenduge, et pordid 80 ja 443 oleks seadme StorSimple väljaminev suhtlemiseks saadaval. Lisateavet leiate teemast [Networking StorSimple seadme nõuded](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
- Kui seadme tarkvara versioon on vanem 6.3.9600.17312 (oktoober 2014 update), keelake andmete 2 ja 3 andmete pordid, kui enne uuenduste lubatud. Jättes andmete 2 või 3 andmete pordid lubatud, kui värskenduse, võib põhjustada seadme kontrolleril taastamise režiimi minna. Pange tähele, et kui võrgu liidesed keelamiseks seotud mahud on võrguühenduseta ja selle väljundtoimingute hakkavad värskenduse ajaks katkenud.  
  
## <a name="whats-new-in-the-february-release"></a>Mis on uut versioonis veebruar

Selle värskenduse sisaldab Paranda jaoks soovitud factory lähtestamine probleem, mis toimus oli täiendatud GA seadmete vabastage oktoober 2014 versioonis. Lisateavet leiate teemast [probleemid fikseeritud selles versioonis](#issues-fixed-in-the-february-release).   

Värskendust ei sisalda uusi funktsioone.  

## <a name="issues-fixed-in-the-february-release"></a>Fikseeritud veebruar väljaanne probleemid

Järgmises tabelis kirjeldatakse probleemi, mis on määratud see värskendus.  
 
| Ei. | Funktsioon | Probleem | Kehtib seadme | Kehtib virtuaalse seade |
|-----|---------|-------|---------------------------------|-------------------------------|
| 1 | Factory lähtestamine | Proovite sooritada factory, mis on seadmes, mis algselt oli GA release (versioon 6.3.9600.17215) installitud, kuid on uuendatud oktoober lähtestamine release (6.3.9600.17312 versioon). Funktsiooni factory lähtestamine nurjub ja seadme ebastabiilne. | Jah | Ei |


## <a name="known-issues-in-the-february-release"></a>Veebruar väljaande teadaolevate probleemide

Järgmises tabelis antakse ülevaade selle väljaande teadaolevate probleemide.
 
| Ei. | Funktsioon | Probleem | Kommentaaride/lahendus | Kehtib seadme  | Kehtib virtuaalse seade |
|-----|---------|-------|----------------------------|-----------------------------|--------------------------|
| 1 | Factory lähtestamine | Mõnel juhul, kui lähtestamist factory, StorSimple seade võib kinni ja kuvada teade: **Reset to factory on pooleli (etapp 8)**. See juhtub siis, kui vajutate klahvikombinatsiooni CTRL + C cmdlet ajal. | Vajutage klahvikombinatsiooni CTRL + C pärast alustamist factory lähtestamine. Kui olete juba selles olekus, pöörduge Microsoft Support järgmised toimingud. | Jah | Ei |
| 2 | Ketas kvoorumi | Harvaesinevate eksemplari, kui ketta jaotise EBOD ruumi, mis 8600device enamik ei saa tulemuseks kvoorumit kettale, siis selle talletusmahtu saab ühenduseta. See jääb ühenduseta isegi juhul, kui selle ketast ühendatakse. | Peate taaskäivitage seade. Kui probleem ei lahene, pöörduge Microsoft Support järgmised toimingud. | Jah | Ei |
| 3 | Pilveteenuse hetktõmmise tõrked | Harvaesinevate juhtudel ei pruugi hetktõmmise cloud tõrkega **varukoopia piirmäära jõudnud**. See juhtub, kui te ületada 255 online kloonid samasse seadmesse sama algse mahtu, mis on kustutatud. |  | Jah | Jah |
| 4 | Vale kontrolleril ID | Kui selle domeenikontrolleri asendamine toimub, domeenikontrolleri 0 võib kuvada selle domeenikontrolleri 1. Ajal kontrolleril väljavahetamine, pildi laadimisel omavahelistes sõlme kontrolleril ID kuvada algselt omavahelistes kontrolleril ID. Harvaesinevate juhtudel selline käitumine võib näha ka pärast taaskäivitada. | Kasutajatoimingut ei vaja. Olukord lahendab ise, kui selle domeenikontrolleri asendamine on lõpule jõudnud. | Jah | Ei |
| 5 | Seadme jälgimise diagrammid | StorSimple halduri teenus, seadme jälgimisega seotud diagrammid ei tööta, kui lihtne või puhverserveri server Configuration seadme jaoks on lubatud NTLM autentimine. | Muutke veebi Puhverserveri konfiguratsioon seadme registreeritud teie haldur StorSimple teenusega, nii et autentimine sätteks pole. Selleks käivitada soovitud Windows PowerShelli cmdlet-käsk StorSimple Set-HcsWebProxy. | Jah | Jah |
| 6 | Salvestusruumi kontod | Salvestusruumi teenuse abil konto salvestusruumi kustutada on mõni mittetoetatud stsenaariumi. See viib olukorda, kus kasutaja andmeid ei saa tuua. |  | Jah | Jah |
| 7 | Seadme Tõrkesiirde | Mitme failovers helitugevuse container sama allika seadmest ja muu target seadmed ei toetata.  Helitugevuse ümbriste esimesel üle seadme nurjus ilma andmete omaniku teeb Tõrkesiirde ühe surnud seadmest, mitu seadet. Pärast Tõrkesiirde, nende helitugevuse ümbriste kuvada või toimivad teisiti Azure klassikaline portaali kuvatava. |   | Jah | Ei |
| 8 | Installimine | Ajal StorSimple adapterit SharePointi installi, tuleb sisestada seadme IP selleks installi lõpule. |  | Jah | Ei |
| 9 | Veebiteenuse puhverserveri | Kui teie web puhverserveri konfigureerimine on määratud protokoll HTTPS, mõjutab oma seadme-teenus side ja seadme katkestada. Tugiteenuste pakettide luuakse ka protsessi, tarbimine märgatavalt ressursid oma seadmes. | Veenduge, et web puhverserveri URL on määratud protokoll HTTP. Lisateavet [konfigureerimine veebiteenuse puhverserveri teie seadme](storsimple-configure-web-proxy.md)kohta. | Jah | Ei |
| 10 | Veebiteenuse puhverserveri | Kui konfigureerimine ja lubada veebiteenuse puhverserveri registreeritud seadmes, siis pead taaskäivitage aktiivne kontrolleril seadmes. |  | Jah | Ei |
| 11 | Kõrge cloud latentsus ja suur I/O töökoormus | Väga kõrge cloud latentsused (järjestuse sekundites) ja suur I/O töökoormus kombinatsiooni käivitamise StorSimple seadme seadme mahud minema halvenenud olekus ja selle väljundtoimingute võib nurjuda tõrketeatega "seade ei ole valmis" tõrge. | Peate käsitsi taaskäivitage seade kontrollerid või seadme Tõrkesiirde taastamine juhul teha. | Jah | Ei |

## <a name="physical-device-updates-in-the-february-release"></a>Seadme värskendused veebruaris versioon

See värskendus lahendab factory reset probleem, mis toimus oli täiendatud GA seadmete vabastage oktoober 2014 versioonis. Ei sisalda mõnda muud värskendused StorSimple seadmele.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Vabastage järjestikust manustatud SCSI (SAS) domeenikontrolleri ja veebruaris püsivara värskendused

Selles versioonis ei sisalda järjestikust lisatud SCSI (SAS) kontrolleril või püsivara värskendused. Draiveri värskendus oli oktoobris, 2014 väljaanne.  

## <a name="virtual-device-updates-in-the-february-release"></a>Virtuaalne seadme värskendused veebruaris versioon

Selles versioonis ei sisalda virtuaalse seadme värskendused. Selle värskenduse rakendamist ei muutu virtuaalne seadme tarkvara versioon.
 
