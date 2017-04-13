<properties
   pageTitle="Rollid, mis ei hakata tõrkeotsing | Microsoft Azure'i"
   description="Siin on mõned levinud põhjused, miks pilveteenuses rolli ei pruugi alustada. On olemas ka neile probleemidele lahendusi."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Tõrkeotsing käivitu pilveteenuses rollid

Siin on mõned levinud probleemide ja lahenduste seotud Azure'i pilveteenustega käivitu rollid.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Puuduvad dll-teekidega või sõltuvused

Ei reageeri rollid ja rollid, mis on Rattasõit **Initializing**, **hõivatud**ja **peatamine** olekus võib olla põhjustatud puuduvad dll-teekidega või komplektid.

Sümptomid puuduvad dll-teekidega või assemblereid võib olla:

- Teie roll eksemplari on vaheldumisi **Initializing**, **hõivatud**ja **peatamine** olekus.
- Teie roll eksemplari on liikunud **valmis** , kuid kui liigute oma veebirakenduse, lehel ei kuvata.

On mitu soovitatavate uurimiseks järgmiste probleemide korral.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Puuduvate DLL probleemid web rolli diagnoosimine

Kui te Liikuge veebilehele, mis on juurutatud web rolli ja brauseris kuvatakse järgmise sisuga serveritõrge, võib viidata, DLL puudub.

![Serveritõrge rakenduses '/' rakenduses.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Lülitage välja kohandatud tõrked probleemide diagnoosimine

Täielikuma tõrketeabe saab vaadata konfigureerimine web.config web rolli kohandatud tõrge režiimi määramiseks olekusse väljas ja teenuse ümbersuunamine.

Kaugtöölaua kasutamata täielikuma tõrgete vaatamiseks tehke järgmist.

1. Avage Microsoft Visual Studio lahendus.

2. **Solution Exploreris**, otsige fail üles ja avage see.

3. Fail, otsige üles jaotis System.Web mitu ja lisage järgmine rida.

    ```xml
    <customErrors mode="Off" />
    ```

4. Salvestage fail.

5. Pakkima ja Juurutage uuesti teenus.

Kui teenus on asjaomases, kuvatakse tõrketeade puuduvate komplekti või DLL nimi.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Probleemid diagnoosimine vaatamise tõrke kaugühenduse teel

Kaugtöölaua abil saate juurdepääsu roll ja täielikuma tõrketeabe kaugühenduse teel. Järgmiste juhiste abil saate vaadata tõrgete kaugtöölaua:

1. Veenduge, et Azure'i SDK 1.3 või uuem versioon on installitud.

2. Lahenduse Visual Studio abil juurutamisel valida "Konfigureerimine Kaugtöölaua ühendused...". Kaugtöölaua ühendus konfigureerimise kohta leiate lisateavet teemast [Kaugtöölaua abil Azure'i rollid](../vs-azure-tools-remote-desktop-roles.md).

3. Microsoft Azure'i klassikaline portaalis, kui eksemplari näitab olek on **valmis**, klõpsake ühte rolli aknad.

4. Lindi alal **Kaugpöördusteenuse** ikooni **Loo ühendus** .

5. Logige sisse virtuaalse masina kaugtöölaua konfigureerimisel määratud mandaadi abil.

6. Avage käsuviiba aken.

7. Tippige `IPconfig`.

8. Pange tähele IPV4 aadress väärtuse.

9. Avage Internet Explorer.

10. Tippige aadress ja veebirakenduse nimi. Näiteks `http://<IPV4 Address>/default.aspx`.

Veebisaidi navigeerimise nüüd tagastavad selgemaks tõrketeated.

* Serveritõrge rakenduses '/' rakenduses.

* Kirjeldus: Praeguse veebipäringu täitmisel ilmnes töötlemata erandi. Lugege lisateavet selle tõrke kohta ja kui see on pärit koodi virnas Jälita.

* Erandite üksikasjad: System.IO.FIleNotFoundException: ei saa laadida faili või komplekteerimine ' Microsoft.WindowsAzure.StorageClient, versioon = 1.1.0.0 kultuur = neutraalne, PublicKeyToken = 31bf856ad364e35' või mõne selle sõltuvuse. Süsteem ei leia määratud faili.

Näiteks:

![Konkreetsete serveritõrge rakenduses '/' rakenduses](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Probleemid, kasutades Arvuta emulaator diagnoosimine

Microsoft Azure Arvuta emulaator abil saate diagnoosimine ja puuduvad sõltuvused ja web.config tõrgete tõrkeotsing.

Klõpsake selle meetodi diagnoosimise parimate tulemuste saamiseks kasutage arvutis või virtual arvutis, kus on Windows puhta installi. Kõige paremini simuleerida Azure keskkonnas, kasutage Windows Server 2008 R2 x64.

1. Installige [Azure'i SDK](https://azure.microsoft.com/downloads/)autonoomne versioon.

2. Arvutis arengu koostada pilvepõhise teenuse project.

3. Windows Exploreris, liikuge pilvepõhise teenuse project bin\debug kausta.

4. Kopeerige .csx kausta ja .cscfg faili arvuti, mida kasutate silumine probleeme.

5. Puhasta seade, avage leiate Azure'i SDK käsuviiba aken ning tippige `csrun.exe /devstore:start`.

6. Tippige käsuviibale, `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.

7. Roll käivitamisel kuvatakse Internet Exploreris tõrke üksikasjalikku teavet. Samuti saate probleemi täpsemaks diagnoosida standard Windows tõrkeotsingu tööriistu.

## <a name="diagnose-issues-by-using-intellitrace"></a>Probleemid, kasutades IntelliTrace diagnoosimine

Töötaja ja web rollid, mis kasutavad .NET Framework 4, saate kasutada [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), mis on saadaval [Microsoft Visual Studio Ultimate](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs).

Teenuse abil IntelliTrace lubatud kasutuselevõtuks tehke järgmist

1. Veenduge, et Azure'i SDK 1.3 või uuem versioon on installitud.

2. Lahenduse juurutamine Visual Studio abil. Juurutamisel, märkige ruut **Luba IntelliTrace .NET 4 rollid** .

3. Kui näiteks alustab, avage **Server Explorer**.

4. Laiendage soovitud **Azure'i\\pilveteenustega** sõlm ja otsige üles juurutamise.

5. Laiendage juurutamise, kuni näete rolli aknad. Paremklõpsake ühte linnanimede.

6. Valige **vaate IntelliTrace logid**. Avatakse **IntelliTrace Kokkuvõte** .

7. Otsige jaotises erandid kokkuvõtte. Kui erandid, on jaotise nimi **Erandi andmed**.

8. Laiendage **Erand andmete** ja otsida **System.IO.FileNotFoundException** tõrkeid, mis on järgmine:

![Erandi andmeid, puuduvad faili või komplekteerimine](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Puuduvate dll-teekidega ja assemblereid aadress

Kadunud DLL ja komplekti tõrgete lahendamiseks tehke järgmist.

1. Avage lahenduse Visual Studios.

2. Avage **Solution Exploreris** **Viited** kausta.

3. Klõpsake komplekti, mis on määratletud viga.

4. Paanil **Atribuudid** , otsige **kohaliku eksemplari atribuudi** ja määrake väärtuseks **True**.

5. Juurutage uuesti pilveteenusesse.

Kinnitanud, et kõik vead on parandatud, saate juurutada teenus pole märkinud ruutu ruut **Luba IntelliTrace .NET 4 rollid** .

## <a name="next-steps"></a>Järgmised sammud

Saate vaadata rohkem [artikleid tõrkeotsingu](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pilveteenustega.

Pilvepõhise teenuse roll probleemide tõrkeotsingu Azure'i PaaS arvuti diagnostika andmete abil leiate teemast [Kevin Williamson ajaveebi sarja](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
