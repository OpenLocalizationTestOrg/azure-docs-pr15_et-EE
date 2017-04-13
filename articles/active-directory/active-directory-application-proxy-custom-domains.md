<properties
    pageTitle="Kohandatud domeene Azure AD Rakenduse puhverserveri töötamine | Microsoft Azure'i"
    description="Kuidas töötada tiitellehed Azure AD Rakenduse puhverserveri kohandatud domeene."
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

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Kohandatud domeene Azure AD Rakenduse puhverserveri töötamine

Vaikimisi domeeni kasutamise võimaldab teil määrata sama URL-i sise- ja URL-i juurdepääs rakenduse, et teie kasutajad on ainult üks URL-i rakendus, olenemata sellest, kui neil on juurdepääs kaudu juurdepääsu meeles pidada. See võimaldab teil ühe otsetee paanil Accessi rakenduse loomine. Kui kasutate Azure AD Rakenduse puhverserveri poolt vaikimisi domeeni, on täiendavaid konfiguratsiooni peate lubama oma domeeni. Kui kasutate kohandatud domeeni, on mõned asjad, mida peate tegema, veenduge, et rakenduse puhverserveri tuvastab domeeni ja kinnitab selle serdid.

## <a name="selecting-your-custom-domain"></a>Kohandatud domeeni valimine

1. Avaldada [Avalda rakenduste rakenduse puhverserver](active-directory-application-proxy-publish.md)rakenduse juhiste järgi.
2. Pärast rakendus kuvatakse programmide loendis, valige see ja klõpsake nuppu **Konfigureeri**.
3. Sisestage väljale **Välise URL-i**, kohandatud domeeni.
4. Kui välise URL-i on https, küsitakse teilt, kas soovite üles laadida nii, et selle Azure saate kinnitada rakenduse URL-i sert. Samuti saate üles laadida metamärkide sert, mis vastab välise URL-i taotluse. Oma [Azure'i kinnitatud domeenide](https://msdn.microsoft.com/library/azure/jj151788.aspx)loend peab olema selle domeeni. Azure'i peab olema domeeni URL-i taotluse või metamärkide sert, mis vastab teie taotlus välise URL-i sert.
5. Veenduge, et lisada DNS-kirje, mis marsruudib sisemise URL-i rakendus, mis võimaldab teil olema sama URL-i sise- ja juurdepääsuks ja rakenduse otsetee ühe kasutaja rakenduste loendis.

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>Korduma kippuvad küsimused töötamise kohandatud domeenide kohta

Q: kas saab valida on juba üles laaditud serdi ilma uuesti üles laadida?  
V: varem üleslaaditud serdid on automaatselt seotud rakenduse ja seal on rakenduse hosti nimele vastavad täpselt ühe sert.  

Q: kuidas lisada sert ja vormingut peaks eksporditud serdi üles laadida?  
V: serdi tuleb üles laadida rakenduse konfigureerimise lehe kaudu. Serdi peaks olema PFX-fail.  

Q: kas ECC certs saab kasutada?  
V: ei ole konkreetsete piirang allkirja meetodite.  

Q: kas SAN certs saab kasutada?  
V: Jah.  

Q: kas metamärkide certs saab kasutada?  
V: Jah.  

Q: saab kasutada mõne muu serdi iga taotluse kohta?  
V: Jah, kui kaks rakenduste ühiskasutus väliste sama host.  

Q: kui uue domeeni registreerida, saab kasutada domeeni?  
V: juhitakse rentniku kinnitatud Domeen loendist domeenide loendit.  

Q: mis juhtub on cert lõppemisel?  
V: saate hoiatus rakenduse konfigureerimise lehel jaotises sert. Kui kasutaja proovib juurde pääseda rakenduse, avaneb turbehoiatus.  

K: mida teha, kui on antud rakenduse cert asendamine?  
V: üles laadida uut serti rakenduse konfigureerimise lehe kaudu.  

Q: Kas kustutamine on cert ja asendada?  
V: kui laadite uut serti, kui vana serti ei kasuta mõni muu rakendus, see kustutatakse automaatselt.  

Q: mis juhtub, kui on cert on tühistatud?  
V: tühistamise kontrollid sertide ei tehta. Kui kasutaja proovib juurde pääseda rakenduse, olenevalt brauseris, ei pruugita turbehoiatus.  

Q: saab kasutada iseallkirjastatud serdi?  
V: iseallkirjastatud serdid on lubatud. Pange tähele, et kui kasutate privaatne sertimiskeskus, CDP (serdi tühistamise jaotuse punkti) serti peaks avalik.  

Q: kas koht, kus kõik serdid Vaata minu rentniku jaoks?  
V: see praegune versioon ei toeta.  


## <a name="see-also"></a>Vt ka

- [Rakenduse puhverserveri rakenduste avaldamine](active-directory-application-proxy-publish.md)
- [Ühekordse sisselogimise lubamiseks](active-directory-application-proxy-sso-using-kcd.md)
- [Tingimusvormingu juurdepääsu lubamine](active-directory-application-proxy-conditional-access.md)
- [Azure AD oma kohandatud domeeninime lisamine](active-directory-add-domain.md)

Uudiseid ja uusimate värskenduste, vaadake [rakenduse puhverserveri ajaveeb](http://blogs.technet.com/b/applicationproxyblog/)
