<properties
pageTitle="Oracle'i VM piltide kasutamise kohta | Microsoft Azure'i"
description="Teavet toetatud konfiguratsioone ja ka Oracle'i VM Windows Server Azure'i piirangud enne juurutamist."
services="virtual-machines-windows"
documentationCenter=""
manager="timlt"
authors="rickstercdn"
tags="azure-service-management"/>

<tags
ms.service="virtual-machines-windows"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-windows"
ms.workload="infrastructure-services"
ms.date="09/06/2016"
ms.author="rclaus" />

#<a name="miscellaneous-considerations-for-oracle-virtual-machine-images"></a>Mitmesugused kaalutluste kohta Oracle'i virtuaalse masina pildid



Selles artiklis antakse ülevaade kaalutluste kohta Oracle'i virtuaalmasinates Azure, mis põhinevad Oracle'i tarkvara pilte, mida Oracle.  

-  Oracle'i andmebaas virtuaalse masina pildid
-  Oracle'i serveri Weblogic serverit virtuaalse masina pildid
-  Oracle'i JDK virtuaalse masina pildid

##<a name="oracle-database-virtual-machine-images"></a>Oracle'i andmebaas virtuaalse masina pildid
### <a name="clustering-rac-is-not-supported"></a>Klastrite (piirkondlik) ei toetata

Azure'i ei toeta praegu Oracle'i otse rakenduse kogumite ülesanne Oracle'i andmebaasist. Ainult autonoomse eksemplarid Oracle'i andmebaasiga on võimalik. Selle põhjuseks on Azure ei toeta praegu virtuaalse ketta ühiskasutuse lugemis-ja kirjutamisõigusega viisil vahel virtuaalse masina mitmes eksemplaris. Multisaate UDP ei toetata.

### <a name="no-static-internal-ip"></a>Pole sisemine staatiline IP

Azure'i määrab iga virtuaalse masina sisemise IP-aadress. Kui virtuaalse masina on virtuaalse võrgus, virtuaalse masina IP-aadress on dünaamiline ja võib muutuda pärast virtual seadme taaskäivitamist. See võib põhjustada probleeme, kuna Oracle'i andmebaasiga eeldab, et olla staatiline IP-aadress. Probleemi vältimiseks kaaluge lisamist virtuaalse masina Azure virtuaalse võrku. Lisateabe saamiseks vaadake [Virtuaalse võrgu](https://azure.microsoft.com/documentation/services/virtual-network/) ja [Azure virtuaalse võrgu loomine](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="attached-disk-configuration-options"></a>Manustatud ketta konfiguratsiooni suvandid

Andmefailide saab paigutada, kas operatsioonisüsteem ketta virtuaalse masina või manustatud ketast, ka andmete ketast. Manustatud ketast pakkuda parema jõudluse ja suurus paindlikkust kui ketta operatsioonisüsteem. Operatsioonisüsteemi ketas võib olla ainult parem, andmebaaside alla 10 gigabaiti (GB).

Manustatud ketast toetuvad Azure'i bloobimälu salvestusteenus. Iga ketta suudab teoreetiline kuni umbes 500 sisend toimingute sekundis (IOPS). Manustatud ketast jõudlus ei pruugi optimaalse algselt ja IOPS jõudlust võivad oluliselt parandada ligikaudu 60-90 minutit makseviisid möödudes "Kirjuta-in". Kui ketta hiljem jääb jõude, võib IOPS jõudluse vähendada kuni teise kirjutamine rakenduses aja pidev. Lühidalt, veel aktiivne on ketas, suurem tõenäosus on lähenemine optimaalse IOPS jõudlust.

Kuigi lihtsaim lähenemine on lisada ühe ketta virtuaalse masina ja sellele andmebaasifailide sellel kettal, see lähenemine on ka kõige piiramine jõudluse osas. Selle asemel saab sageli parandada IOPS tõhustada, kui kasutate mitut manustatud ketast, andmebaasi andmete laiali, ja seejärel kasutage Oracle'i automaatse salvestusruumi haldamine (ASM). Lisateabe saamiseks vaadake [Oracle'i automaatse salvestusruumi ülevaade](http://www.oracle.com/technetwork/database/index-100339.html) . Kuigi see on võimalik kasutada mitut ketast triibustus operatsioonisüsteemi tasemel, see lähenemine on takistada kuna ei ole teada IOPS jõudluse parandamiseks.

Vaatame kahte erinevat lähenemisviisi manustamise mitu ketast vastavalt kas soovite lugeda toimingute tähtsuse või andmebaasi kirjutada.

- **Oracle'i ASM oma** tõenäoliselt töökindluse kirjutamine toiming, kuid halb IOPS loetuks toimingute abil ketta massiivi lähenemine võrreldes. Järgmisel joonisel on kujutatud loogiliselt selle korraldusega.  
    ![](media/virtual-machines-windows-classic-oracle-considerations/image2.png)

>[AZURE.IMPORTANT] Hinnata vahelist kirjutamine jõudlus ja lugege jõudlust juhul-. Tegelike tulemuste võib olla muutuv, kui kasutate seda.

### <a name="high-availability-and-disaster-recovery-considerations"></a>Kõrge kättesaadavus ja katastroofi taastamine kaalutluste

Azure'i virtuaalmasinates Oracle'i andmebaasiga kasutamisel vastutate rakendamiseks on kõrge kättesaadavus ja avariitaastet lahendus mis tahes vältimiseks. Teie vastutate ka varundada oma andmeid ja.

Oracle'i andmebaas Enterprise Edition (ilma piirkondlik) Azure kõrge kättesaadavus ja Avariijärgne taaste saavutada abil [andmete Guard, aktiivne andmete Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html)või [Oracle kuldne Gate](http://www.oracle.com/technetwork/middleware/goldengate), kaks eraldi virtuaalmasinates kaks andmebaasidega. Nii virtuaalmasinates peaks olema sama [pilveteenuses](virtual-machines-linux-classic-connect-vms.md) ja sama [virtuaalse võrgu](https://azure.microsoft.com/documentation/services/virtual-network/) tagamaks, et nad pääsevad üksteisest üle privaatne püsivate IP-aadress.  Lisaks, soovitame selle virtuaalmasinates paigutamine on sama [kättesaadavus](virtual-machines-windows-manage-availability.md) lubada Azure need asetada eraldi viga domeenid ja uuendada domeenid. Ainult virtuaalmasinates sama pilveteenuses saavad osaleda samu kättesaadavus. Iga virtuaalse masina peab olema vähemalt 2 GB mälu ja 5 GB vaba kettaruumi.

Oracle'i andmete valvur, saate kõrge-saadavus saavutada esmane andmebaasist ühe virtuaalse masina, teisene (valige) andmebaasist teise virtuaalse masina, ja nende vahel luua ühesuunalise kopeerimine. Tulem on lugemisõigus andmebaasi koopia. Oracle'i GoldenGate, abil saate konfigureerida kahesuunalise dispersioonanalüüs nende kahe andmebaasi vahel. Saate teada, kuidas häälestada oma andmebaaside nende tööriistade abil kõrge-saadavus lahenduse, dokumentatsioonist [Aktiivne andmete kaitse](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) ja [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) Oracle'i veebisaidil. Kui teil on vaja registri Accessi andmebaasi koopia, saate kasutada [Oracle'i aktiivne andmete Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

##<a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle'i serveri Weblogic serverit virtuaalse masina pildid

-  **Rühmitamise on toetatud Enterprise'i ainult.** Teil on litsents Weblogic serverit rühmitamise ainult siis, kui Enterprise Edition Weblogic serverit serveri kasutamine. Ärge kasutage rühmitamise Weblogic serverit Server Standard Edition.

-  **Ühenduse ajalõpud:** Kui teie rakendus sõltub ühendused avaliku lõpp-punktid teise Azure pilveteenusesse (nt andmebaasi taseme teenus), võib Azure'i sulgege need avatud ühendused tegevusetuse nelja minuti pärast. See võib mõjutada funktsioonid ja rakendused, tuginedes ühenduse kaustu, kuna ühendusi, mis on rohkem kui selle piirmäära passiivsed võib jääda pole enam kehtiv. Kui see mõjutab rakenduse, kaaluge "ülalhoidmise" loogika teie ühendus kaustu.

    Kui lõpp on *sisemine* juurutamise Azure pilveteenuse teenuse (nt autonoomse andmebaasi virtuaalse masina sees oma Weblogic serverit virtuaalmasinates nimega *sama* pilveteenus), siis ühendus on otsene ei viita koormusetasakaalustusteenuse Azure laadimine ja seetõttu ei kehti ühenduse ajalõpp.

-  **UDP multisaate ei toetata.** Azure'i toetab UDP unicasting, kuid mitte multiedastusteenustele või edastamise. Weblogic serverit Server on võimalik kasutada Azure UDP ainusaate võimalusi. Jaoks kõige paremini tuginedes UDP ainusaate tulemuste saamiseks soovitame Weblogic serverit kobar suurus staatiline, hoida või hoida klaster sisaldab üle 10 hallatavate serveritega.

-  **Weblogic serverit serveri eeldab, et avalike ja privaatvõ pordid sama T3 juurdepääsu (nt ettevõtte JavaBeans kasutamisel).** Kaaluge mitmekihilise stsenaarium, kus töötab kiht (EJB) teenuserakenduse Weblogic serverit serveri klaster koosneb kahe või enama hallatud serverite pilveteenuses, nimega **SLWLS**. Kliendi taseme on lihtne Java programmi proovite helistada EJB teenuse kiht erinevate pilveteenus. Kuna on vaja laadimiseks saldo teenuse kiht, tuleb avaliku koormusetasakaalustusega lõpp-punkti Virtuaalmasinates Weblogic serverit serveri klaster jaoks luua. Kui selle lõpp-punkti määratud privaatne pordi erineb avaliku pordi (nt 7006:7008), ilmneb tõrge, näiteks järgmisi:

        [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

        Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

    See on mis tahes T3 Kaugpöördusteenuse, Weblogic serverit serveri loodab laadi koormusetasakaalustusteenuse portide ja Weblogic serverit hallatavate portide sama. Ülaltoodud näites kliendi on juurdepääs pordi 7006 (Laadi koormusetasakaalustusteenuse port) ja hallatavate server on kuulamise 7008 (privaatne port). See piirang kehtib ainult T3 juurdepääsu korral ei HTTP.

    Selle probleemi vältimiseks kasutage ühte järgmistest lahendustest.

    -  Kasutage sama era- ja pordinumbrid pühendunud T3 Accessi koormus tasakaalustatud lõpp-punktid.

    -  Järgmised järgmine töötab parameeter Weblogic serverit serveri käivitamisel.

            -Dweblogic.rjvm.enableprotocolswitch=true

Seotud teavet leiate teemast teabebaasi artikkel **860340.1** <http://support.oracle.com>juures.

-  **Dünaamiliste rühmitamise ja laadi tasakaalustamiseks piirangud.** Oletame, et soovite kasutada dünaamilise kobar Weblogic serverit serveris ja jätke see on üks, avaliku koormusetasakaalustusega lõpp-punkti Azure kaudu. Seda saab teha, kui kasutate fikseeritud pordinumber iga hallatava serverid (mitte dünaamiliselt määratud vahemiku) ja käivitage rohkem hallatud serverite kui administraator on jälgimise masinad (st rohkem kui üks hallatavad serveri virtual masina kohta). Kui teie konfiguratsiooni, on tulemuseks rohkem Weblogic serverit servereid käivitamisel, kui seal on virtuaalmasinates (st kui mitu Weblogic serverit serveri eksemplari ühiskasutus virtual samasse arvutisse), siis ei saa rohkem kui ühe nende esinemisjuhud Weblogic serverit Server serverid sidumiseks antud pordi number-teisi seda virtual arvutisse nurjub.

    Teisalt, kui konfigureerite kordumatu pordinumbrid automaatseks määramiseks selle hallatava serverid serveri administraator, siis koormusetasakaalustuseks ei ole võimalik Kuna Azure ei toeta ühte avaliku porti vastendus mitme privaatne portide, nagu peaksid olema selle konfiguratsiooni puhul.

-  **Mitmes eksemplaris Weblogic serverit serveri virtual arvutisse.** Olenevalt teie juurutamise nõudeid, võiksite suvandi mitu eksemplari Weblogic serverit Server töötab virtual samasse arvutisse, kui virtuaalse masina on piisavalt suur. Näiteks keskmise suurusega virtual masina, mis sisaldab kahe protsessorituuma, võib valida kahe Weblogic serverit serveri eksemplari käivitamiseks. Pange tähele siiski, et soovitame veel ühe punkti tõrgete viimine oma arhitektuuri, mis oleks juhul, kui kasutasite ainult ühe virtual arvutisse, kus töötab mitu eksemplari Weblogic serverit serveri vältida. Vähemalt kaks virtuaalmasinates abil võib olla parem lähenemine ja iga nende virtuaalmasinates võib käivitage Weblogic serverit serveri mitmes eksemplaris. Siiski võib juhul Weblogic serverit serveri osa sellest sama kobar. Pange tähele, siiski see pole praegu võimalik kasutada Azure laadi – Topeltdegressiivse lõpp-punktid, mis on esitatud sellise Weblogic serverit Server juurutuste sama virtual seadmes, kuna Azure laadi koormusetasakaalustusteenuse nõuab koormusetasakaalustusega serverid jaotada kordumatu virtuaalmasinates vahel.

##<a name="oracle-jdk-virtual-machine-images"></a>Oracle'i JDK virtuaalse masina pildid

-  **JDK 6 ja 7 uusimad värskendused.** Ajal Java (praegu Java 8) Viimane avalik, toetatud versiooni soovitame Azure'i muudab JDK 6 ja 7 pildid saadaval. See on mõeldud pärand rakendusi, mis pole veel JDK 8 täiendamiseks valmis. Värskendused eelmise JDK pilte aga pole enam saadaval avalikkusele antud Oracle, Microsoft koostöös JDK 6 ja 7 pilte, mida Azure on mõeldud sisaldavad uuema – avaliku värskenduse, mis on tavaliselt pakutud Oracle'i ainult rühmaga toetatud Oracle'i kliendid. Uute versioonide JDK pildid tehakse aja jooksul värskendatud versioonidega JDK 6 ja 7.

    Saadaval selle JDK 6 ja 7 pilte, ja virtuaalmasinates ja pildid nendest, JDK saab kasutada ainult Azure'is.

-  **64-bitine JDK.** Oracle Weblogic serverit serveri virtuaalse masina pildid ja Azure esitatud Oracle'i JDK virtuaalse masina pildid sisaldavad 64-bitised versioonid nii Windows Server ja selle JDK.

##<a name="additional-resources"></a>Lisaressursid
[Oracle'i virtuaalse masina pilte Azure](virtual-machines-linux-classic-oracle-images.md)
