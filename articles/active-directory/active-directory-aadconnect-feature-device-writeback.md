<properties
    pageTitle="Azure'i AD-ühendus: Lubamine seadme tagasikirjutusega | Microsoft Azure'i"
    description="Selle dokumendi üksikasjad, kuidas lubada seadme tagasikirjutusega Azure'i AD-ühenduse kaudu"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure'i AD-ühendus: Lubamine seadme tagasikirjutusega

>[AZURE.NOTE] Azure'i AD Premiumi tellimuse jaoks on vaja seadme tagasikirjutusega.

Järgmised dokumendid sisaldab teavet Azure'i AD-ühenduse seadme tagasikirjutusega funktsiooni lubamiseks tehke järgmist. Seadme tagasikirjutusega kasutatakse järgmistel juhtudel:

- Tingimusvormingu põhjal seadmed ADFS-i juurdepääsu lubamine (2012 R2 või uuem versioon) kaitstud rakenduste (tuginedes tootja usalduste).

See pakub täiendavaid turbe- ja kinnitust, et juurdepääsu rakendused on antud ainult usaldusväärsete seadmed. Juurdepääsu kohta leiate lisateavet teemast [Juurdepääsu riski haldamise](active-directory-conditional-access.md) ja [kohapealse juurdepääsu abil Azure Active Directory seadme registreerimine häälestamise](https://msdn.microsoft.com/library/azure/dn788908.aspx).

>[AZURE.IMPORTANT]
<li>Seadmed peavad asuma samas mets nimega kasutajad. Kuna seadmed tuleb kirjutada tagasi ühe mets, seda funktsiooni ei toeta praegu juurutamine koos mitme kasutaja kogumit.</li>
<li>Kohapealse Active Directory kogumis saab lisada ainult ühe seadme registreerimine konfiguratsiooni objekti. See funktsioon on ei ühildu topoloogia, kus on sünkroonitud kohapealse Active Directory mitme Azure AD kataloogid.</li>

## <a name="part-1-install-azure-ad-connect"></a>Osa 1: Installi Azure AD ühenduse loomine
1. Installige Azure'i AD-ühenduse kasutamine kohandatud või kiire sätted. Microsoft soovitab alustada kõik kasutajad ja rühmad edukalt sünkroonitud enne seadme tagasikirjutusega.

## <a name="part-2-prepare-active-directory"></a>Osa 2: Ettevalmistamine Active Directory
Järgmiste juhiste abil saate seadmes tagasikirjutusega kasutamise ettevalmistamine.

1.  Arvutist, kuhu on installitud Azure'i AD-ühenduse, käivitage PowerShelli administraatoriõigustes režiimis.

2.  Kui Active Directory PowerShelli mooduli pole installitud, installida, kasutades järgmine käsk:

    `Install-WindowsFeature –Name AD-Domain-Services –IncludeManagementTools`

3. Azure Active Directory PowerShelli mooduli pole installitud, siis laadige alla ja selle installida [Azure Active Directory moodul Windows PowerShelli jaoks (64-bitine versioon)](http://go.microsoft.com/fwlink/p/?linkid=236297). See osa on sõltuvus klõpsake soovitud sisselogimisabimees, mis installitakse koos Azure'i AD-ühenduse.

4.  Ettevõtte administraatori identimisteave, käivitage järgmine käsk ja seejärel väljuge PowerShelli.

    `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Ettevõtte administraatori identimisteave on nõutavad, kuna on vaja muuta konfiguratsiooni nimeruumi. Domeeni administraator pole piisavalt õigusi.

![PowerShelli seadme tagasikirjutusega lubamine](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Kirjeldus:

- Kui need pole juba olemas, loob ja konfigureerib uue ümbriste ja objektide CN = seadme registreerimine konfiguratsiooni, CN teenuste CN = konfiguratsiooni, = [mets dn].
- Kui need pole juba olemas, loob ja konfigureerib uue ümbriste ja objektide CN = RegisteredDevices, [domeeni dn]. Seadme objektid luuakse selles ümbrises.
- Määrab Azure AD Connectori konto, teie Active Directory-seadmete haldamiseks vajalikud õigused.
- Ainult peab ühe mets, töötab ka siis, kui Azure'i AD-ühenduse on installitud mitu metsade.

Parameetrid:

- Domeeninimi: Active Directory domeeni kus luuakse seadme objekte. Märkus: Kõigis seadmetes antud Active Directory kogumis jaoks luuakse ühe domeeni.
- AdConnectorAccount: Active Directory kontot, mis kasutavad Azure'i AD-ühenduse kataloogis objektide haldamiseks. See on kasutatav sünkroonimine Azure'i AD-ühenduse loomiseks AD konto. Kui installisite kiire sätete abil, on eesliide MSOL_ konto.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Osa 3: Luba seadme tagasikirjutusega rakenduses Azure'i AD-ühenduse
Järgmiste toimingute abil saate lubada seadme tagasikirjutusega rakenduses Azure'i AD-ühenduse.

1.  Käivitage viisard installimine uuesti. Valige **sünkroonimise suvandite kohandamine** lehelt täiendavad toimingud ja klõpsake nuppu **edasi**.
![Kohandatud installi sünkroonimise suvandite kohandamine](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2.  Klõpsake lehel valikulised funktsioonid seadme tagasikirjutusega on enam olla tuhm. Palun pange tähele, et kui Azure AD ühenduse prep juhiste täitmist pole seadme tagasikirjutusega on valikulised funktsioonid lehel välja värvi. Seadme tagasikirjutusega ruut ja klõpsake nuppu **edasi**. Kui märkeruut on endiselt keelatud, vt [jaotist tõrkeotsing](#the-writeback-checkbox-is-still-disabled).
![Kohandatud installige seade tagasikirjutusega valikulised funktsioonid](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3.  Lehel tagasikirjutusega näete esitatud domeeni nimega vaikimisi seadme tagasikirjutusega mets.
![Kohandatud installi seadme tagasikirjutusega target mets](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4.  Täitke viisardi konfigureerimise muudatustega ei installi. Vajadusel viidata [kohandatud installi Azure'i AD-ühenduse.](./connect/active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Tingimusvormingu juurdepääsu lubamine
Üksikasjalikud juhised selle stsenaariumi lubamiseks on saadaval [häälestamise kohapealse juurdepääsu abil Azure Active Directory seadme registreerimine](https://msdn.microsoft.com/library/azure/dn788908.aspx).

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Kontrollige, kas seadmed sünkroonitakse Active Directory
Nüüd peaks seadme tagasikirjutusega õigesti töötada. Pange tähele, et võib kuluda kuni 3 tundi seadme objektide peaks olema kirjutada tagasi AD.  Veenduge, et teie seadmetes sünkroonitakse õigesti, tehke järgmist pärast sünkroonimine reegleid.

1.  Käivitage Active Directory halduskeskus.
2.  Laiendage RegisteredDevices, mis on on ühendatud domeenis.
![Active Directory halduskeskuses registreeritud seadmed](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3.  Praeguse registreeritud seadmed on loetletud seal.
![Active Directory halduskeskuses registreeritud seadmete loend](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Tõrkeotsing

### <a name="the-writeback-checkbox-is-still-disabled"></a>Tagasikirjutusega ruut on endiselt keelatud.
Kui ruut seadme tagasikirjutusega pole lubatud isegi juhul, kui järgisite ülal esitatud juhiseid, järgmist juhendab teid installimise viisard kinnitatava enne välja on lubatud.

Alustame esimese:

- Veenduge, et vähemalt üks mets on Windows Server 2012R2. Peab olema seadme objekti tüüp.
- Kui viisard installimine on pooleli, siis muudatused ei tuvastata. Sel juhul installimise viisardi lõpuleviimine ja käivitage see uuesti.
- Veenduge, et esitate initialization skripti konto on tegelikult õige kasutaja Active Directory konnektor kasutavad. Selle probleemi lahendamiseks tehke järgmist.
    - Avage menüü start kaudu **Sünkroonimise teenuse**.
    - Avage vahekaart **konnektorid** .
    - Otsige üles konnektor tüüpi Active Directory domeeniteenused ja valige see.
    - Klõpsake jaotises **toimingud**nuppu **Atribuudid**.
    - **Ühenduse loomine Active Directory kogumis**minek Kontrollige, kas nimi domeeni ja kasutajale määratud klõpsake selle Kuva match skripti määratud kontole.
![Konnektor konto sünkroonimine Service Manager](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Kontrollige konfiguratsiooni Active Directory:
- Veenduge, et seadme registreerimise teenust asub järgmises kohas (CN DeviceRegistrationService CN = seadme registreerimise teenuseid, CN = seadme registreerimine konfiguratsiooni, CN = teenuste CN = = konfigureerimine) jaotises konfiguratsiooni nimetamise konteksti.

![Tõrkeotsing: DeviceRegistrationService nimeruumi konfigureerimine](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

- Veenduge, otsides konfiguratsiooni nimeruum on ainult üks konfiguratsiooni objekt. Kui seal on rohkem kui üks, kustutada duplikaat.

![Tõrkeotsing, dubleeritud objektide otsimine](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

- Seadme registreerimise teenuse objekti, veenduge, et atribuudi vaevuste-DeviceLocation on olemas ja väärtuse. Otsing selles asukoht ja veenduge, et see on olemas objektitüüp vaevuste-DeviceContainer.

![Tõrkeotsing: msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Tõrkeotsing: RegisteredDevices objekti klassi](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

- Veenduge, et Active Directory konnektor kasutatav konto on vajalikud õigused oleval leitud eelmises etapis registreeritud seadmed. See on see ümbris oodatud õigusi:

![Tõrkeotsing, container õiguste kontrollimine](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

- Veenduge, et Active Directory kontot on õigused KN seadme registreerimine konfiguratsiooni, CN = teenuste CN = = konfiguratsiooni objekti.

![Tõrkeotsing, õiguste konfigureerimise seadme registreerimise kinnitamine](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Lisateave
- [Risk tingimusvormingu juurdepääsu haldamine](active-directory-conditional-access.md)
- [Kuidas häälestada kohapealse juurdepääsu abil Azure Active Directory seadme registreerimine](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
