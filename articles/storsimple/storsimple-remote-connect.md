<properties 
   pageTitle="Seadme StorSimple kaugühenduse teel ühenduse | Microsoft Azure'i"
   description="Selgitatakse, kuidas konfigureerida oma seadme kaughalduse ja kuidas luua ühendus Windows PowerShelli StorSimple HTTP- või HTTPS-i kaudu."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="alkohli" />

# <a name="connect-remotely-to-your-storsimple-device"></a>Kaugühenduse teel ühenduse StorSimple seadme

## <a name="overview"></a>Ülevaade

Windows PowerShelli remoting abil saate oma StorSimple seadmega ühendada. Kui loote ühenduse nii, te ei näe menüü. (Kuvatakse menüüs ainult juhul, kui kasutate järjestikune konsooli seadme ühenduse.) Windows PowerShelli remoting, saate luua ühenduse teatud runspace. Saate määrata ka kuvamiskeelt. 

Teie seadme haldamine Windows PowerShelli remoting abil kohta lisateabe saamiseks minge [StorSimple StorSimple seadme haldamiseks Windows PowerShelli kasutamine](storsimple-windows-powershell-administration.md).

Selle õpetuse selgitatakse, kuidas konfigureerida oma seadme kaughalduse ja kuidas luua ühendus Windows PowerShelli StorSimple. Saate ühendada kaudu Windows PowerShelli remoting HTTP- või HTTPS. Kuidas luua ühendus Windows PowerShelli StorSimple otsustamisel arvestage järgmisega. 

- Ühenduse seadme järjestikune konsooli on turvaline, kuid ühenduse järjestikune konsooli üle võrgu parameetrid pole. Olge ettevaatlik, ohtu turvalisusele seadme järjestikune konsooli ühendamisel üle võrgu parameetrid. 

- Ühenduse loomine mõne seansi HTTP kaudu pakkuda rohkem kui üle võrgu kaudu järjestikune konsooli ühenduse turvalisus. Kuigi see pole kõige ebaturvalisem viis, on aktsepteeritav usaldusväärsete võrkudes. 

- Ühenduse loomine mõne HTTPS seansi iseallkirjastatud serdiga kaudu on kõige turvalisem ja soovitatav suvand.

Saate luua ühenduse kaugühenduse teel Windows PowerShelli kasutajaliidese. Kaugjuurdepääs StorSimple seadme kaudu Windows PowerShelli kasutajaliideses on lubatud vaikimisi. Peate lubama kaughalduse seadme esmalt, ja seejärel kliendi kasutavad juurdepääsu seadme.

Selles artiklis kirjeldatud juhised läbi host operatsioonisüsteemis Windows Server 2012 R2.

## <a name="connect-through-http"></a>Ühenduse loomine HTTP kaudu

Ühenduse loomine Windows PowerShelli StorSimple mõne seansi HTTP kaudu pakub rohkem turvalisust, kui ühenduse järjestikune konsooli StorSimple seadme kaudu. Kuigi see pole kõige ebaturvalisem viis, on aktsepteeritav usaldusväärsete võrkudes.

Saate konfigureerida kaughalduse Azure klassikaline portaali või järjestikune konsooli. Valige järgmised toimingud:

- [Azure'i klassikaline portaali abil saate lubada kaughalduse http kaudu](#use-the-azure-classic-portal-to-enable-remote-management-over-http)

- [Järjestikune konsooli abil saate lubada kaughalduse http kaudu](#use-the-serial-console-to-enable-remote-management-over-http)

Kui olete lubanud kaughalduse, saate järgmiste toimingute ette valmistada kliendi kaugühenduse.

- [Kaugtöölaua kliendi ettevalmistamine](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a>Azure'i klassikaline portaali abil saate lubada kaughalduse http kaudu 

Azure klassikaline portaalis http kaughalduse lubamiseks tehke järgmist.

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a>Lubamiseks kaughalduse Azure klassikaline portaali kaudu

1. Juurdepääs **seadmete** > seadme**konfigureerimine** .

2. Liikuge kerides jaotiseni **Kaughalduse** .

3. Määrake **kaughalduse lubamine** väärtuseks **Jah**.

4. Nüüd saate ühendada HTTP kaudu. (Vaikimisi on ühenduse https). Veenduge, et HTTP oleks valitud.

    >[AZURE.NOTE] Ühenduse loomine üle HTTP on lubatud ainult usaldusväärsete võrkudes.

6. Klõpsake lehe allosas **salvestada** .

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Järjestikune konsooli abil saate lubada kaughalduse http kaudu

Seadme järjestikune konsooli kaughalduse lubamiseks tehke järgmist.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Lubamiseks kaughalduse järjestikune konsooli seadme kaudu

1. Menüü järjestikune, valige suvand 1. Avage seadmes järjestikune konsooli kasutamise kohta lisateabe saamiseks [ühenduse loomine Windows PowerShelli StorSimple seadme järjestikune konsooli kaudu](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. Vastava viiba kuvamisel tippige:`Enable-HcsRemoteManagement –AllowHttp`

3. Teid teavitatakse turvahaavatavusi, kasutades HTTP seadmega kohta. Kui kuvatakse vastav viip, kinnitage, tippides **Y**.

4. Veenduge, et HTTP on lubatud, tippides.`Get-HcsSystem`

5. Veenduge, et väljal **RemoteManagementMode** kuvatakse **HttpsAndHttpEnabled**. Järgmisel pildil kuvatakse nende sätete PuTTY.

     ![Järjestikune HTTPS ja HTTP lubatud](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>Kaugtöölaua kliendi ettevalmistamine

Kliendi kaughalduse lubamiseks tehke järgmist.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Kaugtöölaua kliendi ettevalmistamine

1. Windows PowerShelli seansi käivitamine administraatorina.

2. Tippige järgmine käsk StorSimple seadme IP-aadress kliendi usaldusväärsete hostide loendisse lisamiseks. 

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

     Asendage <*device_ip*> IP-aadress seadme; näiteks: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Tippige järgmine käsk Salvesta seadme mandaadi muutujana. 

     *$cred = get-mandaati*

4. Klõpsake kuvatavas dialoogiboksis:

    1. Sisestage kasutajanimi vormingus: *device_ip\SSAdmin*.
    2. Tippige seadme administraatori parooli, kui seade on konfigureeritud häälestusviisardi määratud. Vaikimisi parool on *parool1*.

7. Windows PowerShelli seansi käivitamine seadmes, tippides selle käsu:

     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`

     >[AZURE.NOTE] Windows PowerShelli jaoks seansi loomiseks StorSimple virtuaalse seadmega lisamine soovitud `–Port` parameeter ja määrake avaliku port, mille konfigureerisite Remoting StorSimple virtuaalse seadme.

     Selles etapis teil on aktiivne Windows PowerShelli Kaugseanss seadmele.

    ![PowerShelli remoting HTTP kaudu](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>HTTPS-i kaudu ühenduse loomine

Ühenduse loomine Windows PowerShelli StorSimple on HTTPS seansi kaudu on kõige turvaline ja Soovitatavad meetod kaugühenduse teel ühenduse loomine Microsoft Azure StorSimple seadme. Järgnevalt selgitatakse häälestada nii, et saate kasutada HTTPS-i abil ühenduse loomine Windows PowerShelli StorSimple järjestikune konsooli ja kliendi arvutisse.

Saate konfigureerida kaughalduse Azure klassikaline portaali või järjestikune konsooli. Valige järgmised toimingud:

- [Azure'i klassikaline portaali abil saate lubada kaughalduse https](#use-the-azure-classic-portal-to-enable-remote-management-over-https)

- [Järjestikune konsooli abil saate lubada kaughalduse https](#use-the-serial-console-to-enable-remote-management-over-https)

Kui olete lubanud kaughalduse, abil järgmistest toimingutest selle hosti ettevalmistamine serveri haldamist ja ühendage seade remote Host.

- [Kaughalduse selle hosti ettevalmistamine](#prepare-the-host-for-remote-management)

- [Ühenduse loomine remote Host seade](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a>Azure'i klassikaline portaali abil saate lubada kaughalduse https

Azure'i klassikaline portaali https kaughalduse lubamiseks tehke järgmist.

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a>Lubamiseks kaughalduse https Azure klassikaline portaalis

1. Juurdepääs **seadmete** > seadme**konfigureerimine** .

2. Liikuge kerides jaotiseni **Kaughalduse** .

3. Määrake **kaughalduse lubamine** väärtuseks **Jah**.

4. Nüüd saate ühenduse loomiseks kasutada HTTPS. (Vaikimisi on ühenduse https). Veenduge, et valitud oleks HTTPS. 

5. Klõpsake nuppu **allalaadimine kaughalduse sert**. Määrake asukoht, kuhu soovite faili salvestada. Peate selle serdi installimiseks kliendi või host arvutites, kuhu te kasutate ühenduse loomiseks kasutatav seade.

6. Klõpsake lehe allosas **salvestada** .

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Järjestikune konsooli abil saate lubada kaughalduse https

Seadme järjestikune konsooli kaughalduse lubamiseks tehke järgmist.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Lubamiseks kaughalduse järjestikune konsooli seadme kaudu

1. Menüü järjestikune, valige suvand 1. Avage seadmes järjestikune konsooli kasutamise kohta lisateabe saamiseks [ühenduse loomine Windows PowerShelli StorSimple seadme järjestikune konsooli kaudu](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. Vastava viiba kuvamisel tippige: 

     `Enable-HcsRemoteManagement`

    See peaks võimaldama HTTPS seadmes.

3. Veenduge, et HTTPS on aktiivne, tippides. 

     `Get-HcsSystem`

    Veenduge, et väljal **RemoteManagementMode** kuvatakse **HttpsEnabled**. Järgmisel pildil kuvatakse nende sätete PuTTY.

     ![Järjestikune HTTPS lubatud](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)

4. Väljundi `Get-HcsSystem`, kopeerige seadme järjenumbri ja edaspidiseks kasutamiseks salvestada.

    >[AZURE.NOTE] Serdi CN nime kaardid järjenumbri.

5. Serdi kaughalduse hankimiseks tippimist. 
 
     `Get-HcsRemoteManagementCert`

    Kuvatakse järgmise sisuga sert.

    ![Serdi kaughalduse](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)

5. Kopeerige teave serdi kaudu **---ALUSTAGE serdi---** **---** lõpetamiseks serdi---tekstiredaktorisse nt Notepad ja salvestage see CER-fail nimega. (Saate kopeerida selle faili oma remote Host selle hosti ettevalmistamisel.)

    >[AZURE.NOTE] Luua uus sertifikaat, kasutage funktsiooni `Set-HcsRemoteManagementCert` cmdlet-käsk.

### <a name="prepare-the-host-for-remote-management"></a>Kaughalduse selle hosti ettevalmistamine

Kaugtöölaua, mis kasutab mõnda HTTPS seansi hosti arvuti ettevalmistamine, tehke järgmist.

- [Impordi juurkausta salvestuskohta kliendi või remote host CER-fail](#to-import-the-certificate-on-the-remote-host).

- [Seadme järjenumbreid oma serveri hosti hosts-faili lisamine](#to-add-device-serial-numbers-to-the-remote-host).

Kõiki neid toiminguid kirjeldatakse allpool.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Serdi remote Host importimiseks

1. Paremklõpsake CER-fail ja valige **serdi installimine**. See käivitab serdi impordiviisardi.

    ![Serdi impordiviisardi 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)

2. **Poe asukoht**, valige **Kohaliku arvuti**ja seejärel klõpsake nuppu **edasi**.

3. Valige **kõik serdid järgmises salves paigutada**ja seejärel klõpsake nuppu **Sirvi**. Liikuge root poodi oma remote Host ja seejärel klõpsake nuppu **edasi**.

    ![Serdi impordiviisardi 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)

4. Klõpsake nuppu **valmis**. Kuvatakse teade, mis ütleb teile importimine õnnestus.

    ![Serdi impordiviisardi 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Remote hosti seadme järjenumbreid lisamiseks

1. Käivitage Notepad administraatorina ja avage fail asub \Windows\System32\Drivers\etc hosts.

2. Lisage järgmised kolm kirjed hosts faili: **andmete 0 IP-aadress**, **domeenikontrolleri 0 fikseeritud IP-aadressi**ja **selle domeenikontrolleri 1 fikseeritud IP-aadress**.

3. Sisestage seadme järjenumbri, mis on salvestatud mõnes varasemas versioonis. Vastendage see IP-aadress, nagu on näidatud järgmisel pildil. Selle domeenikontrolleri 0 ja 1 kontrolleril, lisab **Controller0** ja **Controller1** lõpus järjenumber (CN nimi).

    ![Hosts-faili CN nime lisamine](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)

4. Hosts-faili salvestamine.

### <a name="connect-to-the-device-from-the-remote-host"></a>Ühenduse loomine remote Host seade

Sisestage oma seadmes on SSAdmin seansi remote host või kliendi Windows PowerShelli ja SSL-i abil. Seansi SSAdmin kaardid seadme [järjestikune konsooli](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console) menüü suvand 1.

Arvutis, kust soovite teha kaugühenduse Windows PowerShelli kaudu tehke järgmist.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Sisestada ka SSAdmin seansi seadme Windows PowerShelli ja SSL-i abil

1. Windows PowerShelli seansi käivitamine administraatorina.

2. Seadme IP-aadressi lisamine kliendi usaldusväärsete hostide, tippides:

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

    Kus asub <*device_ip*> IP-aadress seadme; näiteks: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Tippides uue mandaadi loomiseks tehke järgmist. 

     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`

    Kui <*seadme target IP*> on IP-aadressi andmed 0 seadme; näiteks **10.126.173.90** nagu eelmisel pildil hosts-faili. Lisaks pakkumise teie seadmele administraatori parool.

4. Tippides seansi loomiseks tehke järgmist.

     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`

    Cmdlet - arvutinimi parameetri ette <*järjenumbri target seadme*>. Selle järjenumbri vastendatud andmed 0 hosts-faili oma serveri hostis IP-aadress näiteks **SHX0991003G44MT** nagu on näidatud järgmisel pildil.

5. Tüüp: 

     `Enter-PSSession $session`

6. Peate oodake mõni minut ja seejärel saate ühendatakse seadme kaudu HTTPS SSL. Kuvatakse teade, mis näitab, et olete loonud ühenduse seadme.

    ![PowerShelli remoting HTTPS ja SSL-i abil](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Järgmised sammud

- Lisateave [StorSimple seadme haldamine Windows PowerShelli abil](storsimple-windows-powershell-administration.md).

- Lisateavet [hübriidjuurutuses StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
