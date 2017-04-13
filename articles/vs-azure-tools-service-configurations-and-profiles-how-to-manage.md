<properties
   pageTitle="Kuidas hallata teenuse konfiguratsioone ja profiilid | Microsoft Azure'i"
   description="Saate teada, kuidas failidega töötamiseks teenuse konfiguratsioone ja profiilid konfiguratsiooni | mis juurutamise keskkonnas sätted salvestada ja avaldada pilveteenustega sätted."
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

# <a name="how-to-manage-service-configurations-and-profiles"></a>Kuidas hallata teenuse konfiguratsioone ja profiilid

## <a name="overview"></a>Ülevaade

Kui avaldate pilveteenus, Visual Studio talletab konfiguratsiooniteavet kahte tüüpi failid: teenuse konfiguratsioone ja profiilid. Teenuse konfiguratsioone (.cscfg failid) talletamiseks juurutamise keskkondade jaoks teenuse Azure pilveteenuse sätted. Azure'i kasutab neid konfiguratsiooni faile, kui seda haldab teie pilveteenustega. Teisalt, avaldada profiilid (.azurePubxml failid) poe sätete pilveteenustega. Need sätted on kirje, mis valida, kui kasutate avalda viisard ja kasutatakse kohalikult Visual Studio. See teema selgitab, kuidas töötada mõlemat tüüpi failid.

## <a name="service-configurations"></a>Teenuse konfiguratsioone

Saate luua mitme teenuse konfiguratsioone kasutada oma juurutamise keskkonnas. Näiteks võite luua teenuse konfigureerimine kohaliku keskkonnas, mida kasutate käivitamiseks ja testimiseks Azure taotlus ja muu teenuse konfiguratsiooni tootmiskeskkonnast.

Saate lisada, kustutada, ümber nimetada, ja muutke neid teenuse konfiguratsioone oma vajaduste järgi. Saate hallata neid konfiguratsioone teenuse Visual Studio, nagu on näidatud järgmisel joonisel.

![Teenuse konfiguratsioone haldamine](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

**Hallata konfiguratsioone** dialoogiboksi saate avada ka selle rolli atribuudi lehtedelt. Azure'i projekti atribuutide rolli avamiseks selle rolli kohta kiirmenüü avamine ja siis nuppu **Atribuudid**. Vahekaardil **sätted** laiendage **Teenuse konfigureerimine** loendit ja seejärel valige **Halda** **Haldamine konfiguratsioone** dialoogiboksi avamiseks.

### <a name="to-add-a-service-configuration"></a>Lisada teenuse konfigureerimine

1. Solution Exploreris Azure'i projekti kiirmenüü avamine ja seejärel valige **Halda konfiguratsioone**.

    Kuvatakse dialoogiboks **Teenuse konfiguratsioone haldamine** .

1. Teenuse konfiguratsiooni lisamiseks peate looma olemasoleva konfiguratsiooni koopia. Selleks valige konfiguratsioon, mida soovite kopeerida loendist nimi ja seejärel valige **Loo koopia**.

1. (Valikuline) Pange teenuse konfigureerimine mõni muu nimi, valige loendist nimi uue teenuse konfigureerimine ja seejärel valige **Nimeta ümber**. Tippige väljale **nimi** nimi, mida soovite kasutada selle teenuse konfiguratsiooni ja seejärel klõpsake **nuppu OK**.

    Uus teenus konfiguratsioonifail nimega ServiceConfiguration. [Uus nimi] .cscfg lisatakse Azure'i projekti Solution Exploreris.


### <a name="to-delete-a-service-configuration"></a>Kustutamiseks teenuse konfigureerimine

1. Solution Exploreris Azure'i projekti kiirmenüü avamine ja seejärel valige **Halda konfiguratsioone**.

    Kuvatakse dialoogiboks **Teenuse konfiguratsioone haldamine** .

1. Teenuse konfiguratsiooni kustutamiseks valige konfiguratsioon, mida soovite kustutada loendist **nimi** ja seejärel valige **Eemalda**. Kuvatakse dialoogiboks kinnitamaks, et soovite selle konfiguratsiooni kustutamine.

1. Valige **Kustuta**.

     Teenuse konfiguratsioonifail eemaldatakse Azure'i projekti Solution Exploreris.


### <a name="to-rename-a-service-configuration"></a>Ümber nimetada teenuse konfigureerimine

1. Solution Exploreris Azure'i projekti kiirmenüü avamine ja seejärel valige **Halda konfiguratsioone**.

    Kuvatakse dialoogiboks **Teenuse konfiguratsioone haldamine** .

1. Teenuse konfigureerimine ümbernimetamiseks valige loendist **nimi** uue teenuse konfigureerimine ja valige **Nimeta ümber**. Väljale **nimi** tippige nimi, mida soovite selle teenuse konfiguratsiooni puhul kasutada, ja seejärel klõpsake **nuppu OK**.

    Azure'i projekti Solution Exploreris muudetakse teenuse konfiguratsiooni faili nimi.

### <a name="to-change-a-service-configuration"></a>Muuta teenuse konfigureerimine

- Kui soovite muuta teenuse konfiguratsiooni, teatud rolli soovite muuta Azure Projectis kiirmenüü avamine ja seejärel nuppu **Atribuudid**. Vt [kohta: Azure'i pilveteenuses, Visual Studio rollid konfigureerimine](https://msdn.microsoft.com/library/azure/hh369931.aspx) lisateabe saamiseks.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Erinevate kombinatsioonide profiilid abil säte tegemine

Profiili abil saate automaatselt täita **Viisardi avaldamine** erinevate kombinatsioonide sätted mitmel otstarbel. Näiteks võite määrata üks profiil silumine ja teise väljaande koostab. Sel juhul profiili **silumine** oleks **IntelliTrace** lubatud ja **silumine** konfiguratsioon ja profiili **väljaanne** oleks **IntelliTrace** keelatud ja **väljaanne** konfiguratsioon. Võite erinevad profiilid juurutamiseks teenuse erinevate salvestusruumi konto abil.

Viisardi esmakordsel käivitamisel luuakse vaikimisi profiili. Visual Studio salvestab profiili faili, mis on .azurePubXml laiend, mis lisatakse Azure'i projekti kaustas **profiilid** . Kui määrate käsitsi erinevate valikute, kui käivitate viisardi hiljem, värskendatakse automaatselt faili. Enne järgmist käivitamist, tuleks on juba avaldatud oma pilveteenuses vähemalt ühe korra.

### <a name="to-add-a-profile"></a>Profiili lisamine

1. Azure'i projekti kiirmenüü avamine ja seejärel valige **Avalda**.

1. **Target profiili** loendi kõrval nuppu **Save profiil** , nagu järgmisel pildil on näha. See loob teie jaoks profiili.

    ![Uue profiili loomine](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. Profiili on loodud, valige **< Halda >** **Target profiili** loendis.

    Kuvatakse dialoogiboks **Haldamine profiilid** , nagu järgmisel pildil on näha.

    ![Dialoogiboksi profiili haldamine](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. **Nime** loendis Valige profiil ja valige **Kopeeri loomine**.

1. Klõpsake nuppu **Sule** .

    Uus profiil profiili sihtloendis kuvatakse.

1. Valige loendis **Target profiil** profiili äsja loodud. Viisardi avaldamine sätted sisestatakse valitud profiilist Valikud.

1. Valige viisardi avaldamine iga lehe kuvamiseks nuppe **Eelmine** ja **Järgmine** ja seejärel Kohandage seda profiili sätete. Lisateabe saamiseks vt [Avaldada Azure rakendus viisard](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Kui olete lõpetanud, kohandada sätteid, valige **järgmisele** lehele naasmiseks. Profiili on salvestatud teenuse avaldamisel, kasutades neid sätteid, või kui valite **salvestamine** profiilide loendi kõrval.

### <a name="to-rename-or-delete-a-profile"></a>Ümber nimetada või kustutada profiili

1. Azure'i projekti kiirmenüü avamine ja seejärel valige **Avalda**.

1. Valige loendis **Target kasutajaprofiilide** **haldamine**.

1. Dialoogiboksis **Haldamine profiilid** Valige eemaldatav profiil, mille soovite kustutada, ja valige **eemaldada**.

1. Valige kuvatavas dialoogiboksis kinnituse **OK**.

1. Valige **Sule**.

### <a name="to-change-a-profile"></a>Profiili muutmiseks

1. Azure'i projekti kiirmenüü avamine ja seejärel valige **Avalda**.

1. Valige loendis **Target profiil** profiili, mida soovite muuta.

1. Valige viisardi avaldamine iga lehe kuvamiseks nuppe **Eelmine** ja **Järgmine** ja seejärel muutke soovitud sätted. Lisateabe saamiseks vt [Avaldada Azure rakendus viisard](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Kui olete sätted muutnud, valige **järgmisele** **lehele** naasmiseks.

1. (Valikuline) valige **Avalda** avaldada pilveteenuses, kasutades uusi sätteid. Kui te ei soovi avaldada oma pilveteenuses praegu ja avaldada viisardi sulgemiseks, Visual Studio küsib, kas soovite muudatused salvestada profiili.

## <a name="next-steps"></a>Järgmised sammud

Azure'i projekti Visual Studio mujal konfigureerimise kohta leiate teemast [konfigureerimine on Azure projekt](http://go.microsoft.com/fwlink/p/?LinkID=623075)
