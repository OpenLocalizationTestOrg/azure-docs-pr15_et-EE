<properties
   pageTitle="Häälestamise nimega autentimiseks mandaadi | Microsoft Azure'i"
   description="Siit saate teada, kuidas mandaati selle Visual Studio abil autentida taotluste Azure'i avaldamine rakenduse Visual Studio Azure või olemasoleva pilvepõhise teenuse jälgimiseks. "
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="setting-up-named-authentication-credentials"></a>Kuidas häälestada nimega autentimiseks mandaadi

Avaldamine rakenduse Visual Studio Azure või olemasoleva pilvepõhise teenuse jälgimiseks, peate sisestama Visual Studio abil saate autentida taotluste Azure'i sisselogimisandmed. On mitmeid kohti Visual Studios, kus te saate sisse logida nende mandaati. Näiteks Explorerist serveri saate **Azure** sõlme kiirmenüü avamine ja valige käsk **Loo ühendus Azure**. Kui olete sisse loginud, Azure kontoga seostatud tellimuse teave on kättesaadav Visual Studio ja polegi rohkem, peate tegema.

Azure'i tööriistad toetab mõne vanema viis anda mandaat, kasutades tellimuse faili (.publishsettings fail). Selles teemas kirjeldatakse seda meetodit, mis on endiselt Azure'i SDK 2.2 toetatud.

Järgmiste üksuste puhul on nõutav Azure autentimist.

- Teie tellimuse ID

- X.509 v3 tunnistus

>[AZURE.NOTE] X.509 v3 sertifikaadi võti pikkus peab olema vähemalt 2048 bittide. Azure'i hülgab mis tahes sert, mis ei vasta selle nõude või mis pole lubatud.

Visual Studio kasutab sertifikaadi andmed koos oma tellimuse ID mandaat. Sobiv mandaate viidatud tellimuse faili (.publishsettings fail), mis sisaldab avalik võti serti. Tellimuse fail võib sisaldada rohkem kui ühe tellimuse mandaat.

Tellimuse teave dialoogiboksis **Uus/Muuda tellimust** saate redigeerida, nagu on selgitatud käesoleva teema.

Kui soovite serdi loomine, saate Vaadake juhiseid teemas [luua ja üles laadida Azure serdi haldus](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) ja seejärel käsitsi laadige üles sert [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.NOTE] Visual Studio hallata oma pilveteenustega nõudva identimisteavet pole sama identimisteavet, mida on vaja autentida taotluse vastu Azure storage teenused.

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Muuta või eksportida autentimiseks mandaadi Visual Studio

Saate luua, muuta või eksportimine mandaadi autentimine dialoogiaknas **Uus tellimus** , mis kuvatakse siis, kui saate teha ühte järgmistest toimingutest.:

- **Server Explorer** **Azure** sõlme kiirmenüü avamine, valige **Tellimuste haldamine**, **serdid** menüüd ja valige nupp **Uus** või **redigeerida** .

- **Azure'i teenuserakenduse avaldamine** viisardis teenuse Azure pilveteenuse avaldamisel valige loendis **Valige tellimuse** **haldamine** ja seejärel menüüd serdid ja valige nupp **Uus** või **redigeerida** .

Järgnev toiming eeldab, et dialoogiboksis **Uus tellimus** on avatud.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Häälestada autentimiseks mandaadi Visual Studio

1. **Valige olemasolev tunnistus** autentimise loendi, valige sert.

1. Klõpsake nuppu **Kopeeri täielik tee** . Sert (CER-fail) tee kopeeritakse lõikelauale.

    >[AZURE.IMPORTANT] Azure'i rakenduse Visual Studio avaldamiseks peab üles selle serdi [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885)serdi üleslaadimiseks tehke järgmist.

    1. Valige link Azure'i portaalis.

         [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885) avatakse.

    1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885)sisse logida ja valige nupp **Pilveteenustega** .

    1. Valige pilveteenuses, mis teile huvi pakub.

        Avaneb leht teenuse kohta.

    1. Vahekaardil **serdid** nuppu **üles laadida** .

    1. CER-fail, mille äsja loodud täielik tee kleepida, ja seejärel sisestage teie määratud parool.
