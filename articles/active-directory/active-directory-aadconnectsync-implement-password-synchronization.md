<properties
    pageTitle="Parooli sünkroonimine Azure'i AD-ühenduse sünkroonimisprobleemide rakendamise | Microsoft Azure'i"
    description="Leiate teavet parooli sünkroonimise tööpõhimõtted ja kuidas seda lubada."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="markusvi;andkjell"/>


# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>Azure'i AD-ühenduse sünkroonimisprobleemide parooli sünkroonimise rakendamine
Selles teemas antakse te peate sünkroonima kohapealse Active Directory (AD) kasutaja paroolid on pilvepõhist Azure Active Directory (Azure AD) teavet.

## <a name="what-is-password-synchronization"></a>Mis on parooli sünkroonimine
Tõenäosus, et teil on blokeeritud saada unustatud parooli tõttu oma tööd on seotud erinevaid paroole, pidage meeles, et peate arv. Lisateavet paroolide peate pidage meeles, et mida suurem tõenäosuse unustada ühte. Küsimused ja kõnede parooli lähtestamise ja muude parooliga seotud probleemide kohta nõuda kõige kasutajatoe ressursse.

Parooli sünkroonimine on funktsioon sünkroonima kohapealse Active Directory kasutaja paroolid on pilvepõhist Azure Active Directory (Azure AD).
See funktsioon võimaldab teil Azure Active Directory teenused (nt Office 365, Microsoft Intune'i, CRM Online'i ja Azure AD domeeniteenused) sisse logida kasutades sama parool, mida kasutate oma kohapealse Active Directory sisse logida.

![Mis on Azure AD-ühenduse](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Paroolide kasutajate tuleb säilitada, et ainult üks vähendades parooli sünkroonimine abil saate:

- Oma kasutajate tööviljakust parandada.
- Kasutajatoe kulude vähendamise  

Ka, kui valite [**AD FS Liiduserveri**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect)kasutada, saate soovi korral lubada parooli sünkroonimise varukoopiana juhuks, kui teie AD FS-i taristu nurjub.

Parooli sünkroonimine on rakendatud Azure'i AD-ühenduse sünkroonimine directory sünkroonimise funktsiooni laiendamine. Keskkonna parooli sünkroonimine kasutamiseks peate:

- Installi Azure AD ühenduse loomine  
- Kataloogisünkroonimise oma kohapealse konfigureerimine AD ja oma Azure Active Directory
- Parooli sünkroonimise lubamine

Lisateavet leiate teemast [integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md)

> [AZURE.NOTE] Active Directory domeeniteenused, mis on konfigureeritud FIPS ja parooli sünkroonimise kohta leiate lisateavet teemast [parooli sünkroonimise ja FIPS](#password-synchronization-and-fips).

## <a name="how-password-synchronization-works"></a>Parooli sünkroonimise tööpõhimõtted
Teenus Active Directory domeeni talletab paroolide tegelik kasutaja parooli kujutis räsi väärtus vorm. Räsi väärtus on tulemus on ühesuunalise Matemaatikafunktsiooni (selle "*räsi algoritmi*"). Ei ole meetodit ühesuunalise funktsiooni Lihttekst versiooni parooli tulem taastada. Ei saa kasutada parooli räsi kohapealse võrgu sisse logida.

Sünkroonida oma parooli, ekstraktib Azure'i AD-ühenduse sünkroonimine oma parooli räsi kohapealse Active Directory. Täiendav turvalisus töötlemine rakendatakse parooli räsi enne, kui see on sünkroonitud Azure Active Directory autentimisteenus. Paroolid on sünkroonitud kasutaja alusel ja ajalises järjestuses.

Tegelike andmete voogu illustreerida parooli sünkroonimine on sarnane sünkroonimise kasutaja andmeid, nt DisplayName või meiliaadressid. Siiski paroolid on sünkroonitud sagedamini standard directory sünkroonimise aken Atribuudid. Parooli sünkroonimise protsess töötab iga 2 minuti järel. Saate muuta selle protsessi sagedus. Kui sünkroonite parooli, kirjutab üle pilve olemasolev parool.

Esimest korda, parooli sünkroonimise funktsiooni, tehtav esialgse sünkroonimise-ulatus kõigi kasutajate paroolide kohta. Ei saa määratleda selgesõnaliselt alamhulga kasutaja paroolid, mida soovite sünkroonida.

Kui mõni asutusesisese parooli muutmiseks, sünkroonitakse värskendatud parool paari minutiga kõige sagedamini.
Parooli sünkroonimise funktsiooni automaatselt uued katsed nurjunud sünkroonimise katsed. Kui ilmneb tõrge sünkroonimiseks parooli katse ajal, logitakse tõrge oma Sündmusevaatur.

Parooli sünkroonimise ei mõjuta parajasti sisse logitud kasutajale.
Teenus cloud praeguse seansi ei mõjuta kohe sünkroonitud parooli muutmine, mis ilmneb siis, kui olete sisse logitud pilveteenusesse. Juhul, kui pilveteenusesse nõuab teilt kinnitamiseks uuesti, peate sisestage uus parool.

> [AZURE.NOTE] Parooli sünkroonimise toetatakse ainult Active Directory objekti tüüp kasutaja. See ei toetata iNetOrgPerson objekti tüüp.

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>Azure'i AD domeeni teenuste parooli sünkroonimise tööpõhimõtted
Parooli sünkroonimise funktsiooni abil saate sünkroonida oma kohapealse paroolide [Azure AD domeeniteenused](../active-directory-domain-services/active-directory-ds-overview.md). See stsenaarium võimaldab Azure AD domeeni teenuste autentimiseks kasutajad pilveteenuses kõik meetodid, mis on saadaval asutusesisese AD. Selle stsenaariumi kogemus on sarnane kohapealse keskkonna Active Directory migreerimise tööriista (ADMT) kasutamine.

### <a name="security-considerations"></a>Turvalisuse alused
Paroolide sünkroonimisel on avatud parooli lihttekstina parooli sünkroonimise funktsiooni Azure AD, või mis tahes seotud teenuseid.

Samuti on kohapealse Active Directory ole parooli pöörduvalt krüptitud vormingus salvestada. Digest Active Directory parooli räsi kasutatakse edastamiseks vahel asutusesisese AD ja Azure Active Directory. Parooli räsi seedima ei saa kasutada juurdepääs kohapealse keskkonna ressurssidele.

### <a name="password-policy-considerations"></a>Parooli poliitika kaalutlused
On kahte tüüpi parool poliitika, mis on mõjutatud parooli sünkroonimise lubamine.

1. Paroolipoliitika keerukus
2. Parooli aegumise poliitika

**Paroolipoliitika keerukus**  
Kui lubate parooli sünkroonimise, parooli keerukuse poliitikaid oma kohapealse Active Directory alistada keerukuse poliitikaid pilveteenuses sünkroonitud kasutajate jaoks. Saate oma kohapealse Active Directory kehtiv paroole Azure AD teenuste kasutamiseks.

> [AZURE.NOTE] Otse pilveteenuses loodud kasutajate paroole on jätkuvalt parool poliitika pilves määratletud.

**Parooli aegumise poliitika**  
Kui kasutaja on ulatuse parooli sünkroonimise, pilvepõhise konto parool on seatud "*Aegumatuse*".
Logige sisse oma pilveteenustega sünkroonitud parool, mis on lõppenud, klõpsake oma kohapealse keskkonna abil saate jätkata. Pilveteenuse parooli värskendatakse järgmine kord, kui muudate parooli asutusesiseses keskkonnas.

### <a name="overwriting-synchronized-passwords"></a>Ülekirjutamise sünkroonitud paroolid
Administraator saab käsitsi Windows PowerShelli kaudu parooli lähtestada.

Sel juhul uus parool alistab sünkroonitud parool ja uus parool on rakendatud kõik parool poliitika pilves määratletud.

Kui muudate parooli kohapealse uuesti uus parool on sünkroonitud ja alistab käsitsi värskendatud parool.

## <a name="enabling-password-synchronization"></a>Parooli sünkroonimise lubamine
Parooli sünkroonimine on lubatud automaatselt, kui installite Azure'i AD-ühenduse **Sätete kiire**abil. Lisateavet leiate artiklist [Alustamine Azure'i AD-ühenduse sätete kiire abil](./connect/active-directory-aadconnect-get-started-express.md).

Kui kasutate kohandatud sätteid, kui installite Azure'i AD-ühenduse, saate lubada sisselogimise lehele kasutaja parooli sünkroonimine. Lisateavet leiate teemast [kohandatud installi Azure'i AD-ühenduse](./connect/active-directory-aadconnect-get-started-custom.md).

![Parooli sünkroonimise lubamine](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Parooli sünkroonimise ja FIPS
Kui teie server on lukus vastavalt Federal teabe töötlemise Standard (FIPS), siis MD5 on keelatud.

**MD5 parooli sünkroonimise lubamiseks tehke järgmist.**

1. Minge **%programfiles%\Azure AD Sync\Bin**.
2. Avatud **miiserver.exe.config**.
3. Minge **konfiguratsiooni/käitusaja** sõlm (faili lõpus).
4. Järgmise sõlme lisamiseks tehke järgmist.`<enforceFIPSPolicy enabled="false"/>`
5. Salvestage muudatused.

Viide, on see lõikama, kuidas peaks välja nägema.

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Turvalisus ja FIPS kohta leiate teemast [AAD parooli sünkroonimise, krüptimise ja FIPS nõuetele vastavus](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>Parooli sünkroonimise tõrkeotsing
Kui parooli ei sünkroonita oodatud viisil, see võib olla kasutajate alamhulga või kõigi kasutajate jaoks.

- Kui teil on probleeme objektide, siis lugege teemat [tõrkeotsing ühe objekti, mis ei sünkroonita paroolid](#troubleshoot-one-object-that-is-not-synchronizing-passwords).
- Kui teil on probleeme, kus pole paroolid on sünkroonitud, lugege teemat [tõrkeotsing, kus pole paroolid on sünkroonitud](#troubleshoot-issues-where-no-passwords-are-synchronized).

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>Ühe objekti, mis ei sünkroonita paroolide tõrkeotsing
Parooli sünkroonimise probleemide tõrkeotsinguks saate hõlpsalt läbivaatamise objekti olekut.

Käivitage **Active Directory kasutajad**ja arvutid. Leidke kasutaja, ja veenduge, et **kasutaja peab parooli järgmisel sisselogimisel muuta** on valimata.
![Active Directory viljakamalt paroolid](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
Kui see on valitud, siis paluge kasutajal sisse logida ja parooli muutmine. Ajutised paroolid ei sünkroonita Azure AD.

Kui näib õige Active Directory, siis järgmiseks tuleb järgida sync engine kasutaja. Pärast kasutaja kohapealse Active Directory Azure AD, näete, kui on kirjeldav tõrge objekti.

1. Käivitage **[Sünkroonimine Service Manager](active-directory-aadconnectsync-service-manager-ui.md)**.
2. Klõpsake nuppu **ühendused**.
3. Valige kasutaja asub **Active Directory konnektor** .
4. Valige **Otsi konnektori ruumi**.
5. Leidke kasutaja, mida otsite.
6. Valige vahekaart **pärandi** ja veenduge, et vähemalt üks sünkroonimine reegel kuvatakse **Parooli sünkroonimise** nagu **True**. Vaikimisi konfiguratsiooni, sünkroonimise reegli nimi on **sisse: AD - kasutaja AccountEnabled**.  
    ![Kasutaja pärandi teave](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. Seejärel [järgige kasutaja](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system) – metaversumi Azure AD konnektori ruumi. Konnektori ruumi objekti peaks olema Väljamineva reegli **Parooli sünkroonimise** , mis on seatud **tõene**. Vaikimisi konfiguratsiooni, sünkroonimise reegli nimi on **välja AAD – kasutaja liituda**.  
    ![Kasutaja atribuutide konnektori ruumi](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. Möödunud nädalal objekti parooli sünkroonimise üksikasjade kuvamiseks klõpsake **sündmuselogi …**.  
    ![Objekti logi üksikasjad](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

Veerus Olek võib olla järgmised väärtused.

Olek | Kirjeldus
---- | -----
Edu | Parool on edukalt sünkroonitud.
FilteredByTarget | **Kasutaja peab parooli järgmisel sisselogimisel**muuta on seatud parool. Parool on sünkroonitud.
NoTargetConnection | Objekti metaversumi või Azure AD konnektori ruumis.
SourceConnectorNotPresent | Pole ühtegi objekti leitud kohapealse Active Directory konnektori ruumi.
TargetNotExportedToDirectory | Objekti Azure AD konnektori ruumis pole veel eksportida.
MigratedCheckDetailsForMoreInfo | Logikirjet on loodud enne koostamine 1.0.9125.0 ja kuvatakse pärand kujul.

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>Kui ei paroolid on sünkroonitud probleemide tõrkeotsing
Käivitage jaotises [sätete sünkroonimine arvutite parooli olekuks](#get-the-status-of-password-sync-settings)skripti. See annab teile ülevaate parooli sünkroonimise konfigureerimine.  
![Sätete sünkroonimine arvutite parooli PowerShelli skripti väljund](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
Kui see funktsioon pole lubatud Azure AD või kanali sünkroonimisolek pole lubatud, siis käivitage ühendamine installimise viisard. Valige **Kohanda sünkroonimise** ja valimise parooli sünkroonimise. Selle muudatuse keelab ajutiselt funktsiooni. Seejärel käivitage viisard uuesti ja lubage parooli sünkroonimise. Käivitage skript uuesti veendumaks, et konfiguratsiooni oleks õige.

Kui skript näitab, et on pole südamelöögi ajal, käivitage skript, [päästik täieliku sünkroonimise kõik paroolid](#trigger-a-full-sync-of-all-passwords). See skript saate kasutada ka muid stsenaariumid, kus konfiguratsioon on õige, kuid paroolid ei sünkroonita.

#### <a name="get-the-status-of-password-sync-settings"></a>Sätete sünkroonimine arvutite parooli oleku hankimine

```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Kõik paroolid täieliku sünkroonimise käivitamine
Võite käivitada täieliku sünkroonimise kõik paroolid järgmise skripti abil:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Järgmised sammud

* [Azure'i AD ühenduse sünkroonimine: Kohandamise sünkroonimise suvandid](active-directory-aadconnectsync-whatis.md)
* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
