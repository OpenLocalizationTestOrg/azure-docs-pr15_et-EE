<properties
   pageTitle="Konfigureerimine on Azure pilveteenuse teenuse Project Visual Studio abil | Microsoft Azure'i"
   description="Saate teada, kuidas konfigureerida mõnda Azure pilveteenuse teenuse projekti Visual Studios, vastavalt oma vajadustele, et projekt."
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

# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Visual Studio ja Azure pilveteenuse teenuse projekt, mis konfigureerimine

Azure'i pilveteenuste projekti, saate konfigureerida vastavalt oma vajadustele, et projekt. Saate määrata järgmiste kategooriate projekti atribuudid.

- **Azure'i pilveteenus avaldamine**

  Saate seada atribuudi veendumaks, et olemasolev pilveteenuses juurutatud Azure'i eksikombel ei kustutata.

- **Käivitage või silumine pilveteenus kohalikus arvutis**

  Valige teenuse konfigureerimine ja määrake, kas alustada Azure storage emulaator.

- **Kinnitage pilveteenuse pakett loomisel**

  Saate määrata hoiatusi käsitleda tõrgete nii, et saate olla kindel, et pilveteenuse pakett on juurutada ilma küsimusi. See vähendab teie ootama aega, kui juurutada ja siis avastada, et ilmnes tõrge.

Järgmisel joonisel valimine konfiguratsioon kasutamiseks käivitamisel või silumine kohalikult oma pilveteenuses. Projekti atribuute nõudvate selles aknas saate määrata, nagu joonisel näha.

![Microsoft Azure'i projekti konfigureerimine](./media/vs-azure-tools-configuring-an-azure-project/IC713462.png)

## <a name="to-configure-an-azure-cloud-service-project"></a>Konfigureerimine on Azure pilveteenuse teenuse projekt

1. Pilvepõhise teenuse projekti **Solution Exploreris**konfigureerimiseks pilvepõhise teenuse projekti kiirmenüü avamine ja seejärel valige **Atribuudid**.

  Visual Studio redaktoris kuvatakse leht pilvepõhise teenuse projekti nime.

1. Valige vahekaart **areng** .

1. Veenduge, et te ei kustutaks kogemata olemasoleva juurutuse Azure, käsuviibas enne kustutamist juurutamise olemasolevat loendit, valige **tõene**.

1. Valige teenuse konfiguratsiooni, mida soovite kasutada, kui teil tekib või silumine **teenuse** loendist oma pilveteenuses, valige teenuse konfigureerimine.

  >[AZURE.NOTE] Kui soovite luua teenuse konfiguratsiooni kasutamise kohta leiate teemast Kuidas: haldamine teenuse konfiguratsioone ja profiilid. Kui soovite muuta teenuse konfigureerimine rolli, vaadake, [Kuidas konfigureerida Azure pilveteenuse teenuse Visual Studio rollid](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Azure'i salvestusruumi alustamiseks valige emulaator käivitamisel või silumine oma pilveteenuses kohalikult, **Alustage Azure salvestusruumi emulaator** **True**.

1. Veenduge, et ei saa kui **käsitleda hoiatused veana**on paketi valideerimise vigu, avaldada, valige **tõene**.

1. Veenduge, et teie web rolli kasutab sama port iga kord, kui see käivitab kohalikult IIS-i kiire **kasutamine web projekti pordid**, valige **tõene**. Kindla web projekti kindla pordi kasutamiseks web project kiirmenüü avamine, valige vahekaart **Atribuudid** , **Web** menüüd ja pordinumber **Projekti URL-i** säte jaotises **IIS-i kiire** muutmine. Sisestage näiteks `http://localhost:14020` projekti URL.

1. Teenus cloud projekti atribuutide tehtud muudatused salvestada, valige tööriistaribal nuppu **Salvesta** .

## <a name="next-steps"></a>Järgmised sammud

Visual Studio Azure pilveteenuse teenuse projektide konfigureerimise kohta lisateabe saamiseks vt [konfigureerimine oma Azure'i projekti abil mitme teenuse konfiguratsioone](vs-azure-tools-multiple-services-project-configurations.md).
