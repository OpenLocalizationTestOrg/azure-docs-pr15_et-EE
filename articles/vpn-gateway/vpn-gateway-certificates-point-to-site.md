<properties 
   pageTitle="Luua iseallkirjastatud serdid punkti saidi virtuaalse võrgu asutusesiseses ühendused makecert abil | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse makecert abil saate luua iseallkirjastatud serdid opsüsteemis Windows 10."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# <a name="working-with-self-signed-certificates-for-point-to-site-connections"></a>Töötamine iseallkirjastatud serdid punkti saidi ühendused

See artikkel aitab teil abil **makecert**iseallkirjastatud serdi loomine ja seejärel luua kliendi sertide seda. Juhised opsüsteemis Windows 10 jaoks makecert kirjutada. MakeCert kinnitamise serdid, mis ühilduvad P2S ühenduste loomine. 

P2S ühenduste, serdid eelistatud meetod on kasutada oma ettevõtte sert lahenduse, veendudes, et anda kliendi nimi väärtus levinud vorminguga 'name@yourdomain.com', asemel 'NetBIOS domeeni name\username' vorming.

Kui teil pole ettevõtte lahenduse, iseallkirjastatud serdi tuleb lubada P2S kliendid virtuaalse võrguga ühenduse loomiseks. Oleme teadlikud, et makecert on taunitud, kuid see pole veel lubatud meetod iseallkirjastatud sertide, mis ühilduvad P2S ühenduste loomine. Töötame iseallkirjastatud sertide loomine mõne muu lahendus, kuid sel ajal, makecert on eelistatud meetod.

## <a name="create-a-self-signed-certificate"></a>Iseallkirjastatud serdi loomine

MakeCert on üks võimalus iseallkirjastatud serdi loomine. Järgmised toimingud sõelub abil makecert iseallkirjastatud serdi loomine. Neid juhiseid ei ole juurutus-mudel konkreetset. Need sobivad nii ressursihaldur ja klassikaline.

### <a name="to-create-a-self-signed-certificate"></a>Iseallkirjastatud serdi loomine

1. Arvutis, kus töötab Windows 10, allalaadimine ja installimine [Windowsi tarkvara Kit (SDK) Windows 10 jaoks](https://dev.windows.com/en-us/downloads/windows-10-sdk).

2. Pärast installimist, saate otsida makecert.exe kasuliku jaotises selle tee: C:\Program Files (x86) \Windows Kits\10\bin\<arch >. 
        
    Näide:`C:\Program Files (x86)\Windows Kits\10\bin\x64`

3. Järgmiseks loomine ja sert isikliku serdi poes teie arvutisse installida. Järgmises näites luuakse Azure'i üleslaadimine P2S konfigureerimisel vastavate *CER* -fail. Järgmine käsk Käivita administraatorina. Asendage *ARMP2SRootCert* ja *ARMP2SRootCert.cer* nimi, mida soovite kasutada serti.<br><br>Serdi asub sertide – praegune User\Personal\Certificates.

        makecert -sky exchange -r -n "CN=ARMP2SRootCert" -pe -a sha1 -len 2048 -ss My "ARMP2SRootCert.cer"


###  <a name="rootpublickey"></a>Avalik võti hankimine

Punkti saidi ühenduste VPN-lüüsi konfigureerimise käigus on avalik võti juurkausta serdi Azure üles laadida.

1. CER-fail: serdi saamiseks avage **tekst certmgr.msc**. Paremklõpsake root iseallkirjastatud serdi, klõpsake käsku **Kõik tööülesanded**ja seejärel klõpsake nuppu **ekspordi**. **Serdi ekspordiviisardi**avaneb.

2. Viisardi, klõpsake nuppu **edasi**, valige **ei, ekspordi privaatvõti**ja seejärel klõpsake nuppu **edasi**.

3. Lehel **Ekspordi failivorming** valige **Base-64-kodeeritud X.509 (. CER).** Klõpsake nuppu **edasi**. 

4. Klõpsake selle **faili eksportida**, **liikuge** asukohta, kuhu soovite eksportida sert. Serdi faili nimi **faili nimi**. Klõpsake nuppu **edasi**.

5. Klõpsake nuppu **valmis** serdi eksportida.

 
### <a name="export-the-self-signed-certificate-optional"></a>Eksportige iseallkirjastatud sert (valikuline)

Kui soovite eksportida iseallkirjastatud serti ja seda turvaliselt säilitada. Kui vaja, saate hiljem mõnda teise arvutisse installida ja luua mitme kliendi sertide või eksportida teise CER-fail. Mis tahes arvuti, kuhu on installitud kliendi serti ja mis on konfigureeritud ka proper VPN klientrakenduse sätted saate luua ühenduse virtuaalse võrgu kaudu P2S. Seetõttu, mida soovite veenduge, et kliendi serdid on loodud ja installitud ainult siis, kui vaja ja iseallkirjastatud sert on talletatud turvaline.

Iseallkirjastatud serdi eksportida on pfx, valige juurkausta sert ja kasutada samu juhiseid nagu on kirjeldatud [eksportida kliendi serdi](#clientkey) eksportida.

## <a name="create-and-install-client-certificates"></a>Loomine ja installige kliendi

Ärge installige iseallkirjastatud sert, kliendi arvutisse. Tuleb teil luua kliendi serti iseallkirjastatud sert. Saate seejärel eksportida ja kliendi serdi kliendi arvutisse installida. Järgmised toimingud ei ole juurutus-mudeli konkreetset. Need sobivad nii ressursihaldur ja klassikaline.

### <a name="part-1---generate-a-client-certificate-from-a-self-signed-certificate"></a>Osa 1 - luua iseallkirjastatud serdi kliendi sert

Järgmised toimingud sõelub üks viis, kuidas luua iseallkirjastatud serdi kliendi serti. Teil võib luua mitme kliendi sertide serti. Seejärel saate iga kliendi sertifikaadi eksporditud ja kliendi arvutisse installitud. 

1. Samas arvutis, mida kasutasite iseallkirjastatud serdi loomine, avage administraatorina käsuviiba.

2. Selles näites "ARMP2SRootCert" viitab teie loodud iseallkirjastatud sert. 
    - Saate muuta iseallkirjastatud juur, mis teil loovad kliendi serdi kaudu nime *"ARMP2SRootCert"* . 
    - Saate muuta *ClientCertificateName* soovite luua kliendi sert olema nimi. 


    Saate muuta ja käivitada kliendi serdi loomiseks valimi. Kui käivitate selle muutmata järgmises näites, tulem on nimega ClientCertificateName oma isikliku serdi poes serdi ARMP2SRootCert põhjal loodud kliendi serti.

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "ARMP2SRootCert" -is my -a sha1

4. Kõik serdid on talletatud teie 'Serdid - praegune User\Personal\Certificates' oma arvutisse salvestada. Saate luua mitme kliendi sertide vastavalt vajadusele põhineb seda toimingut.

### <a name="clientkey"></a>Osa 2 - kliendi sertifikaadi eksportimine

1. Eksportida klienti, avage **tekst certmgr.msc**. Paremklõpsake kliendi sert, mida soovite eksportida, klõpsake käsku **Kõik tööülesanded**ja seejärel klõpsake nuppu **ekspordi**. **Serdi ekspordiviisardi**avaneb.

2. Viisardi, klõpsake nuppu **edasi**, valige **Jah, ekspordi privaatvõti**, ja seejärel klõpsake nuppu **edasi**.

3. Lehel **Ekspordi failivorming** saate jätke vaikesätted, mis on valitud. Klõpsake nuppu **edasi**. 
 
4. **Lehel,** peab kaitsta privaatvõti. Kui valite parooli kasutada, veenduge, et salvestada või parool, mida saate seada selle serdi meeles pidada. Klõpsake nuppu **edasi**.

5. Klõpsake selle **faili eksportida**, **liikuge** asukohta, kuhu soovite eksportida sert. Serdi faili nimi **faili nimi**. Klõpsake nuppu **edasi**.

6. Klõpsake nuppu **valmis** serdi eksportida.  

### <a name="part-3---install-a-client-certificate"></a>Osa 3 - kliendi serdi installimine

Iga kliendi, mida soovite ühendada virtuaalse võrgu punkti saidi ühenduse abil peab olema installitud kliendi serti. See tunnistus on lisaks vaja VPN konfiguratsiooni pakett. Järgmised toimingud sõelub kliendi serdi käsitsi installimiseks.

1. Leidke ja kopeerige klientarvuti *pfx* -fail. Topeltklõpsake kliendi arvutisse installimiseks *pfx* -faili. **Praeguse kasutaja** **Poe asukoht** jätta ja seejärel klõpsake nuppu **edasi**.

2. Imporditav **fail** lehe, ärge tehke muudatused. Klõpsake nuppu **edasi**.

3. **Privaatvõti kaitse** lehel sisestage parool sertifikaadi, kui kasutasite, või kas turvalisus põhisumma, mis on selle serdi installimiseks on õige ja seejärel klõpsake nuppu **edasi**.

4. **Serdi poe** lehe, jätke vaikeasukoha ja seejärel klõpsake nuppu **edasi**.

5. Klõpsake nuppu **valmis**. **Turvahoiatus** serdi installimiseks klõpsake nuppu **Jah**. Serdi on nüüd edukalt imporditud.

## <a name="next-steps"></a>Järgmised sammud

Jätkake oma saidi punkti konfigureerimine. 

- **Ressursihaldur** juurutamise mudeli juhised leiate teemast [konfigureerimine punkti saidi ühendus on VNet PowerShelli kaudu](vpn-gateway-howto-point-to-site-rm-ps.md). 
- **Klassikaline** juurutamise mudeli juhised leiate teemast [VPN-ühenduse konfigureerimine SharePointi saidil on VNet klassikaline portaalis](vpn-gateway-point-to-site-create.md).