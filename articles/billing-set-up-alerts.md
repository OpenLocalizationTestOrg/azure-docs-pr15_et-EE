<properties
    pageTitle="Arveldus Microsoft Azure'i tellimuste teatiste häälestamine | Microsoft Azure'i"
    description="Kirjeldab, kuidas saate häälestada teatisi oma Azure arve nii, et saate vältida arvelduse üllatusi."
    services=""
    documentationCenter=""
    authors="vikdesai"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2016"
    ms.author="vikdesai"/>

# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>Microsoft Azure'i tellimuste arvelduse teatiste häälestamine

Kas olete huvitatud palju olete veedavad iga kuu Azure tellimuse? Kui olete konto administraator Azure tellimuse kasutajaks, Azure'i arveldus teatiste teenuse abil saate luua kohandatud arvelduse teatised, mis aitavad teil jälgida ja arvelduse tegevuse Azure kontode haldamine.

See teenus on eelvaade teenust, nii, et teil esmalt on Logi selle jaoks. Külastage selle funktsiooni lubamiseks haldusportaal Azure'i konto [lehel eelvaade funktsioonid](https://account.windowsazure.com/PreviewFeatures) .

## <a name="set-the-alert-threshold-and-email-recipients"></a>Teatiste adressaadid lävi ja e-posti seadmine

Kui olete saanud e-posti kinnituse arveldustoe teenus on sisse lülitatud tellimuse jaoks, külastage konto portaalis [leht tellimused](https://account.windowsazure.com/Subscriptions) . Klõpsake tellimust, mida soovite jälgida, ja seejärel käsku **teatised**.

![][Image1]

Seejärel valige **Lisa teate** luua oma esimene – saate häälestada lisada kuni viis billing teatiste tellimuse erinevate lävi kuni kahe meilisõnumite adressaatide iga teatise kohta.

![][Image2]

Kui lisate teatise, saate talle kordumatu nimi, kulutuste lävi valige ja valige e-posti aadressid, kuhu teatised saadetakse. Piirmäära häälestamisel saate valida mõne **Arveldus kokku** või **Rahaliste krediitkaardiga** loendist **Teatiste jaoks** . Arvete summa, saadetakse teatis, kui tellimuse kulutuste ületab läve. Rahaliste krediidi, saadetakse teatis, kui rahaliste krediiti kukutage alla. Rahaliste krediiti kehtivad tavaliselt operatsioonisüsteem ja MSDN-i kontoga seotud tellimuste.

![][Image3]

Azure'i toetab mis tahes meiliaadressi, kuid ei veenduge, et e-posti aadress töötab, kontrollige üle nii Näpu.

## <a name="check-on-your-alerts"></a>Kontrollige oma teatised

Kui olete häälestanud teatised, konto Center need loendid ja kuvatakse, kui palju veel saate häälestada. Iga teatisega, kuvatakse kuupäev ja kellaaeg saadeti, kas see on teatise arveldus kokku või rahaliste krediidi ja häälestamist limiit. Kuupäeva ja kellaaja vorming on 24-tunnise maailmaaega koordineerimine (UTC) ja kuupäev on yyyy-mm-dd vorming. Teatise loendis selle redigeerimiseks klõpsata plussmärki või nuppu Prügikast, kustutage see.

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png
