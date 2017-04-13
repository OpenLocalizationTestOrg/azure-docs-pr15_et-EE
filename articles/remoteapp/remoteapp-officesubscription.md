
<properties 
    pageTitle="Kuidas kasutada Office 365 tellimuse Azure RemoteApp | Microsoft Azure'i"
    description="Siit saate teada, kuidas saate oma Office 365 tellimuse Azure RemoteApp jagada Office'i rakendused."
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Kuidas kasutada Office 365 tellimuse Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Kas teadsite, et saate olemasoleva Office 365 tellimuse Azure RemoteApp ühiskasutusse anda pilveteenuses Office'i rakendusi? Lugeda Lisateavet oma Office 365 + Azure RemoteApp suvandid, sh lingid artiklite juurde Office 365 kohta, mis aitavad teil kõige paremini teie tellimus.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Kuidas kasutada Office 365 kontode Azure RemoteApp
Tutvuge Peetri uue artiklist leiate teavet: [Kuidas kasutada Azure RemoteApp teenusekomplektiga Office 365 Kasutajakontod](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Office'i rakenduste käivitamiseks Azure RemoteApp saab kasutada Office 365 tellimust?

Jah! Office 365 tellimuse abil tegelikult on ainus võimalus tuua Office'i rakenduste Azure RemoteApp.

(Märkus: kui teie Azure RemoteApp juurutus saadetaks majutusteenuse partneri, nad on võimalik, et teile Office'i litsentsid, mis põhineb [Teenuse pakkuja litsentsileping](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))


Office 365 tellimuse suur asi on, et see võimaldab kasutada sama kasutajalitsentsi paljude erinevate platvormide ja keskkonnas, sh Azure pilve. Kui kasutate Office'i rakenduste Azure RemoteApp peate ei osta täiendavaid litsentse või konfigureerimine olemasolevate litsentside kuidagi teisiti. Teil on vaja Office 365 tellimuse, mis sisaldab [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx).

Office 365 ProPlus lubab [ühiskasutatavas arvutis aktiveerimine](https://technet.microsoft.com/library/Dn782860.aspx) – see funktsioon võimaldab ajutist kasutajapõhine aktiveerimise Office'i virtuaalse ja cloud keskkonnas, nt Azure RemoteApp (ja Kaugtöölauateenuseid).

Millist Office 365 lepingud hõlmavad Office 365 ProPlusi? Tutvuge [teenuse kättesaadavus iga lepingu raames](https://technet.microsoft.com/library/office-365-plan-options.aspx) tabel. Pange tähele, et kõik lepingud hõlmavad Office 365 ProPlusi (nt Office 365 Businessi leping). Kui teie lepingut ei ole, kaaluge leping, millele ei (nt Office 365 Education E3).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>OK, kuidas on minu Office 365 ProPlusi litsentside Azure RemoteApp kasutada?

Iga kasutaja jaoks Office 365 ProPlusi litsentsiga aktiveerida Office'i rakenduste kuni 5 arvutite ja tahvelarvutite ja telefonide Üksikkasutaja. Iga aktiveerimise on registreeritud kasutaja kuni seadme Office'i desaktiveerida. (Kasutajad saavad hallata oma [Office 365 portaalis](https://portal.office365.com/)seadmed).

Azure'i RemoteAppi üksikkasutaja võib sisse logida mitmes arvutis samal päeval ilma märkamata. On põhjus teenuse automaatselt haldab ja skaala ressursid pilves, kui kasutaja näeb ainult need rakendused ja programmide ühiskasutusse. See stsenaarium, mis on Office 365 ProPlus pakub ühiskasutatavale arvutile aktiveerimise režiimi-see tähendab, et kasutaja ei pea tegema mis tahes litsentsihalduse juurde pääsemiseks, nende ressursside ja mis üksikuid arvutites arvestata 5 arvuti aktiveerimise limiit.

Kui te (haldus) Office 365 ProPlusi litsentside määramine kasutajatele, nad saavad kasutada Office'i oma isikliku seadmeid, samuti kaudu oma Azure RemoteApp saidikogumi.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Millised Office'i rakendused saab kasutada teenusekomplektiga Office 365 ja Azure RemoteApp?

Office 365 tellimuse abil saate aktiveerida ja Azure RemoteApp juurutuste Office 2013 ühiskasutus. Me praegu ei toeta Azure RemoteApp Office'i ja muude versioonide kasutamine. See hõlmab Office 2003, Office 2007, Office 2010 ja Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Kuidas on lood Visio Pro või Project Pro?

Office 365 ProPlusi pildi RemoteApp teie tellimus sisaldab sisaldab Visio Pro ja Project Pro. Kuid ei saa kasutada Office 365 ProPlusi tellimuse aktiveerimiseks nende programmide – nad on oma litsentsi. [Office 365 portaalis](https://portal.office365.com/)saate need aktiveerida. 

Teil pole litsentsi neid programme, kui te ei soovi neid kasutada. Programmid, mida soovite kasutada - ja vahele jätta teised ainult aktiveerimine Näete endiselt neid pildil, kuid te ei saa neid kasutada. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Kuidas alustada Office 365 ja Azure RemoteApp?

Nüüd, kui teate, et Office 365 litsentsid üksikasju, vaatame kasutamise alustamiseks kasutamine Azure RemoteApp - on väga lihtne:

**Office 365 ProPlusi (nõuab tellimuse)** pilti kasutada oma Azure RemoteApp saidikogumi loomisel.

![Office 365 Pro Plusi Azure RemoteApp pilt](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


Sellel pildil sisaldab Windows Server ja Office 365 ProPlusi uusim versioon. Kui olete oma saidikogumi (sh avaldamise rakendused) kasutajate lihtsalt logige Azure RemoteApp (kasutades nende RemoteApp klient) ja esitada volituste Office 365 jaoks mis tahes Office'i rakenduste. Litsentside edastatakse automaatselt ilma ühtki või halduse nõutav.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Saate luua kohandatud pildi teenusekomplektiga Office 365 ProPlusi?

Saate luua kohandatud pildi oma saidikogumi, mis sisaldab Office 365 ProPlus. On 2 valikuid – kasutage pakume Azure Galerii või saate luua oma kohandatud pilt ja installige Office 365 ProPlus.

### <a name="use-the-azure-gallery-image"></a>Azure'i Galerii kasutamine

Lihtsaim viis juurutada teenusekomplekti Office 365 ProPlus kogumi on [alustada ühe Azure galerii pildid](remoteapp-image-on-azurevm.md) teie Azure RemoteApp tellimus sisaldab. Veenduge, et valite **Windows Server kaugtöölaua seansi hosti teenusekomplektiga Office 365 ProPlusi eelinstallitud** pilt. Seejärel klõpsake soovitud rakenduste installimine selle pildi ja Oletegi valmis.

### <a name="use-a-custom-image"></a>Kohandatud pildi kasutamine

Saate igal ajal luua kohandatud pilt – saate luua [Azure VM](remoteapp-image-on-azurevm.md) või [luua kohalikult pilt](remoteapp-create-custom-image.md) ja laadige see Azure'i. Mõlemal juhul veenduge, et installite Office 365 ProPlus sõlme ühiskasutatavale arvutile aktiveerimise abil. [Office'i juurutamise tööriista](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) kasutamine ja järgige [juhiseid](https://technet.microsoft.com/library/Dn782858.aspx) installi.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Automaatvärskenduste keelamine Office 365 ProPlus teie kohandatud pilt - oluline

Kohandatud pildi kasutab Azure RemoteApp mallina lisamise lisaressursid nimega järele teie kasutajad suureneb. Viivitused ja ühenduse probleemide vältimiseks automaatvärskenduste keelamine Office'i pilt. Kui teil ei ole, siis iga selle malli abil loodud ressursi automaatselt värskendada, kui see on käivitatud. Selle asemel kasutada standard Azure RemoteApp protsessi värskendamine kohandatud pildi. Nii saate värskendada Office'i rakenduste üks kord malli pilti ja seejärel andke Azure RemoteApp Olge ettevaatlik, saada värskendusi kasutajatele.

Lisage automaatvärskenduste keelamine Office'i juurutamise tööriista konfiguratsioonifail järgmine:

        <Updates Enabled="FALSE" />

Nüüd konfiguratsiooni fail peaks sisaldama järgmisi ridu:
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Aga kuidas värskendada pilt koos Office 365 ProPlus?

On palju põhjusi värskendada oma saidikogumi pildi. Siin on paar.

- Saada uusimad Windowsi värskendused 
- Rakenduse Office 365 ProPlusi uusimate värskenduste hankimine
- Kohandatud rakenduse värskendamine
- Muud pilt ise konfiguratsiooni sätete muutmine

Lõpuni juhised kasutada pilt, saate värskendada oma saidikogumi värskendamine, minge [siia](remoteapp-update.md). Kuid pildi ja Office 365 ProPlusi värskendamise kohta lisateabe saamiseks vaadake järgmist teavet.

Saate värskendada oma pildi kahel viisil – asendamine oma pildi täiesti uue või käsitsi värskendada oma olemasoleva pildi.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>Oma pildi asendamiseks uusima Azure Galerii + lisa kohandused
Selle suvandi abil saate lasta Microsoft Windows Server ja Office 365 ProPlusi värskenduste eest. Selle asemel, et värskendada oma olemasoleva pildi, saate luua täiesti uus pilt, mis põhineb uusima Galerii. Korrake kõik toimingud, nagu enne Kohandage pilti-kohandatud rakendusi, siis installige muuta pildi konfiguratsiooni jne.

Galerii Pildid värskendatakse regulaarselt, seega saate hoiate lihtne, teab, et oma Windows Server ja Office 365 ProPlusi rakendused on ajakohased. Muidugi on kompromiss, et teil tuleb teil rakendada kohandused iga kord, kui saate uue pildi. Saate luua skriptide automatiseerimiseks säte kohandused.

### <a name="manually-update-your-existing-image"></a>Käsitsi värskendada oma olemasoleva pildi

Selle suvandi abil saate värskenduste rakendamine pildi standard Windows tööriistad. Office 365 ProPlus alla laadima ja installima uusimaid värskendusi või versiooni Office 365 ProPlusi Office'i juurutamise tööriista kasutamine.

> [AZURE.IMPORTANT] Pidage meeles – Office 365 ProPlusi automaatvärskenduste keelamine.

Kas vajate Office'i juurutamise tööriista värskendusi kasutamise kohta leiate lisateavet?

- [Office 365 toodete klõpskäivituse abil juurutamine Office'i juurutamise tööriista abil](https://technet.microsoft.com/library/JJ219423.aspx)
- [Juurutamine ja värskendamine Office 365 ProPlusi Office'i juurutamise tööriista abil](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (video)
- [Office 365 ProPlusi update sätete konfigureerimine](https://technet.microsoft.com/library/dn761708.aspx)
