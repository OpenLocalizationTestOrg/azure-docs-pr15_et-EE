<properties
    pageTitle="Töötamine taotluste arvestada minirakenduste rakenduse puhverserver"
    description="Kuidas saada tööle Azure AD Rakenduse puhverserveri hõlmab."
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
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Töötamine taotluste arvestada minirakenduste rakenduse puhverserver

Taotluste arvestada apps teha ümbersuunamise abil turvalisus Turbeloa teenuse (STS), mis omakorda taotleb identimisteabe kasutaja eest märgiks lase enne ümbersuunamist kasutaja rakendusse. Rakenduse puhverserveri töötamiseks nende ümbersuunamised lubamiseks on vaja järgmist võtta.

## <a name="prerequisites"></a>Eeltingimused
Selle toimingu sooritamiseks veenduge, et STS, suunab taotluste arvestada rakendus on saadaval ainult kohapealse võrgu.

## <a name="azure-classic-portal-configuration"></a>Azure'i klassikaline konfiguratsiooni portaal

1. Avaldamine rakenduse [Avalda rakenduste rakenduse puhverserveri](active-directory-application-proxy-publish.md)kirjeldatud juhiste järgi.
2. Rakenduste loendis Valige taotluste arvestada rakendus ja klõpsake nuppu **Konfigureeri**.
3. Kui valisite oma **Preauthentication meetod** **Passthrough** , veenduge, et valida **HTTPS** oma **Välise URL-i** skeem.
4. Kui valisite oma **Preauthentication meetod** **Azure Active Directory** , valige **pole** teie **Sisemine autentimise meetodit**.


## <a name="adfs-configuration"></a>ADFS-i konfigureerimine

1. Avatud ADFS-i haldus.
2. Minge **Tuginedes tootja loodab**, paremklõpsake rakenduse saate avaldada rakenduse puhverserver ja valige **Atribuudid**.  
  ![Tuginedes tootja loodab paremklõpsake rakenduse nime - screentshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. Valige vahekaardi **lõpp-punktid** jaotises **lõpp-punkti tüüp** **Oli-Federation**.
4. Sisestage väljale **URL usaldusväärsed** rakenduse puhverserveri jaotises **Välise URL-i** sisestatud URL ja klõpsake nuppu **OK**.  
  ![Lisage soovitud lõpp-punkti - määramine usaldusväärsed URL-i väärtus - kuvatõmmis](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>Vt ka

- [Rakenduse puhverserveri rakenduste avaldamine](active-directory-application-proxy-publish.md)
- [Ühekordse sisselogimise lubamiseks](active-directory-application-proxy-sso-using-kcd.md)
- [Teil on rakenduse puhverserveri probleemide tõrkeotsing](active-directory-application-proxy-troubleshoot.md)
- [Omakliendi rakendused suhelda puhverserveri rakenduste lubamine](active-directory-application-proxy-native-client.md)

Uudiseid ja uusimate värskenduste, vaadake [rakenduse puhverserveri ajaveeb](http://blogs.technet.com/b/applicationproxyblog/)
