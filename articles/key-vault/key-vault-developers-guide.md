<properties
   pageTitle="Peamised Vault Developer's Guide | Microsoft Azure"
   description="Arendajad saate hallata Microsoft Azure keskkonnas krüptovõtmete Azure võti Vault. "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/03/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-developers-guide"></a>Azure'i peamised Vault Developer's Guide
Kasutades klahvi Vault, siis saab juurdepääsu tundliku sisuga teabe jõudmist teie rakenduste turvaliselt nii, et:

- Võtmed ja saladused kaitstud ilma et ise koodi kirjutada ja siis lihtsalt saab neid kasutada oma taotlusi.
- Teil võimalik oma kliente enda ja hallata oma võtmed, nii saate pakuvad tuum tarkvara omadused. Sel viisil oma rakendused on oma vastutust ega võimalikke kohustusi kliendi üürnikule võtmed ja saladusi.
- Rakenduse saate abil allkirjastamise ja krüptimise veel hoiab Koodvõtmete haldamise välise rakenduse nii, et lahus on sobiv rakendus, mis on geograafiliselt hajutatud.

- Võti Vault viimisega September 2016 rakendused saavad nüüd kasutada võti Vault sertifikaadid. Lisateabe saamiseks vt **võtmed, saladusi, ja sertifikaatide** artikli [ülejäänud viide](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Azure võti Vault kohta leiate üldist teavet teemast [mis on võti Vault](key-vault-whatis.md).

## <a name="videos"></a>Videod
See video näitab teile, kuidas luua oma peamiste vault ja kasutusvõimaluste rakendusest "Tere võti Vault" proovi.

[AZURE.VIDEO azure-key-vault-developer-quick-start]

Ressursside mainitud video lingid:
- [Azure PowerShelli](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure'i peamised Vault kood](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

Õppida rohkem saab [Võti Vault blogi](http://aka.ms/kvblog) jälgida ja osaleda [Võti Vault Foorum](http://aka.ms/kvforum).

## <a name="creating-and-managing-key-vaults"></a>Loomise ja haldamise peamisi võlvid

Enne kui Azure võti Vault koodi, saate luua ja hallata võlvid ülejäänud, ressursihaldur Mallid, PowerShelli või CLI, nagu on kirjeldatud järgmistes artiklites:

- [Saate luua ja hallata peamisi võlvid koos ülejäänud](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [Saate luua ja hallata peamisi võlvid PowerShelliga](key-vault-get-started.md)
- [Saate luua ja hallata peamisi võlvid CLI](key-vault-manage-with-cli.md)
- [Peamised vault luua ja lisada Azure ressursihaldur malli kaudu saladus](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] Vastu peamised võlvid on autenditud läbi AAD ja volitatud vault kohta määratud võtme Vault juurdepääsu poliitika kaudu.

## <a name="coding-with-key-vault"></a>Peamised Vault kodeerimine

Võti Vault juhtimissüsteemi programmeerijatele koosneb mitu liidesed ülejäänud sihtasutusena, [Võti Vault ülejäänud API-teatmiku](https://msdn.microsoft.com/library/azure/dn903609.aspx).

Te saate, edukas loal teha järgmist:

- Hallata krüptograafilised võtmed kasutades [Loo](https://msdn.microsoft.com/library/azure/dn903634.aspx), [Import](https://msdn.microsoft.com/library/azure/dn903626.aspx), [Update](https://msdn.microsoft.com/library/azure/dn903616.aspx), [kustutage](https://msdn.microsoft.com/library/azure/dn903611.aspx) ja muude toimingute

- Hallata saladusi [saada](https://msdn.microsoft.com/library/azure/dn903633.aspx), [Update](https://msdn.microsoft.com/library/azure/dn986818.aspx), [kustutage](https://msdn.microsoft.com/library/azure/dn903613.aspx) ja muude toimingute

- Kasutada krüptovõtmete [märk](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[kontrolli](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) ja [Krüpti](https://msdn.microsoft.com/library/azure/dn878060.aspx)/[dekrüptida](https://msdn.microsoft.com/library/azure/dn878097.aspx) toimingud

Järgmiste SDK-d leiate võti Vault töötamiseks:

|[![.NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/mt765854.aspx)|[![Node.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[.NET käsitlev SDK-dokumentatsioon](https://msdn.microsoft.com/library/mt765854.aspx)|[Node.js käsitlev SDK-dokumentatsioon](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[.NET SDK Nugeti pakett](http://www.nuget.org/packages/Microsoft.Azure.KeyVault)|[Node.js SDK pakett](https://www.npmjs.com/package/azure-keyvault)|

.NET SDK versioon 2.x kohta lisateabe saamiseks vt [pressiteade märgib](key-vault-dotnet2api-release-notes.md).

## <a name="example-code"></a>Näidiskoodis
Kasutades klahvi Vault rakendustest täielik näiteid leiate:

- .NET proovi taotluse *HelloKeyVault* ja Azure web service näiteks. [Azure'i võti Vault koodi näidised](http://www.microsoft.com/download/details.aspx?id=45343)
- Õpetus, mis aitab teil õppida, kuidas kasutada Azure võti Vault Azure veebirakendus. [Kasutada Azure peamised Vault veebirakendus:](key-vault-use-from-web-application.md)

## <a name="how-tos"></a>Õpetused

Järgmised artiklid ja stsenaariumid anda ülesande juhendile Azure võti Vault töötamiseks:

- [Muuda võtme vault rentniku ID pärast tellimuse liikuda](key-vault-subscription-move-fix.md) - kui teisaldate oma Azure'i abonementide rentnikule A rentniku B, oma olemasoleva võtme võlvid ei pääse poolt koolijuhid (kasutajate ja rakenduste) ja üürnik B. parandus juhendi abil.
- [Juurdepääs võtme Vault tulemüüri taga](key-vault-access-behind-firewall.md) - juurdepääsu peamine vault peamised vault kliendi rakendus vajab juurdepääsu mitme baaskogumi puhul erinevaid funktsioone.

- [Loo ja Transfer HSM-Protected võtmed Azure võti Vault](key-vault-hsm-protected-keys.md) - nii saate kavandada, luua ja seejärel edastage oma HSM kaitstud võtmed kasutada Azure võti Vault.
- [Kuidas edasi turvalise väärtusi (nt paroole) ajal](../resource-manager-keyvault-parameter.md) - kui sa pead läbima turvaline väärtus (nt parooli) parameetrina ajal, saab Salvesta väärtus kui salajane Azure võti Vault ja viide väärtust muud ressursihaldur mallid.
- [Kuidas kasutada võti Vault laiendatav SQL serveri juhtkonna](https://msdn.microsoft.com/library/dn198405.aspx) - The SQL serveri konnektor Azure võti Vault võimaldab SQL Server ja SQL-in-a-VM võimendamiseks Azure'i võti Vault teenuse Extensible Key Management (EKM) pakkuja kaitsta krüptovõtmed taotlused link; Läbipaistev andmekrüptimist, Backup krüptimist ja veeru tasemel krüptimine.
- [Juurutamise sertifikaatide vms võtit hoidlast](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - pilve rakendus töötab VM Azure vajab. Kuidas sa saad selle serdi arvesse selle VM täna?
- [Kuidas Install Key Vault lõpuni võtme pööramine ja auditeerimise](key-vault-key-rotation-log-monitoring.md) - see jalutab läbi kuidas seadistada võtme pööramine ja Azure võti Vault auditeerimisel.

Rohkem ülesandele kohast integreerimine ja kasutades Azure võti võlvid, vaadake teemast [Ryan Jones Azure ressursihaldur malli näited võti Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

## <a name="integrated-with-key-vault"></a>Integreeritud võti Vault

Need artiklid on muud stsenaariumid ja teenuseid, mis muudavad meid või integreerida võti Vault.

- [Azure ketta krόpteerimise](../security/azure-security-disk-encryption.md) tasakaalustab tööstuse standard [BitLockeri](https://technet.microsoft.com/library/cc732774.aspx) funktsioon Windows ja Linux OS ja andmeid ketaste krüptimine pakkuda [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) tegur. Lahendus on integreeritud Azure võti Vault aitab kontrollida ja reguleerida ketta krüpteerimise võtmed ja tellimuse võti vault, tagades virtuaalne masin ketta kõik andmed krüpteeritakse puhkeasendis Azure storage saladusi.


## <a name="supporting-libraries"></a>Toetab raamatukogude

- [Microsoft Azure võti Vault Core](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) teek `IKey` ja `IKeyResolver` liidesed vajalike tunnuste võtmed toimingute abil.

- [Microsoft Azure võti Vault Extensions](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) pakub laiendatud võimalused Azure võti Vault.

## <a name="other-key-vault-resources"></a>Muud võti Vault võimalused
- [Peamised Vault blogi](http://aka.ms/kvblog)
- [Peamised Vault Foorum](http://aka.ms/kvforum)
