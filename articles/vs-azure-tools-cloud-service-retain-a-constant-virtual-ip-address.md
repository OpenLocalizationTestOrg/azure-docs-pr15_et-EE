<properties
   pageTitle="Kuidas säilitada konstandi virtuaalse IP-aadressi jaoks mõnda pilveteenusesse | Microsoft Azure'i"
   description="Siit saate teada, kuidas tagada, et virtuaalse IP-aadress (VIP) oma Azure pilveteenuses ei muutu."
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

# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>Kuidas säilitada konstandi virtuaalse IP-aadressi jaoks mõnda pilveteenusesse

Pilveteenuses, mis on majutatud Azure'i värskendamisel peate veenduge, et virtuaalse IP-aadress (VIP) teenus ei muutu. Mitme domeeni halduse teenused kasutada domeeninimede registreerimiseks Domain Name System (DNS). DNS-i toimib ainult siis, kui VIP jääb samaks. Azure'i tööriistad **Viisardi avaldamine** saate tagada, et teie pilveteenuses VIP ei muutu värskendamist. DNS-i domeeni haldamise puhul pilveteenuste kasutamise kohta leiate lisateavet teemast [konfigureerimise teenuse Azure pilveteenuses kohandatud domeeni nimi](./cloud-services/cloud-services-custom-domain-name.md).

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Avaldamise pilveteenus oma VIP muutmata

Pilveteenus VIP eraldatakse esmalt juurutamist Azure teatud keskkonnas, nt tootmiskeskkonda. VIP ei muuda, v.a juhul, kui kustutate juurutamise otseselt või kaudselt kustutatakse juurutamise värskenduse protsessi. Säilitamise VIP, siis kustutage ei juurutamise ja te peate kindlasti Visual Studio ei kustutata juurutamise automaatselt. **Viisardi avaldamine**, mis toetab mitut Juurutussuvandid juurutamise sätete määramisega saate määrata käitumist. Saate määrata värske juurutamise või Värskenda juurutus, mis võib olla suureneva või samaaegne, ja mõlemat tüüpi värskendus juurutuste säilitamise VIP. Juurutamise eri tüüpi määratlused, leiate [Azure'i rakendus viisard avaldamine](vs-azure-tools-publish-azure-application-wizard.md).  Lisaks saate määrata, kas eelmise kasutuselevõtu pilveteenus kustutatakse, kui ilmneb tõrge. VIP ootamatult võib muutuda, kui see suvand pole õigesti seadistatud.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Värskendada oma VIP muutmata pilveteenus

1. Kui juurutate oma pilveteenuses vähemalt ühe korra Azure'i projekti sõlm kiirmenüü avamine ja klõpsake nuppu **Avalda**. Kuvatakse **Avaldada Azure taotluse** viisard.

1. Tellimuste loendis valida, millise, millele soovite juurutada ja seejärel klõpsake nuppu **edasi** . Viisardi lehel **sätted** kuvatakse.

1. **Levinud sätete** vahekaardil, veenduge, et pilveteenusesse millele juurutamist, **keskkonnas**, **Koostada konfiguratsiooni**ja **Teenuse konfiguratsiooni** nimi on kõik õige.

1. Kontrollige vahekaardil **Täpsemad sätted** salvestusruumi konto ja juurutamise silt on õige, et ruut **Kustuta juurutamise korral** on tühi, ja et oleks märgitud ruut Värskenda **juurutamine** . **Juurutamise** ruut Värskenda valides saate veenduge, et juurutamise ei kustutata ja teie VIP ei lähe kaotsi kui avaldate rakenduse. Tühjendada **kasutuselevõtt nurjumise ruut Kustuta**, tagate, et teie VIP ei lähe kaotsi kui juurutamise käigus ilmneb tõrge.

1. Täpsustada, kuidas soovite rollid tuleb värskendada, valige link **sätted** **juurutamise värskendada** välja kõrval ja valige astmeline värskendamine või samaaegne suvand **juurutamise värskendada** sätted dialoogiboksi. Kui valite astmeline värskendamine, värskendatakse iga eksemplari üksteise järel, et rakendus oleks alati saadaval. Kui valite samaaegse update, muudetakse kõik eksemplarid samal ajal. Samaaegne värskendamine on kiirem, kuid teenust ei pruugi olla saadaval värskendamise käigus.

1. Kui olete oma sätted, klõpsake nuppu **edasi** .

1. Viisardi lehel **Kokkuvõte** kinnitage oma sätted ja seejärel klõpsake nuppu **Avalda** .

  >[AZURE.WARNING] Juurutamise nurjumisel peaks aadress, miks see nurjus ja Juurutage uuesti kiiresti oma pilveteenus lahkumist rikutud olekus vältimiseks.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet avaldamise Azure Visual Studio, leiate teemast [Azure avaldamine rakendus viisard](vs-azure-tools-publish-azure-application-wizard.md).
