<properties
    pageTitle="Microsoft autentimisrakenduse KKK"
    description="Korduma kippuvad küsimused ja vastused, mis on seotud rakenduse Microsoft Authentication ja Azure Mitmikautentimise loetletakse."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar, librown"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator-application-faq"></a>Microsoft Authenticator rakenduse FAQ

Rakendusega Microsoft Authenticator asendatakse Azure'i autentimisrakenduse ja on soovitatav rakendus Azure'i Mitmikautentimise kasutamisel. See rakendus on saadaval Windows Phone, Androidi ja iOS-i jaoks.

## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

- **Mis juhtus Azure'i Autentija, mitme teguri Auth ja Microsofti konto rakendusi?**

    Rakendusega Microsoft Authenticator asendab üksteisest need rakendused. Azure'i Autentija versiooniks Microsoft Authenticator. Kui kasutate mitut tegurit Auth või Microsofti konto rakenduste, installige Microsoft Authenticator ja lisada oma konto uuesti. Veenduge, et konto lisamine uut rakendust enne kustutamata lõpuleviimiseks.

- **Ma kasutan Microsoft Authenticator rakenduse juba kontrollimise koode. Kuidas aktiveerida ühe klõpsuga tõuketeatised?**  

    Sisselogimine tõuketeatised teatisega kinnitamise on saadaval ainult Microsofti kontod, mitte nagu Google või Facebook kolmanda osapoole kontod. Töökoha või kooli Microsofti kontot, saate valida oma ettevõtte selle välja lülitada, kuigi.

    Kui kasutate Microsofti kontot isikliku konto jaoks, ja soovite üle minna Tõuketeatiste, peate oma konto uuesti lisada. Registreerige oma kontoga seade uuesti ja Tõuketeatiste häälestamine.  

    Kui teie konto pole lubatud kaheastmelise kontrollimise, lugege teemat [kaheastmelise kontrollimise kohta,](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) et otsustada, kui see on õige.  

- **Kui kas mul on võimalik, et kasutada ühe klõpsuga Tõuketeatiste iPhone'is või iPadis?**  

    See funktsioon on beeta August, kui muutub laiemalt kättesaadavaks Microsofti kontod lõpuni. Kui soovite liituda meie beta programm, saata meilisõnumeid msauthenticator@microsoft.com. Lisage oma eesnimi, perekonnanimi ja Apple'i ID oma sõnum.  

- **Tehke ühe klõpsuga Tõuketeatiste töö-Microsofti kontoga?**  

    Ei, Tõuketeatiste ainult töö Microsofti kontod ja Azure Active Directory kontod. Kui teie töö või kooli kasutab Azure AD kontod, võivad nad selle funktsiooni keelata.  

- **Ma taastatud seadme varukoopia põhjal ja minu konto koodid on puudu või ei tööta. Mis juhtus?**  

    Turvalisuse huvides me ei kontod varukoopiate põhjal taastamine rakendus. Kui rakendus iOS-i taastamine varukoopia teie kontod on endiselt kuvatakse, kuid need ei tööta sisselogimist kontrolli vastu võtta või luua turvalisus koodid. Pärast rakenduse taastamiseks kontode kustutamine ja uuesti lisada.

- **Sain uuele seadmele. Kuidas vana seade rakendusega Microsoft Authenticator eemaldamine ja teisaldamine uude?**

    Rakendusega Microsoft Authenticator lisamine uuele seadmele automaatselt eemaldada see muudes seadmetes. Hallata, millised seadmed on teie konto jaoks konfigureeritud, veebisaidilt sama abil saate hallata kaheastmelise kontrollimise ja eemaldama vana rakendused.

    Isikliku Microsofti kontot, on see veebisait lehel [konto turvalisus](https://account.microsoft.com/security) . Töökoha või kooli Microsofti kontode puhul, võib see veebisait, kas [minu rakendused](https://myapps.microsoft.com) või kohandatud portaali, mis teie asutuses on seadistatud.

## <a name="contact-us"></a>Võtke meiega ühendust

Kui teie küsimusele ei olnud siin, jätke kommentaari lehe allosas. Või [pöörduge klienditoe poole](https://support.microsoft.com/contactus) ja me vastata teie probleemi niipea, kui võimalik.

Kui tugiteenuste, sisaldab nii palju järgmist teavet, kui saate:

- **Kasutaja ID** -mis on e-posti aadress proovisite sisse logida?
- **Üldise kirjelduse viga** – mis täpne tõrketeade, kas teie näha?  Kui tõrketeadet ei kuvata, on kirjeldatud käitu, märkasite üksikasjad.
- **Lehe** -lehe, mis olid teil kui teile kuvati see tõrge (lisada URL-i)?
- **Tõrkekood** - on saabunud konkreetse tõrkekoodi.
- **SeansiId** - teatud seansi id on saabunud.
- **Korrelatsiooni ID** – mis on loodud, kui kasutaja kuvati see tõrge korrelatsiooni id koodi.
- **Ajatempli** – mis on täpsed kuupäeva ja kellaaja, mis teile kuvati see tõrge (sisaldada soovitud ajavöönd)?

Suur osa selle teabe leiate oma lehele. Kui ei kontrollida, kas teie sisselogimine ajal, valige **Kuva üksikasjad**.

![Tõrke üksikasjade sisselogimine](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Need andmed aitab teie probleemi lahendamiseks nii kiiresti kui võimalik.

## <a name="related-topics"></a>Seotud teemad

- [Azure'i Mitmikautentimise KKK](multi-factor-authentication-faq.md)  
- [Kaheastmelise kontrollimise kohta](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) Microsofti kontode puhul
- [Kaheastmelise kontrollimise on probleeme?](multi-factor-authentication-end-user-troubleshoot.md)
