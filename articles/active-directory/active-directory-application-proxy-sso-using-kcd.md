<properties
    pageTitle="Ühekordse sisselogimise ja rakenduse puhverserveri | Microsoft Azure'i"
    description="Ühekordse sisselogimise Azure AD Rakenduse puhverserveri abil esitada antakse ülevaade."
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
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>Ühekordse sisselogimise ja rakenduse puhverserver

Ühekordse sisselogimise on oluline osa Azure AD rakenduse puhverserver. Parima kasutuskogemuse pakub järgmist:

1. Kasutaja sisse logib pilveteenusesse  
2. Kõigi turvalisuse valideerimine tehakse pilves (eelautentimist)  
3. Kui kutse saatmist kohapeal rakendusse rakenduse puhverserveri konnektor impersonates kasutaja. Rakenduse taustväärtus arvab, et see on pärit domeeni ühendatud seadet regulaarne kasutaja.

![Ettevõtte võrgus lõppkasutaja rakenduse puhverserveri, kaudu juurdepääsu diagramm](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Azure AD Rakenduse puhverserveri abil saate anda kogemuse ühekordse sisselogimise (SSO) kasutajate jaoks. Järgmised juhised abil saate avaldada oma rakenduste SSO kasutamist.


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>SSO kohapeal IWA rakenduste kasutamine KCD rakenduse puhverserver
Saate lubada ühekordse sisselogimise rakenduste abil integreeritud Windowsi autentimine (IWA) rakenduse puhverserveri konnektorid õiguse annab Active Directory jäljendada kasutajatel, ning saata ja vastu võtta sõned nende nimel.


### <a name="network-diagram"></a>Võrgustikudiagramm

Selle diagrammi selgitatakse voogu, kui kasutaja proovib juurde pääseda rakenduse kohapeal, mis kasutab IWA.

![Vooskeemi Microsoft AAD autentimine](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. Kasutaja sisestab URL-i kohapeal rakenduse avamiseks rakenduse puhverserveri kaudu.
2. Rakenduse puhverserveri suunab taotluse Azure AD autentimisteenuste preauthenticate abil. Selles etapis Azure AD kehtib mis tahes rakendatav autentimis- ja luba poliitika, näiteks multifactor autentimist. Kui kasutaja on kinnitatud, Azure AD loob märgiks ja kasutajale saadab.
3. Kasutaja edastab luba rakenduse puhverserver.
4. Rakenduse puhverserveri kinnitatakse luba ja toob selle kasutaja põhisumma nimi (UPN) ja seejärel saadab kutse, UPN-i ja teenuse põhisumma nimi (SPN) konnektor dually autenditud turvalise kanali kaudu.
5. Konnektor teeb selle kohapeal Kerberose piiratud delegeerimine (KCD) läbirääkimiste AD, teadasaamiseks kasutajal saada Kerberose märku rakendusse.
6. Active Directory saadab Kerberose märku rakenduse konnektor.
7. Konnektor saadab algse taotluse rakenduse serveriga, kasutades seda saadud AD Kerberose märku.
8. Rakendus saadab vastuse konnektor, mis tagastatakse rakenduse puhverserveri teenuse ja kasutajale.

### <a name="prerequisites"></a>Eeltingimused

Enne alustamist koos SSO rakenduse puhverserver, veenduge, et teie keskkond on valmis konfiguratsioone ja järgmised sätted.

- Rakenduste SharePoint veebirakenduste, nagu on seatud integreeritud Windowsi autentimist. Lisateabe saamiseks vaadake [Kerberos-autentimine toe lubamine](https://technet.microsoft.com/library/dd759186.aspx)või SharePointi leiate [kavandamine rakenduses SharePoint 2013 Kerberos-autentimine](https://technet.microsoft.com/library/ee806870.aspx).

- Kõik teie rakendused on teenuse subjektinimede.

- Konnektor töötava serveri ja rakenduse töötava serveri on ühendatud domeeni ja sama domeeni või usaldav domeenid. Domeeniga ühenduse loomise kohta leiate lisateavet teemast [liitumine arvuti domeeni](https://technet.microsoft.com/library/dd807102.aspx).

- Konnektor töötava serveri on juurdepääs kasutajate jaoks soovitud TokenGroupsGlobalAndUniversal lugeda. See on vaikesäte, mis võib põhjuseks kõvendid keskkonna turvalisus. Lisateave selle [KB2009157](https://support.microsoft.com/en-us/kb/2009157)sisse.

### <a name="active-directory-configuration"></a>Active Directory konfigureerimine

Active Directory konfiguratsiooni sõltub sellest, kas teie rakenduse puhverserveri konnektor ja avaldatud server on sama domeeni või mitte.

#### <a name="connector-and-published-server-in-the-same-domain"></a>Konnektor ja avaldatud serveri sama domeeni

1. Active Directory, valige **Tööriistad** > **kasutajate ja arvutites**.
2. Valige konnektor töötava serveri.
3. Paremklõpsake ja valige **Atribuudid** > **delegeerimine**.
4. Valige **Usalda arvuti delegeerimine määratud teenustele** ja all **teenused, kuhu saate selle konto esitamine delegeeritud mandaat**, lisage serveri SPN identiteedi väärtus.
5. See võimaldab rakenduse puhverserveri konnektor jäljendada kasutajatel ad rakenduste loendis määratletud suhtes.

![Konnektor – SVR atribuutide akna kuvatõmmis](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>Konnektor ja avaldatud serveri erinevates domeenides

1. Töötamine KCD domeenides eeltingimuste loendi leiate teemast [Kerberose piiratud delegeerimine domeenides](https://technet.microsoft.com/library/hh831477.aspx).
2. Windows 2012 R2, kasutage funktsiooni `principalsallowedtodelegateto` atribuudi konnektor serveris lubamiseks rakenduse puhverserveri volitatud konnektor server, kus on avaldatud serveri `sharepointserviceaccount` ja Delegeeriv server on `connectormachineaccount`.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`saab rakendamise arvutikonto või teenuse konto, mille rakendamise rakenduse pool töötab.


### <a name="azure-classic-portal-configuration"></a>Azure'i klassikaline konfiguratsiooni portaal

1. Avaldamine rakenduse [Avalda rakenduste rakenduse puhverserveri](active-directory-application-proxy-publish.md)kirjeldatud juhiste järgi. Veenduge, et valida **Azure Active Directory** **Preauthentication meetod**.
2. Pärast rakenduste loendis kuvatakse teie rakendus, valige see ja klõpsake nuppu **Konfigureeri**.
3. Klõpsake jaotises **Atribuudid**seatud **Sisemine autentimise meetodit** **Integreeritud Windowsi autentimist**.  
  ![Täiustatud rakenduse konfigureerimine](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. Sisestage **Sisemine rakenduse SPN** serveri. Selles näites on meie avaldatud rakenduse SPN http/lob.contoso.com.  

>[AZURE.IMPORTANT] Kui teie kohapealse UPN-i ja Azure Active Directory UPN-i pole identsed, peate konfigureerimine [delegeeritud login identiteedi](#delegated-login-identity) selleks eelautentimist töötamiseks.

|  |  |
| --- | --- |
| Sisemise Autentimismeetodi määramine | Kui kasutate Azure AD eelautentimist, saate määrata ka sisemise autentimise meetodit, et kasutajad saaksid kasu ühekordse sisselogimise (SSO) selle rakenduse. <br><br> Kui teie rakendus kasutab IWA ja Kerberose piiratud delegeerimine (KCD) SSO selle rakenduse lubamiseks saate konfigureerida, valige **Integreeritud Windowsi autentimine** (IWA). Rakendustes, mis kasutavad IWA peab olema konfigureeritud, kasutades KCD, muidu rakenduse puhverserver ei saa avaldada need rakendused. <br><br> Kui teie rakendus ei kasuta IWA, valige väärtus **pole** . |
| Sisemine SPN | See on teenuse turvasubjektinimi (SPN) ettevõttesisese rakenduse soovitud kohapeal Azure AD konfigureeritud. Rakenduse puhverserveri konnektor kasutab SPN Kerberos sõned rakenduse abil KCD toomiseks. |


## <a name="sso-for-non-windows-apps"></a>SSO-Windows rakendused
Kerberose delegeerimine voogu rakenduses Azure AD Rakenduse puhverserveri algab Azure AD autendib kasutaja pilveteenuses. Kui kutse saabub kohapealse probleemid Azure AD Rakenduse puhverserveri konnektor suheldes kohaliku Active Directory Kerberos Piletite kasutaja nimel. Selle protsessi viidatakse nimega Kerberose piiratud delegeerimine (KCD). Järgmises etapis saadetakse taotluse selle Kerberos Piletite taustväärtus taotluse. On mitmeid protokollide, et määrata, kuidas saada taotlus. Enamik – Windows serverite oodatud Negotiate/spnego't, mis toetab nüüd Azure AD rakenduse puhverserver.

![Mitte-Windowsi SSO skeem](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>Delegeeritud login identiteet
Delegeeritud login identiteedi aitab teil teha kaks teistsugune sisselogimise stsenaariumid:

- Mitte-Windowsi rakendusi, mis tavaliselt saada kasutaja identiteet kasutajanimi või SAM konto nimi pole e-posti aadressi kujul (username@domain).
- Alternatiivne login konfiguratsioone kui UPN-i Azure AD ja UPN-i oma kohapealse Active Directory on erinevad.

Rakenduse puhverserver, saate valida millist identiteedi Kerberos Piletite abil. See säte on taotluse kohta. Mõned neist suvanditest sobivad süsteemid, mis ei aktsepteeri e-posti aadressi vorming, teised on mõeldud alternatiivne Logi sisse.

![Delegeeritud login identiteedi parameetri kuvatõmmis](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Kui delegeeritud sisselogimise identiteedi kasutatakse, väärtus ei pruugi domeenide või ettevõtte metsade kordumatu. Selle probleemi vältimiseks saate avaldada need rakendused kahe eri konnektor rühmade kasutamine kaks korda. Kuna iga teise kasutaja publik, saate selle konnektorid liituda lisamiseks mõnda muusse domeeni.


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>Töötamine SSO kui kohapealse ja pilveteenuse identiteete pole identsed
Kui teisiti konfigureeritud, rakenduse puhverserveri eeldab, et kasutajad on kohapealse ja pilveteenuse täpselt sama identiteeti. Saate konfigureerida, iga rakenduse, millised identiteedi tuleks kasutada ühekordse sisselogimise täites.  

See funktsioon võimaldab palju ettevõtted, mis on erinevate kohapeal ja pilveteenuse identiteeti on SSO pilvest kohapeal rakenduste nõudmata kasutajate sisestage erinevate kasutajanimed ja paroolid. Sinna kuuluvad asutused, mille:

- On mitu domeeni ettevõttesiseselt (joe@us.contoso.com, joe@eu.contoso.com) ja ühe domeeni pilves (joe@contoso.com).

- On-marsruuditavaid domeeninime ettevõttesiseselt (joe@contoso.usa) ja juriidiline üks pilveteenuses.

- Ärge kasutage domeeninimede ettevõttesiseselt (joe)

- Kasutage muu pseudonüümid kohapeal ja pilveteenuses. Nt joe-johns@contoso.comvs.joej@contoso.com  

See aitab rakendusi, mis aktsepteerida aadressid e-posti aadressi, mis on väga levinud stsenaariumi – Windows taustväärtus serverite kujul.


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>Erinevate pilveteenuste ja kohapeal identiteetide SSO seadmine

1. Azure'i AD-ühenduse sätete konfigureerimine, nii põhi identiteedi meiliaadress (mail). Seda tehakse kohandamine käigus, muutes väljale **Kasutajanimi põhimõtet** sätete sünkroonimine. Teate, et neid sätteid ka määrata, kuidas kasutajad logige sisse teenusekomplekti Office 365, Windows10 seadmed, ja muud rakendused, mida kasutada Azure AD tema identiteedi poe.  
  ![Kasutajate kuvatõmmis – ripploend kasutaja turvasubjektinimi tuvastamine](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Rakenduse konfigureerimise rakenduse sätteid soovite muuta, valige **Logi sisse identiteedi delegeeritud** kasutada:
  - Kasutajanimi põhimõtet:joe@contoso.com  
  - Alternatiivsete põhimõte kasutajanimi:joed@contoso.local  
  - Kasutajanimi kasutajanimi põhimõtet osa: joe  
  - Alternatiivsete põhimõte kasutajanimi kasutajanimi osa: joed  
  - Kohapealse SAM konto nimi: sõltuvalt kohapeal domeeni domeenikontrolleri konfigureerimine

  ![Delegeeritud login identiteedi rippmenüü kuvatõmmis](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>Erinevate identiteetide SSO tõrkeotsing
Kui SSO protsessi on tõrge kuvatakse see konnektor masina sündmuste logi nagu on selgitatud teemas [tõrkeotsing](active-directory-application-proxy-troubleshoot.md).
Kuid mõnel juhul taotluse edukalt saadetakse kirjutamata rakenduse samal ajal, kui see rakendus vastuse erinevate HTTP vastused. Tõrkeotsing juhul peab algama uurides sündmuste arv 24029 rakenduse puhverserveri seansi sündmuste logi arvutis konnektor. Kasutaja ID, mida kasutati delegeerimine kuvatakse "kasutaja" välja sündmuse üksikasju. Seansi Logi sisse lülitada, valige **Kuva analüütiline ja silumine logid** juhul, kui viewer vaatamiseks menüü.


## <a name="see-also"></a>Vt ka

- [Rakenduse puhverserveri rakenduste avaldamine](active-directory-application-proxy-publish.md)
- [Teil on rakenduse puhverserveri probleemide tõrkeotsing](active-directory-application-proxy-troubleshoot.md)
- [Nõuded rakendusi töötamine](active-directory-application-proxy-claims-aware-apps.md)
- [Tingimusvormingu juurdepääsu lubamine](active-directory-application-proxy-conditional-access.md)

Uudiseid ja uusimate värskenduste, vaadake [rakenduse puhverserveri ajaveeb](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
