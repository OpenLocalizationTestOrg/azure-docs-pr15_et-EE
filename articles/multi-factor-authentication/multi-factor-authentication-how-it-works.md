<properties 
    pageTitle="Azure'i Mitmikautentimise – kuidas see toimib"
    description="Azure'i Mitmikautentimise aitab kaitsta juurdepääs andmetele ja rakenduste kasutaja nõudmisel lihtsa sisselogimise protsessi koosoleku ajal. See turvalisemaks, mis nõuab autentimist teine vorm ja pakub mitmesuguseid võimalusi lihtne kinnitamine kaudu tugev autentimine."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

#<a name="how-azure-multi-factor-authentication-works"></a>Azure'i Mitmikautentimise tööpõhimõte

Mitmikautentimise turvalisus seisneb oma kihiti olevad lähenemisviisi. Mitme autentimise tegureid kompromisse esitatakse kohtumisel ründajad. Isegi kui ründaja haldab teavet kasutaja parooli, on kasutuks omamise usaldusväärsete seadme ka ilma. Kasutaja peaks kaotada seade, isik, kes leiab see ei saa te seda kasutada, kui ta ka teab kasutaja parooli.

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)



Azure'i Mitmikautentimise aitab kaitsta juurdepääs andmetele ja rakenduste kasutaja nõudmisel lihtsa sisselogimise protsessi koosoleku ajal.  See turvalisemaks, mis nõuab autentimist teine vorm ja pakub tugeva autentimise mitmesuguseid lihtne kinnitamise suvandid kaudu:

- telefonikõne
- tekstsõnumi
- mobiilirakenduse teatis – võimaldab kasutajatel valida eelistate
- mobiilirakenduse kinnituskood
- 3. tootja VANDE sõned

Lisateabe saamiseks oh kuidas see toimib leiate järgmisest videost.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

##<a name="methods-available-for-multi-factor-authentication"></a>Mitmikautentimise saadaval meetodid
Kui kasutaja sisse logib, saadetakse täiendava kontrolli kasutajale.  Järgmisena kirjeldame viise, mida saab kasutada teise kontrolli loendit.

Kinnitusmeetod  | Kirjeldus
------------- | ------------- |
Telefonikõne | Kõne pannakse kasutaja nutitelefoni, paluge neil kinnitada, et ta on sisse logida, vajutades märgi #.  See on kinnitamise protsessi lõpuleviimiseks.  See suvand on konfigureeritav ja saab muuta teie määratud koodi.
Tekstsõnumi | Sõnum saadetakse kasutaja nutitelefonis 6 numbrilise.  Sisestage see kood kinnitamise protsessi lõpuleviimiseks.
Mobiilirakenduse teatis | Taotluse kinnitamine saadetakse kasutaja nutitelefoni, paludes neil täieliku kinnitamine, valides Kinnita mobiilirakenduse kaudu. See juhtub siis, kui teie peamine kinnitusmeetod valitud rakenduse teatis.  Kui nad saavad seda, kui ta on sisse logida, saate need aruande pettuse.
Kinnituskood rakendusega Mobile | Mobiilirakenduse kaudu, mis töötab kasutaja nutitelefonis saadetakse kinnituskood.  See juhtub siis, kui teie peamine kinnitusmeetod valitud kinnituskood.


##<a name="available-versions-of-azure-multi-factor-authentication"></a>Azure'i Mitmikautentimise saadaval versioonid
Azure'i Mitmikautentimise on saadaval kolme eri versioonides.  Järgmises tabelis kirjeldatakse iga üksikasjalikumalt.

Versioon  | Kirjeldus
------------- | ------------- |
Mitmikautentimise Office 365 jaoks | Selle versiooni Office 365 rakenduste töötab ja on hallata Office 365 portaali kaudu. Nii administraatorid saavad nüüd turvalisemaks oma Office 365 ressursside mitmikautentimise abil.  See versioon on teenusekomplekti Office 365 tellimust.
Mitmikautentimise Azure administraatorite jaoks | Mitmikautentimise võimalused Office 365 jaoks sama Alamhulk on saadaval tasuta Azure kõik administraatorid. Iga haldus konto Azure tellimuse nüüd pääseb täiendavad kaitse, võimaldades selle mitmikautentimise põhifunktsioone. Nii, et administraator, mis soovib Azure portaali loomine VM, veebilehe salvestusruumi haldamine, mobiilsideseadmete teenuseid või Azure'i teenus saate lisada mitmikautentimise oma administraatori konto.
Azure'i Mitmikautentimise | Azure'i Mitmikautentimise pakub võimaluste rikkaim kogum. <br><br>Pakub täiendavaid konfiguratsiooni suvandid kaudu soovitud Azure haldusportaali, täpsemalt aruandlus ja tugiteenuste jaoks mitmesuguseid kohapealse ja pilveteenuse rakenduste. Azure'i Mitmikautentimise saab osta omaette litsentsi ning on kombineeritud Azure Active Directory Premium ja ettevõtte mobiilsus. <br><br>See saab osta ka eraldi tarbimine luues Azure'i mitmekordne autentimisteenuse Azure tellimuse.
##<a name="feature-comparison-of-versions"></a>Funktsioon versioonide võrdlus
Järgmises tabelis leiate Azure'i Mitmikautentimise erinevates versioonides saadaolevate funktsioonide loendi.


Funktsioon  | Mitmikautentimise jaoks Office 365 (sisaldub Office 365 SKU-d)|Mitmikautentimise Azure administraatoritele (Azure'i tellimuses sisalduv) | Azure'i Mitmikautentimise (sisaldub Azure AD Premium ja ettevõtte mobiilsus)
------------- | :-------------: |:-------------: |:-------------: |
Administraatorid saavad kaitse MFA kontod| * | * (Ainult Azure administraatorikontode jaoks saadaval)|*
Kui teine mobiilirakenduse kaudu|* | * | *
Kui teine telefonikõne|* | * | *
SMS-i teine|* | * | *
Rakenduse paroolid klientidele, mis ei toeta MFA|* | * | *
Administraator juhtida autentimismeetodite| *| *| *
PIN-koodi režiim| | | *
Pettuse teatis| | | *
MFA aruanded| | | *
Ühekordse Meediumierandi| | | *
Kohandatud tervitust telefonikõnede| | | *
Helistaja ID telefonikõnede kohandamine| | | *
Sündmuse kinnituse| | | *
Usaldusväärsete IP-d| | | *
MFA peatada meeles peetud seadmed (avaliku eelvaade)| | | *
MFA SDK| | | *
MFA kohapealse rakenduste MFA serveri kasutamine| | | *


##<a name="how-to-get-azure-multi-factor-authentication"></a>Azure'i Mitmikautentimise hankimine

Kui soovite pakutud Azure'i Mitmikautentimise asemel ainult need, mis on Office 365 kasutajatele ja administraatoritele Azure kõiki funktsioone, on mitu võimalust hankimine:

1.  Azure'i Mitmikautentimise litsentse osta ja määrama kasutajatele.
2.  Ostke litsentse on kombineeritud neis, nt Azure Active Directory Premium või ettevõtte mobiilsus Azure'i Mitmikautentimise ja määrama kasutajatele.
3.  Looge Azure mitmekordne autentimine pakkuja Azure tellimuse sees. Kui teil pole veel Azure tellimuse, te saate registreeruda Azure prooviversiooni tellimuse. Proovitellimused tuleb tavaline tellimused prooviversiooni aegumise enne teisendada.

Kui kasutate Azure mitmekordne autentimisteenuse on kaks kasutus mudeleid saadaval on arve Azure tellimuse kaudu:


- **Kasutaja kohta**. Üldiselt suurettevõtetele mis soovite lubada mitmikautentimise jaoks kindla arvu töötajad, kes vajavad regulaarselt autentimist.
- **Autentimise kohta**. Üldiselt suurettevõtetele mis soovite lubada mitmikautentimise suurele rühmale, välised kasutajad, kellel on vaja harva autentimine.

Hinnad üksikasjad leiate [Azure MFA hinnad.](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

Valige asukoht kohta või tarbimine-põhiste mudelit, mis töötab teie asutuse jaoks kõige paremini.   Klõpsake, et leida alustamine vt [Töötamise alustamine](multi-factor-authentication-get-started.md)
