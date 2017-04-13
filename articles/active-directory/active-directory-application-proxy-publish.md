<properties
    pageTitle="Rakenduste Azure AD Rakenduse proxy avaldamine | Microsoft Azure'i"
    description="Kohapealse rakenduste pilve Azure AD Rakenduse puhverserveri avaldada."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-using-azure-ad-application-proxy"></a>Azure'i AD Rakenduse puhverserveri rakenduste avaldamine

Azure'i AD Rakenduse puhverserveri aitab teil toetavad serveri töötajate avaldamise kohapealse rakendused Interneti kaudu. Selle punkti peaks olema juba [lubatud rakenduse puhverserveri Azure klassikaline portaalis](active-directory-application-proxy-enable.md). Selles artiklis tutvustatakse avaldada rakendusi, mis töötavad teie kohalikus võrgus ja turvalist remote juurdepääsu väljaspool võrgu juhiseid. Kui olete selle artikli, peate olema valmis konfigureerida rakendus isikupärastatud teabe või turbenõuetele.

> [AZURE.NOTE] Rakenduse puhverserveri on funktsioon, mis on saadaval ainult juhul, kui läksite Premium või Azure Active Directory Basic edition. Lisateavet leiate teemast [Azure Active Directory väljaanded](active-directory-editions.md).

## <a name="publish-an-app-using-the-wizard"></a>Avaldamine rakenduse viisardi abil

1. Logige sisse administraatorina [Azure klassikaline portaalis](https://manage.windowsazure.com/).
2. Avage Active Directory ja valige kataloog, kui olete lubanud rakenduse puhverserver.

    ![Active Directory - ikoon](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Klõpsake vahekaarti **rakendused** ja klõpsake Kuva allservas nuppu **Lisa**

    ![Rakenduse lisamine](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Valige **Avalda väljaspool võrgu kaudu juurdepääsetavad see rakendus**.

    ![Rakendus, mis on väljaspool võrgu kaudu juurdepääsetavad avaldamine](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Rakenduse kohta järgmist teavet:

    - **Nimi**: rakenduse jaoks kasutajasõbralik nimi. See peab olema kordumatu kataloogi sees.
    - **Sisemise URL**: aadress juurde pääseda rakenduse sees oma privaatvõrgu kasutava rakenduse puhverserveri konnektor. Saate sisestada kindla tee avaldada, ülejäänud serveri on avaldamata serveris taustväärtus. Sel viisil saate avaldada sama serveri eri saidid ja anna igaüht oma nime ja juurdepääsu reegleid.

        > [AZURE.TIP] Kui avaldate selle tee, veenduge, et see sisaldaks kõigi vajalikud pilte, skripte ja laadilehti rakenduse. Näiteks kui teie rakendus on https://yourapp/app ja kasutab pilte, mis asub aadressil https://yourapp/media, siis te peaks avaldada https://yourapp/ tee.

    - **Preauthentication meetod**: Kuidas rakenduse puhverserveri kinnitab enne nende Accessi rakenduse kasutajad. Valige üks suvanditest rippmenüüst menüü.

        - Azure Active Directory: Rakenduse puhverserveri suunab kasutaja Azure AD, mis autendib nende õiguste kataloogi ja rakenduse sisse logima.
        - PassThrough: Kasutajad ei pea autentida rakenduse avamiseks.

    ![Teenuserakenduse atribuutide](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Viisardi lõpetamiseks klõpsake ekraani allservas märkenuppu. Rakendus on nüüd määratletud Azure AD.


## <a name="assign-users-and-groups-to-the-application"></a>Rakenduse kasutajate ja rühmade määramine

Selleks, et kasutajad saaksid teie avaldatud taotlus, peate määrama eraldi või rühmad. (Eemaldage kindlasti ise määrata juurdepääsu liiga). Selleks, et iga kasutaja on litsents Azure'i tavaline või uuem. Saate määrata litsentse ükshaaval või rühmadele. Üksikasjalikumat teavet teemast [rakenduse määramine kasutajatele](active-directory-applications-guiding-developers-assigning-users.md) . 

Rakendusi, mis nõuavad eelautentimist, annab see õiguste rakendust kasutada. Rakendused, mis ei nõua eelautentimist kasutajad saate endiselt määrata rakenduse nii, et see kuvatakse nende rakenduste loend, nt minu rakendused.

1. Pärast viisardi lisa rakendus, kuvatakse lehe Kiirkäivituse rakenduse. Kellel on juurdepääs rakenduse haldamiseks valige **kasutajad ja rühmad**.

    ![Rakenduse puhverserveri Lühijuhend määrata kasutajatele - kuvatõmmis](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Otsige kataloogis rühmadele või kuvada kõikide kasutajate. Otsingutulemite kuvamiseks klõpsake märkeruutu.

    ![Rühmadele või kasutajatele - kuvatõmmis otsimine](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Märkige iga kasutaja või rühm, mida soovite määrata see rakendus ja klõpsake käsku **Määra**. Teil palutakse kinnitada, et seda toimingut.

> [AZURE.NOTE] Integreeritud Windowsi autentimist rakendused, saate määrata ainult kasutajad ja rühmad, mis on sünkroonitud kohapealse Active Directoryst. Logige sisse Microsofti kontoga ja külalistele kasutajatele ei saa määrata Azure Active Directory rakenduse puhverserveri avaldatud rakendused. Veenduge, et kasutajate identimisteavet, mis on osa sama Domeen avaldamisel rakenduse sisse logida.

## <a name="test-your-published-application"></a>Avaldatud rakenduse testimine

Kui teie taotlus on avaldatud, saate selle välja testida navigeerides URL, mis on avaldatud. Veenduge, et pääsete sellele, et see muudab õigesti ja, et kõik toimib ootuspäraselt. Kui teil on probleeme või kuvatakse tõrketeade, proovige [tõrkeotsingujuhendi](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Rakenduse konfigureerimine

Saate muuta avaldatud rakendused või häälestamine konfigureerimine lehel Täpsemad suvandid. Sellel lehel saate kohandada rakenduse nime muuta või üleslaadimise logo. Saate hallata juurdepääsureeglid, nt eelautentimist meetodit või mitmikautentimise.

![Täiustatud konfigureerimine](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Pärast avaldamist rakenduste abil Azure Active Directory rakenduse puhverserveri need kuvatakse rakenduste loendis Azure AD ja saate hallata neid seal.

Kui keelate rakenduse puhverserveri teenuseid pärast on avaldatud rakenduste, nad ei ole enam väljaspool teie privaatvõrgu kaudu juurdepääsetavad. See ei kustuta rakendusi.

Saate vaadata rakenduse ja veenduge, et see on kättesaadav, topeltklõpsake rakenduse nimi. Kui rakenduse puhverserveri teenus on keelatud ja rakendus pole saadaval, kuvatakse hoiatusteade ekraani ülaosas.

Rakenduse kustutamiseks valige loendist rakendus ja seejärel klõpsake käsku **Kustuta**.

## <a name="next-steps"></a>Järgmised sammud

- [Avaldada oma domeeninime kasutavad rakendused](active-directory-application-proxy-custom-domains.md)
- [Ühekordse sisselogimise lubamiseks](active-directory-application-proxy-sso-using-kcd.md)
- [Tingimusvormingu juurdepääsu lubamine](active-directory-application-proxy-conditional-access.md)
- [Nõuded rakendusi töötamine](active-directory-application-proxy-claims-aware-apps.md)

Uudiseid ja uusimate värskenduste, vaadake [rakenduse puhverserveri ajaveeb](http://blogs.technet.com/b/applicationproxyblog/)
