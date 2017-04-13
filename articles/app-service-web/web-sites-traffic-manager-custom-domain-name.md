<properties
    pageTitle="Azure'i rakenduse teenus, mis kasutab liikluse haldur koormusetasakaalustuseks konfigureerida kohandatud domeeninime web appi."
    description="Jaoks kohandatud domeeninime kasutada ka web appi Azure'i rakenduse teenus, mis sisaldab liikluse haldur koormusetasakaalustuseks."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Azure'i rakendust Service liikluse halduris kohandatud domeeninime web appi konfigureerimine

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Selles artiklis on toodud üldised juhised Azure'i rakendust Service kohandatud domeeninime kasutamine, liikluse haldur koormusetasakaalustuseks kasutatavad.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>DNS-i kirjete mõistmine

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>Teie web Appsi jaoks standardrežiimis konfigureerimine

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>Kohandatud domeeni DNS-i kirje lisamine

> [AZURE.NOTE] Kui ostsite domeeni kaudu Azure'i rakenduse teenuse veebirakenduste vahele jätta juhiste ja viimasel etapil [Osta domeeni Web Appsi](custom-dns-web-site-buydomains-web-app.md) artiklis viidata.

Kohandatud domeeni seostada veebirakenduse Azure'i rakendust Service, peate lisama uue kirje tabelis DNS-i oma kohandatud domeeni oma domeeninime kaudu ostetud domeeniregistraatori tööriista abil. Leidke ja DNS-i tööriistu saate kasutada järgmiste juhiste abil.

1. Logige sisse oma kontole oma domeeniregistraatori juures ja otsige DNS-i kirjete haldamise leht. Otsige linke või saidi **Domeeninime**, **DNS-i**või **Name Server Management**märgistatud alad. Sageli lingi sellelt lehelt leiate oma konto teabe vaatamine ja seejärel otsin lingi, nt **Minu Domeenid**.

1. Kui olete leidnud haldamise lehel oma domeeni nimi, otsige linki, mis võimaldab teil DNS-kirjete redigeerimiseks. See võib loendis **Zone file**, **DNS-i kirjeid**, või **Täpsemalt** konfiguratsiooni lingina.

    * Lehe tõenäoliselt on juba loonud, nt mõni kirje seostada mõne kirje "**@**"või"\*" 'domeeni parkimine' leht. See võib sisaldada ka kirjeid levinud alamdomeenide näiteks **www**.
    * Lehe mainimine **CNAME-kirjet**, või sisestada ripploendis kirje tüübi valimiseks. See mainida ka muude kirjetega, nt **kirjete** ja **MX-kirjed**. Mõnel juhul nimetatakse muud nimed, nagu on **Pseudonüümi kirje**CNAME-kirje.
    * Lehe on ka väljad, mis võimaldavad teil **kaardi** **hosti nimi** või **domeeninime** mõne muu domeeni nimi.

1. Kuigi iga domeeniregistraatori üksikasjad erineda, üldiselt vastendate *kaudu* oma kohandatud domeeni nime (nt **contoso.com**,) *-* liikluse haldur domeeninimi (**contoso.trafficmanager.net**), mida kasutatakse veebirakenduse jaoks.

    > [AZURE.NOTE] Teine võimalus, kui kirje on juba kasutusel ja vajate sisaldab ennatlikult siduda teie rakendused, saate luua täiendava CNAME-kirje. Näiteks sisaldab ennatlikult sidumiseks oma veebirakenduse **www.contoso.com** , luua CNAME-kirje: **awverify.www** **contoso.trafficmanager.net**. Saate oma veebirakenduse "www.contoso.com" lisada seejärel ilma "www" CNAME-kirje. Lisateabe saamiseks vt [kohandatud domeeni veebirakenduse loomine DNS-i kirjed][CREATEDNS].

1. Kui olete lõpetanud, lisada või muuta DNS-i kirjeid oma domeeniregistraatori, salvestage muudatused.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Luba liikluse haldur

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [Node.js Arenduskeskus](/develop/nodejs/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
