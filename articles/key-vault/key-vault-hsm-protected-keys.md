<properties
    pageTitle="Kuidas luua ja üle HSM kaitstud klahvid Azure'i klahvi Vault | Microsoft Azure'i"
    description="Selles artiklis abil saate kavandada, luua ja edastamine oma HSM kaitstud klahvid, mida kasutada Azure klahvi Vault."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Kuidas luua ja kanda HSM kaitstud Azure klahvi Vault võtmed

##<a name="introduction"></a>Sissejuhatus

Lisatud tagamiseks, kui kasutate Azure klahvi Vault, saate importida või riistvara turvalisus moodulid (HSMs), mis jäta HSM äärist võtmed. Selle stsenaariumi nimetatakse sageli *tuua oma klahv*või BYOK. Funktsiooni HSMs on FIPS 140-2 tase 2 kinnitatud. Azure'i klahvi Vault kasutab ta nShield pere HSMs kaitsta oma võtmed.

Kasutage teavet selles teemas aitavad kavandada, luua ja edastamine oma HSM kaitstud klahvid, mida kasutada Azure klahvi Vault.

See funktsioon pole saadaval Azure Hiina. 

>[AZURE.NOTE] Azure'i klahvi Vault kohta leiate lisateavet teemast [mis on Azure klahvi Vault?](key-vault-whatis.md)  
>
>Saamisega alustamise õpetus, mis sisaldab loomiseks olulisi vault HSM kaitstud võtmed, teemast [Azure klahvi Vault alustamine](key-vault-get-started.md).

Lisateabe saamiseks genereerimine ja üle mõne HSM kaitstud vajutamisega Interneti kaudu:

- Saate luua võti ühenduseta töökoha, mis vähendab rünnak pind.

- Võti on krüptitud koos on klahvi Exchange'i Key (KEK), mis jääb alles, kuni see on kantud Azure'i klahvi Vault HSMs krüptitud. Ainult krüptitud versiooni tootenumbri jätab algse töökoha.

- Funktsiooni tööriistakomplekt komplekti rentniku tootevõti, seob tootevõti Azure'i klahvi Vault maailma atribuudid. Nii pärast Azure klahvi Vault HSMs vastu ja dekrüptida tootevõti, ainult need HSMs saate seda kasutada. Tootevõtit ei saa eksportida. See on jõustatud ta HSMs.

- Selle klahvi Exchange'i Key (KEK) võtme krüptimiseks kasutatavat luuakse Azure'i klahvi Vault HSMs sees ja pole eksporditav. Funktsiooni HSMs Jõusta saab KEK väljaspool olevat HSMs pole selge versiooni. Lisaks on tööriistakomplekt sisaldab kinnituse ta, et selle KEK pole eksporditav ja ehtne HSM, et toodeti, sest sees on loodud kaudu.

- Funktsiooni tööriistakomplekt sisaldab kinnituse, et Azure'i klahvi Vault maailma Genereeriti ka ehtne HSM, sest valmistanud ta kaudu. See kinnitus osutub teile, et Microsoft kasutab ehtne riistvara.

- Microsoft kasutab eraldi KEKs ja eraldi turvalisus maailma geograafiliste piirkondade. See eraldamine tagab, et tootevõtit saab kasutada ainult andmekeskuste piirkonna, kus teil krüptitud. Näiteks klahvi Euroopa kliendi ei saa kasutada andmekeskuste Põhja-Ameerika või Aasia.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>Ta HSMs ja Microsofti teenuste kohta lisateavet

Ta e-turvalisuse on ees globaalne pakkuja andmete krüptimine ja cyber turvalisus lahendused finants kõrge tehnoloogia tootmisprotsessi, Governmenti ja sektorile. 40-aastane tulemusi kaitsta ettevõtte ja government teavet, kasutatakse nelja suurima kujutatud ja kosmosesõidukite viiele ta lahendusi. Nende lahendused kasutavad ka 22 NATO riigis ja secure worldwide makse tehinguid rohkem kui 80%.

Microsoft on koostööd ta HSMs olek pilt suurendada. Need täiustused võimaldavad teil saada tüüpilised eelised majutatud teenuste ilma minetamata üle oma võtmed. Täpsemalt need täiustused võimaldavad hallata nii, et te ei pea selle HSMs Microsoft. Nimega pilveteenus Azure klahvi Vault skaala lühiajaliselt ettevõtte kasutus diagrammi täita. Samal ajal tootevõti on kaitstud Microsofti HSMs sees: saate säilitada kontrolli võtme elutsükli, kuna teil luua võti ja kanda Microsofti HSMs.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Rakendamise Azure'i klahvi Vault tuua oma võti (BYOK)

Kasutage järgmist teavet ja juhiseid, kui teil luua oma HSM kaitstud võti ja Azure klahvi Vault edastamine – selle too milline olukord teie omaga võti (BYOK).


##<a name="prerequisites-for-byok"></a>Eeltingimuste BYOK

Leiate järgmisest tabelist eeltingimuste loendi Azure'i klahvi Vault tuua oma võti (BYOK).

|Nõue|Lisateave|
|---|---|
|Azure'i tellimust|Mõne Azure'i klahvi Vault loomiseks peate tellimuse Azure: [tasuta prooviversiooni kasutajaks](https://azure.microsoft.com/pricing/free-trial/)|
|Azure'i klahvi Vault Premium teenuse kiht toetamiseks HSM kaitstud võtmed|Lisateavet Azure klahvi Vault jaoks teenuse astme ja võimaluste kohta leiate [Azure'i klahvi Vault hinnad](https://azure.microsoft.com/pricing/details/key-vault/) veebisaidilt.|
|Ta HSM, kiipkaartide ja tarkvara|Teil peab olema juurdepääs ta riistvara turvalisus mooduli ja ta HSMs funktsionaalseid põhiteadmisi. Vaadake [Ta riistvara turvalisus mooduli](https://www.thales-esecurity.com/msrms/buy) loendi ühilduvad mudelid või osta HSMI, kui teil on üks.|
|Järgmised riist- ja tarkvara:<ol><li>X ühenduseta x64 töökoha minimaalne Windowsi operatsioonisüsteemi Windows 7 ja ta nShield tarkvara, mis on vähemalt versiooni 11.50.<br/><br/>Kui see töökoha töötab Windows 7, peate [installima Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Töökoha, mis on Internetiga ühendatud ja on minimaalne Windowsi operatsioonisüsteemi Windows 7.</li><li>USB-draivile või kaasaskantavas seadmes, millel on vähemalt 16 MB vaba ruumi.</li></ol>|Soovitame turvalisuse huvides esimese töökoha on ühendatud võrku. Siiski soovitusega ei programmiliselt jõustada.<br/><br/>Pange tähele, et järgige juhiseid, klõpsake selle töökoha nimetatakse lahti töökoha.</p></blockquote><br/>Lisaks kui rentniku tootevõti on tootmine võrgu, soovitame kasutada teise, eraldi töökoha alla laadida soovitud tööriistakomplekt rentniku võti. Kuid katsetamiseks, saate kasutada sama töökoha esimene.<br/><br/>Pange tähele, et järgige juhiseid, klõpsake selle teise töökoha nimetatakse töökoha Interneti-ühendusega.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Luua ja Azure klahvi Vault HSM tootevõti üle

Kasutate viis järgmist luua ja Azure klahvi Vault HSM tootevõti ülekandmiseks:

- [Samm 1: Interneti-ühendusega töökoha ettevalmistamine](#step-1-prepare-your-internet-connected-workstation)
- [Samm 2: Lahti töökoha ettevalmistamine](#step-2-prepare-your-disconnected-workstation)
- [Samm 3: Luua oma võti](#step-3-generate-your-key)
- [Samm 4: Ettevalmistamine võtme edastamine](#step-4-prepare-your-key-for-transfer)
- [Juhis 5: Azure'i klahvi Vault tootevõtit üleviimine](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Samm 1: Interneti-ühendusega töökoha ettevalmistamine
See esimene samm tehke järgmised toimingud sisse oma töökoha, mis on Interneti-ühendus.


###<a name="step-11-install-azure-powershell"></a>Samm 1.1: Azure'i PowerShelli installimine

Interneti-ühendusega töökoha kaudu alla ja installige Azure'i PowerShelli moodul, mis sisaldab haldamiseks Azure'i klahvi Vault cmdlet-käsud. Selleks on vaja vähemalt versiooni 0.8.13.

Installimise juhiste saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

###<a name="step-12-get-your-azure-subscription-id"></a>Samm 1.2: Teie Azure tellimuse ID hankimine

Mõne Azure PowerShelli seansi käivitamine ja Azure'i kontosse sisse logida, kasutades järgmine käsk:

        Add-AzureAccount
Hüpikakende brauseriaknas, sisestage oma Azure'i konto kasutajanimi ja parool. Seejärel kasutage käsku [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) :

        Get-AzureSubscription
Alates väljund, kasutate Azure klahvi Vault Tellimuse ID leidmine. Peate selle tellimuse ID hiljem.

Sulgege Azure PowerShelli aken.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Samm 1.3: Azure'i klahvi Vault BYOK tööriistakomplekt allalaadimiseks

Minge Microsoft Download Center ja geograafilised piirkond või Azure eksemplari [allalaadimine Azure'i klahvi Vault BYOK tööriistad](http://www.microsoft.com/download/details.aspx?id=45345) . Järgmise teabe abil saate tuvastada alla laadida ja selle vastavate SHA 256 paketi räsi paketi nimi:

---

**Põhja-Ameerika:**

BYOK KeyVault tööriistad United States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Euroopa:**

KeyVault BYOK-tööriistade Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Aasia:**

KeyVault BYOK-tööriistade AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Ladina-Ameerika:**

KeyVault BYOK-tööriistade LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Jaapan:**

KeyVault BYOK-tööriistade Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Austraalia:**

KeyVault BYOK-tööriistade Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure'i Government:**](https://azure.microsoft.com/features/gov/)

KeyVault BYOK-tööriistade USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Kanada:**

KeyVault BYOK-tööriistade Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Saksamaa:**

KeyVault BYOK-tööriistade Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**India:**

KeyVault BYOK-tööriistade India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

Kinnitage oma allalaaditud BYOK Tööriistad: Azure'i PowerShelli seansi tagamine kasutage [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet-käsk.

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Funktsiooni tööriistakomplekt sisaldab järgmist:

- Klahv Exchange'i Key (KEK) paketi, mille nimi algab **BYOK-KEK - pkg-.**
- Turvalisus World paketi, mille nimi algab **BYOK-SecurityWorld - pkg-.**
- Python skript nimega v**erifykeypackage.py.**
- Käsurea täitmisfail nimega **KeyTransferRemote.exe** ja seotud dll-teekidega.
- Visual C++ Redistributable Package, nimega **vcredist_x64.exe.**

Kopeerige pakett USB-draivile või muude kaasaskantavas.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Samm 2: Lahti töökoha ettevalmistamine

See teise samm tehke järgmised toimingud sisse töökoha, mis on ühendatud võrku (Interneti-ühendus või oma ettevõtte võrku).


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Samm 2.1: Ettevalmistamine lahti töökoha, kus ta HSM

NCipher (ta) tugi tarkvara installimine Windowsi arvutisse ja seejärel selle manusena ta HSM arvuti.

Veenduge, et ta tööriistad on teie Path (**%nfast_home%\bin** ja **%nfast_home%\python\bin**). Tippige näiteks järgmist:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Lisateavet leiate kaasas ta HSM.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Samm 2.2: Lahti töökoha BYOK tööriistakomplekt installimine

USB-draivile või muude kaasaskantavas BYOK tööriistakomplekt paketi kopeerida, ja seejärel tehke ühte järgmistest:

1. Failid ekstraktida allalaaditud pakett mis tahes kausta.
2. Selles kaustas, käivitage vcredist_x64.exe.
3. Järgige juhiseid installi Visual C++ käitusaja komponendid Visual Studio 2013.

##<a name="step-3-generate-your-key"></a>Samm 3: Luua oma võti

Tehke see kolmas toiming, klõpsake lahti töökoha järgmistest toimingutest.

###<a name="step-31-create-a-security-world"></a>Samm 3,1: Luua maailma turvalisus

Käivitage käsuviip ja ta uue maailma programmi käivitamine.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

See programm loob **Turvalisus World** faili % NFAST_KMDATA%\local\world, mis vastab C:\ProgramData\nCipher\Key halduse Settings\User kausta. Saate kasutada erinevate väärtuste kvoorumi, kuid selles näites teil palutakse sisestada kolm tühja kaarti ja PIN igale kirjele. Klõpsake mis tahes kaartide anda täielik juurdepääs turvalisus maailma. Nendel kaartidel muutuvad soovitud **Administraatori kaardi määrata** uue turvalisus World.

Seejärel tehke järgmist.

- Maailma failist varukoopia. Turvaline kaitse maailma faili, kaardid administraator ja nende sõrmed ja veenduge, et pole inimene on rohkem kui üks kaart juurdepääsu.

###<a name="step-32-validate-the-downloaded-package"></a>Samm 3,2: Kinnitage allalaaditud pakett

See toiming on valikuline, kuid soovitatav, et te saate kontrollida järgmiselt:

- Võti Exchange klahvi, mis sisaldab soovitud tööriistad on loodud ehtne ta HSM kaudu.
- Ehtne ta HSM on loodud räsi turvalisus maailmas, mis sisaldab soovitud tööriistad.
- Klahv Exchange'i oluline on eksporditav.

>[AZURE.NOTE]Allalaaditud pakett kinnitamiseks on HSM peab olema ühendatud, sisse lülitatud, ning peavad olema turvalisus maailma seda (nt teie loodud ainult üks).

Kinnitamiseks allalaaditud pakett:

1.  Käivitage skript verifykeypackage.py, ühte järgmistest, sõltuvalt teie geograafilised piirkond või eksemplari Azure'i sidumine:
    - Põhja-Ameerikas:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - Euroopa:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Aasia:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Ladina-Ameerika jaoks

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - Jaapan:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Austraalia:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - [Azure'i Government](https://azure.microsoft.com/features/gov/), mis kasutab USA valitsuse eksemplari Azure:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Kanada puhul:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - Saksamaa:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - India:

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]Ta tarkvara sisaldab python %NFAST_HOME%\python\bin veebisaidil

2.  Veenduge, et näete järgmist teavet, mis näitab, et eduka valideerimine: **tulem: edu**

See skript kinnitatakse allkirjastaja ahelas kuni ta juur võtit. Räsi juurkausta klahvi on manustatud skripti ja selle väärtus peab olema **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Seda väärtust saate kinnitada ka eraldi [ta veebisaidi](http://www.thalesesec.com/).

Nüüd olete valmis looma uue tootenumbri.

###<a name="step-33-create-a-new-key"></a>Samm 3.3: Looge uus võti

Ta **generatekey** programmi abil luua võti.

Võtme genereerimiseks järgmine käsk:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Kui käivitate selle käsu, kasutage neid juhiseid.

- Parameetri *kaitse* peab olema seatud väärtus **moodul**, nagu on näidatud. See loob mooduli kaitstud klahvi. BYOK tööriistad ei toeta OCS kaitstud võtmed.

- Asendage väärtus *contosokey* **ident** ja **plainname** mis tahes stringiväärtus. Valitsemiskulude minimeerimine ja vähendada tõrkeid, soovitame kasutada sama väärtuse mõlemad. **Ident** väärtus peab sisaldama ainult arve, kriipsud ja väiketähti.

- Funktsiooni pubexp jäetakse tühi (vaikimisi) selles näites, kuid saate määrata kindlad väärtused. Lisateabe saamiseks vaadake ta dokumentatsioonist.

See käsk loob Tokenized klahvi faili kaustas %NFAST_KMDATA%\local alustades **key_simple_**, järgneb **ident** käsk määratud nimega. Näide: **key_simple_contosokey**. See fail sisaldab mõnda krüptitud võti.

Varundage selle Tokenized võti faili turvalises kohas.

>[AZURE.IMPORTANT] Kui teisaldate hiljem tootevõti Azure'i klahvi Vault, Microsoft ei saa eksportida see võti tagasi saate nii, et see muutub äärmiselt oluline varundada oma võti ja turvalisus maailma turvaline. Pöörduge ta juhised ja head tavad varundada tootevõtit.

Nüüd olete valmis Azure'i klahvi Vault tootevõti edastada.

##<a name="step-4-prepare-your-key-for-transfer"></a>Samm 4: Ettevalmistamine võtme edastamine

See Neljas samm tehke järgmised toimingud lahti töökoha kohta.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Samm 4.1: Vähendatud õigustega võtme koopia loomine

Käivitage vähendamiseks käsureale õiguste tootevõtit, klõpsake ühte järgmistest, sõltuvalt teie geograafilised piirkond või eksemplari Azure

- Põhja-Ameerikas:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- Euroopa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Aasia:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Ladina-Ameerika jaoks

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- Jaapan:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Austraalia:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- [Azure'i Government](https://azure.microsoft.com/features/gov/), mis kasutab USA valitsuse eksemplari Azure:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Kanada puhul:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- Saksamaa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- India:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Kui käivitate selle käsu, Asendage *contosokey* määratud sama väärtusega **Samm 3.3: luua uue tootenumbri** juhises [Genereeri tootevõtit](#step-3-generate-your-key) .

Teil palutakse oma turvalisus maailma administraator kaartide lisandmoodul.

Kui käsk on lõpule jõudnud, näete **tulem: edu** ja piiratud õigustega tootenumbri koopia on faili nimega key_xferacId_<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>Samm 4.2: Kontrolli uue koopia võti

Soovi korral käivitage selle ta Utiliidid kinnitamiseks uue tootenumbri minimaalsete õigusi:

- aclprint.py:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Need käsud käivitamisel asendamine contosokey määratud sama väärtusega **Samm 3.3: luua uue tootenumbri** juhises [Genereeri tootevõtit](#step-3-generate-your-key) .

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Samm 4.3: Krüptimine tootevõtit Microsofti Key Exchange võti

Käivitage ühte järgmistest käskudest, olenevalt teie geograafilised piirkonnast või Azure eksemplari.

- Põhja-Ameerikas:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Euroopa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Aasia:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Ladina-Ameerika jaoks

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Jaapan:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Austraalia:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- [Azure'i Government](https://azure.microsoft.com/features/gov/), mis kasutab USA valitsuse eksemplari Azure:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Kanada puhul:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Saksamaa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- India:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Kui käivitate selle käsu, kasutage neid juhiseid.

- Asendage *contosokey* identifikaator, mida kasutasite loomiseks sisestage **Samm 3.3: luua uue tootenumbri** juhises [Genereeri tootevõtit](#step-3-generate-your-key) .

- Asendage *SubscriptionID* Azure'i tellimus, mis sisaldab teie olulisi vault ID-ga. Saate tuua selle väärtuse varem, **Samm 1.2: teie Azure tellimuse ID hankimine** [ettevalmistamine oma Interneti-ühendusega töökoha](#step-1-prepare-your-internet-connected-workstation) juhises.

- Asendage *ContosoFirstHSMKey* silt, mida kasutatakse teie väljundi faili nimi.

Kui see on lõpule jõudnud edukalt, kuvatakse **tulem: edu** ja seal on uus fail praeguses kaustas, mis on järgmine nimi: TransferPackage -*ContosoFirstHSMkey*.byok

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Samm 4.4: Teie paketi võtme edastamine kopeerimiseks Interneti-ühendusega töökoha

Kasutage USB-draivile või muude kaasaskantavas kopeerimiseks väljundfail eelmises juhises (KeyTransferPackage ContosoFirstHSMkey.byok) oma Interneti-ühendusega töökoha.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>Juhis 5: Azure'i klahvi Vault tootevõtit üleviimine

See viimane toiming sisse töökoha Interneti-ühendusega kasutada cmdlet-käsu [Lisa-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) võtme edastamine paketi kopeeritud lahti töökoha Azure'i klahvi Vault HSM üleslaadimine:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Kui üles õnnestub, kuvatakse atribuutide klahvi, mille just lisasite.


##<a name="next-steps"></a>Järgmised sammud

Nüüd saate selle HSM kaitstud klahvi oma võtme võlvkelder. Lisateavet leiate jaotisest **kui soovite kasutada riistvara turvalisus mooduli (HSM)** [kasutamise alustamine Azure'i klahvi Vault](key-vault-get-started.md) õpetuses.
