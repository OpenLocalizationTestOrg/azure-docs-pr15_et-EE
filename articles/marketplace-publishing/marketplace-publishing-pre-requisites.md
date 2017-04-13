<properties
   pageTitle="Mittetehnilistest eeltingimuste loomise pakkumise Azure'i turuplatsil | Microsoft Azure'i"
   description="Mõista loomine ja juurutamine pakkumise Azure'i turuplatsi teistele ostmiseks."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="08/18/2016"
  ms.author="hascipio"/>

# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Üldine eeltingimuste loomise pakkumise Azure turuplatsiga
Üld-, äri-protsess kesksele eeltingimused, mis on vajalikud edasiliikumiseks pakkumine loomisprotsessi mõista.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Veenduge, et te Microsofti müüja
Üksikasjalikud juhised Microsofti müüja konto registreerimine, minge [konto loomine ja registreerimise](marketplace-publishing-accounts-creation-registration.md).

- **Kui teie ettevõttel on juba registreeritud müüja arendaja keskuses ja soovite luua uue pakkumise,** seejärel avalda sisselogimiseks portaali registreerimist teha ja milline Arenduskeskus sama e-posti id-ga. See on vajalik, et Arenduskeskus ja avaldamise portaal on omavahel lingitud.
- **Kui teie ettevõttel on juba registreeritud müüja arendaja keskuses ja soovite redigeerida olemasolevaid pakkumise,** siis kas login avalda portaali administraatori konto või mõne muu kontoga, mis on lisatud co-admin avalda portaal. Allpool on esitatud co-administraatori konto lisamiseks järgmist.

## <a name="steps-to-add-a-co-admin-in-the-publishing-portal"></a>Juhised avaldamise portaalis co-administraatori lisamine
Administraatorid, avalda portaalis saate lisada teiste liikmete ettevõte, kes töötavad rakenduse co admin klõpsake jaotises avaldamine portaalis. **Eeldades, et olete administraator,** allpool esitatud on co-administraatori lisamise juhised

>[AZURE.NOTE] Uute kasutajate jaoks, enne lisate co-admin avalda portaali, veenduge, et olete loonud vähemalt ühe taotluse avalda portaalis. See on nõutav vahekaardil **tootjad** kuvatakse alles pärast loomine vähemalt ühe taotluse avalda portaal.

1. Veenduge, et co-admin e-post on Microsofti account(MSA). Kui ei, kasutajaks mõne MSA, kasutades seda [linki](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Veenduge, et on vähemalt üks rakendus admin konto all enne kui proovite lisada co-administraator.
3. Pärast eespool kirjeldatud toimingud on valmis, avalda sisselogimiseks portaali co-admin e-posti id ja seejärel Logi välja.
4. Nüüd sisselogimiseks avalda videoportaali administraator e-posti id-ga.
5. Liikuge tootjad -> valige oma konto -> administraatorid -> Lisa co administraator (allpool toodud pilt)

    ![Joonis](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)

6. Veenduge, et Microsofti mis tahes edastamiseks jälgitav e-posti ID eri avaldamise protsess (nt Arenduskeskus, avaldamise portaali) juures.
7. Arenduskeskus registreerimine, ärge ühe isikuga seotud konto abil. See on soovitatud eemaldamine ühelt sõltuvus.
8. Kui teil näo Arenduskeskus registreerimise probleeme, siis lugege tõsta Piletite, mis on selle [lingi](https://developer.microsoft.com/en-us/windows/support)kaudu.

## <a name="steps-to-delete-a-co-admin-in-the-publishing-portal"></a>Kustutamine co-admin avalda portaal
**Eeldades, et olete administraator,** allpool on toodud juhised kustutamine co-administraator.

1. Sisselogimiseks avalda videoportaali administraator e-posti id-ga.
2. Liikuge **tootjad** -> valige oma konto -> **Administraatorid** -> **Co-administraatorid**.
3. Klõpsake nuppu **X** järgmine co halduskeskuse soovite tot kustutamine (pilt, mis on esitatud allpool).

    ![Joonis](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [AZURE.IMPORTANT] Teil pole täielik ettevõtte maksuvabastuse ja panga teave, kui kavatsete avaldada ainult tasuta pakub (või tuua oma litsentsi).

> Ettevõtte peab olema täidetud alustada. Siiski ajal ettevõtte töötab maksuvabastuse ja panga teave Microsoft Developer konto, arendajad saate alustada tööd [Avaldamisportaal](https://publish.windowsazure.com), kuidas see sertifitseeritud, virtuaalse masina pildi loomine ja selle testimine Azure lavastus keskkonnas. Ainult viimasel etapil teie pakkumise avaldamine Azure'i turuplatsi jaoks on vaja täieliku müüja konto kinnitamine.

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>Azure'i "pay-as-you-go" tellimuse hankimine
See on tellimus, mida kasutate luua teie VM pilte ja [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/)pildid üle. Kui teil pole olemasolevale tellimusele, seejärel Registreeruge veebisaidil https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>Riigid "Müük:"
> [AZURE.WARNING]
Azure'i turuplatsilt teenuste müümiseks peab veenduge, et teie registreeritud üksus on ühest riikidest kinnitatud "müüa-:". See piirang on väljamakse ja maksude põhjustel. Otsime aktiivselt laiendamine lähitulevikus seda loendit riigid, nii et Hoia. Täieliku loendi leiate teemast jaotise 1b [Azure'i turuplatsi osalemist poliitika](http://go.microsoft.com/fwlink/?LinkID=526833).

## <a name="next-steps"></a>Järgmised sammud
Kui mittetehnilistest eeltingimused on täidetud, edasi on pakkumine teatud tehnilise eeltingimused. Klõpsake linki vastav pakkumine tüüp, mida soovite luua Azure'i turuplatsil artikkel.

- [VM tehnilise eeltingimused](marketplace-publishing-vm-image-creation-prerequisites.md)
- [Lahendus malli tehnilise eeltingimused](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Vt ka
- [Alustamine: Azure'i turuplatsi avaldamine pakkumise kohta](marketplace-publishing-getting-started.md)
