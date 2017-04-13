<properties
    pageTitle="Rakenduse puhverserveri tõrkeotsing | Microsoft Azure'i"
    description="Antakse ülevaade Azure AD Rakenduse puhverserveri tõrgete tõrkeotsingu sooritamiseks."
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
    ms.date="08/19/2016"
    ms.author="kgremban"/>



# <a name="troubleshoot-application-proxy"></a>Rakenduse puhverserveri tõrkeotsing

Kui tõrkeid on juurdepääs avaldatud rakenduses või avaldamise rakendustes, märkige ruut kui Microsoft Azure AD Rakenduse puhverserveri töötab õigesti järgmistest suvanditest:

- Avage Windows Services konsooli ja veenduge, et teenus **Microsoft AAD rakenduse puhverserveri konnektor** on lubatud ja töötab. Võite ka vaadata rakenduse puhverserveri teenus atribuudid, nagu on näidatud järgmisel pildil:  
  ![Microsoft AAD rakenduse puhverserveri konnektor atribuutide akna kuvatõmmis](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)

- Avage Sündmusevaatur ja otsige rakenduse puhverserveri konnektor sündmuste **rakenduste ja teenuste logid** > **Microsoft** > **AadApplicationProxy** > **konnektori** > **administraator**.
- Vajadusel üksikasjalikumad logid on saadaval lülitate analytics silumine logid ja sisselülitamine rakenduse puhverserveri konnektor seansi Logi sisse.

## <a name="the-page-is-not-rendered-correctly"></a>Lehe ei teisendata õigesti

Kui te ei kuule vastava tõrketeate, võib rakendusega või on valesti toimimise veel probleeme. See võib juhtuda siis, kui teie avaldatud artiklis tee, kuid rakenduse jaoks on vaja sisu, mis on olemas ka selle tee.

Näiteks kui avaldate tee https://yourapp/app, kuid rakenduse kutsub piltide https://yourapp/media, nad ei renderdada. Veenduge, et rakenduse abil kõrgeima taseme tee tuleb kaasata kogu asjakohase sisu avaldamist. Selles näites oleks http://yourapp/.

Kui soovite muuta oma tee kaasata viidatud sisu, kuid kasutajate maa tee süvitsi lingi kas vajate endiselt, leiate ajaveebipostitusest [säte õige link rakenduse puhverserveri rakenduste Azure AD juurdepääsu paneel ja Office 365 rakendusekäiviti](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="general-errors"></a>Üldised tõrked

Tõrge | Kirjeldus | Eraldusvõime
--- | --- | ---
See ettevõtte rakendus ei pääse juurde. Teil pole õigust selle rakenduse avamiseks. Autoriseerimine nurjus. Veenduge, et see rakendus kasutaja juurdepääsu määramiseks. | Teil võib-olla olete määranud kasutaja selle rakenduse jaoks. | Avage **rakenduse** menüüd ja klõpsake jaotises **kasutajad ja rühmad**, määrata selle kasutaja või kasutajate rühma see rakendus.
See ettevõtte rakendus ei pääse juurde. Teil pole õigust selle rakenduse avamiseks. Autoriseerimine nurjus. Veenduge, et kasutajal on Azure Active Directory Premium või Basic litsentsi. | Kasutajate võidakse kuvada see tõrge juurde pääseda rakenduse avaldatud, kui nad ei selgesõnaliselt määranud litsentsiga Premium/Basic abonendi administraator. | Minge menüüsse abonendi Active Directory **litsentsid** ja veenduge, et selle kasutaja või rühma kasutajale on määratud Premium või põhi litsentsi.


## <a name="connector-troubleshooting"></a>Konnektori tõrkeotsing
Kui registreerimine nurjus viisardi installimisel, on kaks võimalust vaadata selle põhjus. Vaadake sündmuste logi jaotises **rakenduste ja teenuste Logs\Microsoft\AadApplicationProxy\Connector\Admin**või käivitage järgmine käsk Windows PowerShelli.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| Tõrge | Kirjeldus | Eraldusvõime |
| --- | --- | --- |
| Konnektor registreerimine nurjus: Veenduge, et teil on lubatud rakenduse puhverserveri Azure'i haldusportaal ja kas sisestasite oma Active Directory kasutajanime ja parooli õigesti. Tõrge: "ühe või mitme ilmnenud tõrked." | Sulgesite Azure AD sisse logida ilma registreerimise aken. | Konnektor viisardist ja registreerimist konnektor. |
| Konnektor registreerimine nurjus: Veenduge, et teil on lubatud rakenduse puhverserveri Azure'i haldusportaal ja kas sisestasite oma Active Directory kasutajanime ja parooli õigesti. Tõrge: "AADSTS50001: ressursi `https://proxy.cloudwebappproxy.net /registerapp` on keelatud." | Rakenduse puhverserver on keelatud.  | Veenduge, et lubate rakenduse puhverserveri Azure klassikaline portaalis enne konnektor registreerimiseks. Rakenduse puhverserveri lubamise kohta leiate lisateavet teemast [lubamine rakenduse puhverserveri teenused](active-directory-application-proxy-enable.md). |
| Konnektor registreerimine nurjus: Veenduge, et teil on lubatud rakenduse puhverserveri Azure'i haldusportaal ja kas sisestasite oma Active Directory kasutajanime ja parooli õigesti. Tõrge: "ühe või mitme ilmnenud tõrked." | Kui registreerimise aken avaneb ja sulgub kohe ilma, mis võimaldab teil sisse logida, ilmselt saate selle tõrke. See tõrge ilmneb juhul, kui teie süsteemi võrgu tõrge. | Veenduge, et see on võimalik avaliku veebisaidi brauseri kaudu ühenduse ja, et pordid avatud [rakenduse puhverserveri eeltingimused](active-directory-application-proxy-enable.md)määratletud. |
| Konnektor registreerimine nurjus: Veenduge, et teie arvuti on Internetiga ühendatud. Tõrge: "esines pole lõpp-punkti listening veebisaidil `https://connector.msappproxy.net :9090/register/RegisterConnector` mis võiks sõnumi vastu võtta. Sageli põhjuseks on vale aadress või SOAP toiming. Kui Esita Lisateavet InnerException, vaadake. " | Kui Azure AD kasutajanime ja parooliga sisse logida, kuid siis selle tõrketeate, võib olla, et kõik pordid kohal 8081 on blokeeritud. | Veenduge, et vajalikud pordid avatud. Lisateabe saamiseks vt [rakenduse puhverserveri eeltingimused](active-directory-application-proxy-enable.md). |
| Kustuta on esitatud registreerimise aknas. Ei saa jätkata – akna sulgemiseks. | Olete sisestanud vale kasutajanimi või parool. | Proovi uuesti. |
| Konnektor registreerimine nurjus: Veenduge, et teil on lubatud rakenduse puhverserveri Azure'i haldusportaal ja kas sisestasite oma Active Directory kasutajanime ja parooli õigesti. Tõrge: "AADSTS50059: rentniku tuvastamise teavet leitud kas taotluse või vaikimisi mis tahes esitatud identimisteabe ja otsingu teenuse põhimõttel URI on nurjunud. | Proovite sisse logida kasutades Microsofti Account ja mitte domeenis, mis on osa soovite juurdepääsu kataloogi organisatsiooni ID-d. | Veenduge, et admin on osa rentniku domeeni domeeni nimi, näiteks siis, kui Azure AD domeen on contoso.com, admin tuleks admin@contoso.com. |
| Praeguse täitmise poliitika PowerShelli skriptide toomine nurjus. | Kui konnektori installimine nurjub, kontrollige veenduge, et PowerShelli täitmise poliitika on keelatud. | Avage rühmapoliitika redaktori. Avage **Arvuti konfiguratsioon** > **Haldus mallid** > **Windowsi komponentide** > **Windows PowerShelli** ja topeltklõpsake **sisselülitamine skripti täitmist**. Seda saab seadistada **Pole konfigureeritud** või **lubatud**. Kui määratud **lubatud**, veenduge, et jaotises Suvandid täitmise poliitika on seatud kummagi **Luba kohalike skripte ja remote allkirjastatud skriptide** või **kõik skriptid lubada**. |
| Konnektori konfiguratsiooni allalaadimine nurjus. | Saatja kliendi sert, mida kasutatakse autentimine, aegunud. See võib ilmneda ka siis, kui teil on installitud taha proxy konnektor. Sel juhul konnektor ei pääse Interneti ja ei saa anda rakendusi remote kasutajatele. | Pikendamise usalda käsitsi abil soovitud `Register-AppProxyConnector` Windows PowerShelli cmdlet-käsu. Kui teie konnektor on proxy taga, on vaja anda Interneti-ühendus konnektor kontod "võrguteenuste" ja "Kohalik süsteem." Seda saab teha, andes neile juurdepääsu puhverserverist või neid mööda hiilida puhverserverist seadmisega. |
| Konnektor registreerimine nurjus: Veenduge, et olete oma Active Directory konnektor registreerimiseks globaalne administraator. Tõrge: "registreerimise taotlus on keelatud." | Alias (pseudonüüm), mida proovite sisse logida pole selles domeenis administraator. Teie konnektor on alati installitud kataloogi, kasutaja domeeni omanik. | Veenduge, et admin soovite sisse logida on globaalne Azure AD rentniku õigused.|


## <a name="kerberos-errors"></a>Kerberose tõrked

| Tõrge | Kirjeldus | Eraldusvõime |
| --- | --- | --- |
| Praeguse täitmise poliitika PowerShelli skriptide toomine nurjus. | Kui konnektori installimine nurjub, kontrollige veenduge, et PowerShelli täitmise poliitika on keelatud. | Avage rühmapoliitika redaktori. Avage **Arvuti konfiguratsioon** > **Haldus mallid** > **Windowsi komponentide** > **Windows PowerShelli** ja topeltklõpsake **sisselülitamine skripti täitmist**. Seda saab seadistada **Pole konfigureeritud** või **lubatud**. Kui määratud **lubatud**, veenduge, et jaotises Suvandid täitmise poliitika on seatud kummagi **Luba kohalike skripte ja remote allkirjastatud skriptide** või **kõik skriptid lubada**. |
| 12008 - azure AD ületanud lubatud Kerberose autentimist katsete taustväärtus server maksimaalne arv. | Sel juhul võib viidata vale konfiguratsiooni vahel Azure AD ja kirjutamata application server, või kuupäeva ja kellaaja konfiguratsiooni nii masinad probleem. | Taustväärtus serveri keeldunute Kerberos Piletite, mis on loodud Azure AD. Veenduge, et Azure AD ja taustväärtus rakenduse server on õigesti konfigureeritud. Veenduge, et kuupäeva ja kellaaja konfiguratsiooni Azure AD ja taustväärtus rakenduse server on sünkroonitud. |
| 13016 - azure AD ei saa Kerberos Piletite kasutaja nimel alla laadida, sest puudub serva luba või juurdepääsu küpsise pole UPN-i. | On probleem STS konfiguratsiooni. | UPN-i taotluste konfiguratsiooni STS parandamine. |
| 13019 - azure AD ei saa tuua Kerberos Piletite kasutaja nimel järgmise üldine API tõrke tõttu. | Sel juhul võib viidata vale konfiguratsiooni vahel Azure AD ja domeenikontrolleri serverisse või kuupäeva ja kellaaja konfiguratsiooni nii masinad probleem. | Selle domeenikontrolleri keeldunute Kerberos Piletite, mis on loodud Azure AD. Veenduge, et Azure AD ja taustväärtus rakenduse server on konfigureeritud õigesti, eriti SPN konfigureerimine. Veenduge, et Azure AD on ühendatud, et tagada, et selle domeenikontrolleri luuakse usalda Azure AD domeenikontrolleri sama Domeen domeeni. Veenduge, et kuupäeva ja kellaaja konfiguratsiooni Azure AD domeenikontrolleri sünkroonitakse. |
| 13020 - azure AD ei saa Kerberos Piletite kasutaja nimel alla laadida, sest kirjutamata serveri SPN pole määratletud. | Sel juhul võib viidata vale konfiguratsiooni vahel Azure AD ja domeenikontrolleri serverisse või kuupäeva ja kellaaja konfiguratsiooni nii masinad probleem. | Selle domeenikontrolleri keeldunute Kerberos Piletite, mis on loodud Azure AD. Veenduge, et Azure AD ja taustväärtus rakenduse server on konfigureeritud õigesti, eriti SPN konfigureerimine. Veenduge, et Azure AD on ühendatud, et tagada, et selle domeenikontrolleri luuakse usalda Azure AD domeenikontrolleri sama Domeen domeeni. Veenduge, et kuupäeva ja kellaaja konfiguratsiooni Azure AD domeenikontrolleri sünkroonitakse. |
| 13022 - azure AD ei saa autentida kasutaja, kuna serveri kirjutamata vastaks Kerberos-autentimine katsete HTTP 401 viga. | Sel juhul võib viidata vale konfiguratsiooni vahel Azure AD ja taustväärtus application server, või kuupäeva ja kellaaja konfiguratsiooni nii masinad probleem. | Taustväärtus serveri keeldunute Kerberos Piletite, mis on loodud Azure AD. Veenduge, et Azure AD ja taustväärtus rakenduse server on õigesti konfigureeritud. Veenduge, et kuupäeva ja kellaaja konfiguratsiooni Azure AD ja taustväärtus rakenduse server on sünkroonitud. |
| Veebisaidi lehte ei saa kuvada. | Teie kasutaja võidakse kuvada see tõrge avaldatud kui rakendus on rakendus IWA rakenduse avamiseks. Selle rakenduse jaoks määratletud SPN võivad olla valed. | IWA rakenduste: Veenduge, et selle rakenduse jaoks konfigureeritud SPN oleks õige. |
| Veebisaidi lehte ei saa kuvada. | Teie kasutaja võidakse kuvada see tõrge juurdepääsu avaldatud kui rakendus on rakendus OWA rakendus. See võib olla põhjustatud ühte järgmistest: <br> – See rakendus määratletud SPN on vale. <br> -Kasutaja, kes proovis rakenduse avamiseks on kasutusel proper ettevõtte konto asemel Microsofti kontoga sisse logida või kasutaja on kasutaja külalisena. <br> -Kasutaja, kes proovis rakenduse avamiseks selle rakenduse kohapeal küljel pole õigesti määratletud. | Juhised vastavalt sellele leevendada: <br> – Veenduge, et selle rakenduse jaoks konfigureeritud SPN on õige. <br> -Veenduge, et kasutaja logib oma ettevõtte konto, mis vastab teie domeeni avaldatud rakenduse abil sisse. Microsofti Account kasutajate ja Külastajate ei pääse IWA rakendused. <br> -Teha veenduge, et selle kasutaja määratletud selle kirjutamata rakenduse kohapeal arvutisse on asjakohased õigused. |
| See ettevõtte rakendus ei pääse juurde. Teil pole õigust selle rakenduse avamiseks. Autoriseerimine nurjus. Veenduge, et see rakendus kasutaja juurdepääsu määramiseks. | Kasutajate võidakse kuvada see tõrge juurde pääseda rakenduse avaldatud, kui nad kasutavad Microsofti kontod asemel oma ettevõtte kontoga sisse logima. Külalisi võib-olla ka see tõrge. | Microsofti Account kasutajate ja külalistele ei pääse IWA rakendused. Veenduge, et kasutaja logib sisse oma ettevõtte konto, mis vastab teie domeeni avaldatud rakenduse abil. |
| See ettevõtte rakendus ei pääse juurde kohe. Proovige hiljem uuesti... Konnektor on aegunud. | Kasutajate võidakse kuvada see tõrge avaldatud, kui nad pole õigesti määratletud selle rakenduse kohapeal küljel rakenduse avamiseks. | Veenduge, et teie kasutajad on asjakohased õigused selle kirjutamata rakenduse kohapeal arvutisse määratletud. |


## <a name="see-also"></a>Vt ka

- [Azure Active Directory puhverserverit lubamine](active-directory-application-proxy-enable.md)
- [Rakenduse puhverserveri rakenduste avaldamine](active-directory-application-proxy-publish.md)
- [Ühekordse sisselogimise lubamiseks](active-directory-application-proxy-sso-using-kcd.md)
- [Tingimusvormingu juurdepääsu lubamine](active-directory-application-proxy-conditional-access.md)

Uudiseid ja uusimate värskenduste, vaadake [rakenduse puhverserveri ajaveeb](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
