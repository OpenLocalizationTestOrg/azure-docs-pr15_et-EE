<properties
   pageTitle="Alustamine toimingud halduse komplekti turvalisus ja Audit lahenduse | Microsoft Azure'i"
   description="Selle dokumendi abil saate alustada toimingud halduse komplekti turvalisus ja Audit lahenduste võimalused jälgida oma hübriid pilve."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="get-started-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>
 
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Toimingute haldus komplekti turvalisus ja Audit lahenduse töötamise alustamine
Selle dokumendi abil saate kiiresti hakata toimingud halduse komplekti (OMS) turbe- ja auditilogi lahenduste võimalused, mida iga suvand suunavad.

## <a name="what-is-oms"></a>Mis on OMS?
Microsofti toimingute komplekti (OMS) on Microsofti pilvepõhise IT lahendus, mis aitab hallata ja kaitsta oma kohapealse ja pilveteenuse taristu. OMS kohta lisateabe saamiseks lugege artiklit [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>OMS turvalisus ja Audit armatuurlaud

Lahendus OMS turvalisus ja Audit pakub täielik vaadet oma organisatsiooni IT turvalisus asendi sisseehitatud otsing päringutega, kuid probleemide lahendamiseks, mis nõuab teie tähelepanu. **Turvalisus ja Audit** armatuurlaud on kõik seotud turvalisuse OMS avakuva. Üksikasjalik võimaldavad arvutite olekusse turvalisus. See hõlmab ka võimalus vaadata kõik sündmused viimase 24 tunni jooksul 7 päeva, või mõni muu kohandatud aja jooksul. **Turvalisus ja Audit** armatuurlaua juurdepääsemiseks tehke järgmist.

1. Klõpsake **vasakul paanil** **Microsoft Operations Management Suite** peamine armatuurlaud.
2. Klõpsake labale **sätted** jaotises **lahenduste** **turbe** -ja Audit.
3. **Turvalisus ja Audit** armatuurlaua kuvatakse:

    ![OMS turvalisus ja Audit armatuurlaud](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Kui avate esimest korda armatuurlaua ja teil jälgida OMS seadmed, paanid pole täidetud agendi saadud andmetega. Kui olete installinud agent, võib kuluda aega asustamiseks, seetõttu osad algselt võib olla puudu mõned andmed nagu need on endiselt üleslaadimise pilveteenusesse.  Sel juhul on tavaline kuvamiseks ilma konkreetseid teabe mõned paanid. Lugege lisateavet selle kohta, kuidas installida Windowsi süsteem ja [ühenduse Linuxi arvutite OMS](https://technet.microsoft.com/library/mt622052.aspx) OMS agent Linuxi selle toimingu kohta lisateabe saamiseks [ühenduse loomine Windows arvutite otse OMS](https://technet.microsoft.com/library/mt484108.aspx) .

> [AZURE.NOTE] Agent kogub teavet praeguse sündmusi, mis on lubatud, näiteks arvuti nimi, IP-aadress ja kasutajale nime põhjal. Siiski kogutakse pole/dokumendifailid, andmebaasi nimi või privaatne andmeid.   

Lahendused on kogumi loogika, visualiseerimine ja andmeid tuua reeglid, mis võtme kliendi probleemid. Turvalisus ja Audit on üks lahendus, teised saab lisada. Lugege artiklit [Lisa lahenduste](https://technet.microsoft.com/library/mt674635.aspx) Lisateavet selle kohta, kuidas lisada uue lahenduse.

Armatuurlaua OMS turvalisus ja Audit on algselt korraldatud neli põhilist Kategooriad:

- **Turvalisus domeenide**: selle ala, mida saab edasi uurida turvalisus kirjete aja jooksul, ründevara hindamise juurde, värskendada assessment, võrgu turvalisus, identiteeti ja Accessi arvutite turvalisuse sündmused ja kiire juurdepääs Azure'i turbekeskus armatuurlaua.
- **Kuid probleemid**: see suvand, mis võimaldab teil kiiresti tuvastada aktiivne probleemide arv ja tõsidust järgmiste probleemide korral.
- **Avastused (eelvaade)**: võimaldab teil rünnak mustrite tuvastamiseks visualiseerimine Turbeteatiste, nagu need toimuvad vastu oma ressursse.
- **Ohtu ärianalüüsi**: võimaldab teil rünnak mustrite tuvastamiseks poolt visualiseerimise pahatahtlik väljaminevate IP liikluse, pahatahtlik ohtu tüüp ja kaardi serverid, mis näitab, kus need IP-d on pärit koguarv. 
- **Levinud turvalisus päringud**: see suvand annab teile loendi levinumate turvalisus suhtes, mida saate jälgida oma keskkonnas. Ühes nende päringute klõpsamisel avaneb **Otsingu** tera selle päringu tulemused.

> [AZURE.NOTE] Lugege lisateavet selle kohta, kuidas OMS hoiab teie andmete turvaliseks, kuidas OMS tagab andmete.

## <a name="security-domains"></a>Turvalisus domeenid

Ressursside kontrollimisel oluline keskkonna hetkeseisu kiiresti juurdepääsu. Samas on oluline, et jälgida tagasi sündmused, mis on möödunud, mis võib kaasa tuua paremini mõista, mis juhtub teie keskkonnas, teatud aja. 

> [AZURE.NOTE] andmete säilitamine vastavalt selle OMS hinnad leping. Lisateabe saamiseks külastage [Microsofti Operations Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) hinnad lehe.

Langeva vastus ja kohtumeditsiini uurimine stsenaariumid otse kasu tulemuste paanil **Turvalisus kirjete aja jooksul** saadaval.

![Turvalisus kirjete aja jooksul](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Kui klõpsate sellel paanil, avatakse **Otsingu** tera, nähtaval päringu tulem **Turvalisus sündmuste** jaoks (tüüp = SecurityEvents) andmete põhjal seitsme viimase päeva, nagu allpool näidatud:

![Turvalisus kirjete aja jooksul](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Otsingutulemite jagatakse kaks paani: vasakul paanil annab teile turvalisus sündmuste, mis ei leia arvutites, kus need sündmused ei leitud, kontod, mis olid avastatud nende arvutites arv ja tegevused, jaotuse. Paremal paanil pakub teile kokku tulemused ja turvalisuse sündmused arvuti nimi ja sündmuse toimingute kronoloogiline vaate. Võite klõpsata ka **Kuvada rohkem** see sündmus, nt sündmuse andmed, sündmuse ID-d ja Sündmuse allikas üksikasjade kuvamiseks.

> [AZURE.NOTE] OMS otsingupäringu kohta lisateabe saamiseks lugege [OMS otsida viide](https://technet.microsoft.com/library/mt450427.aspx).

### <a name="antimalware-assessment"></a>Ründevaratõrje hindamine

See suvand võimaldab teil kiiresti tuvastada arvutisse pole piisavalt kaitse ja arvutites, mis on kahjustada ründevara tükk. Ründevara assessment oleku ja tuvastatud ohud jälgida serverites on lugeda ja siis saadetakse andmete töötlemiseks pilveteenuses OMS teenuse. Serverite tuvastati ohtude ja piisavalt kaitsega serverid on näidatud ründevara assessment armatuurlaud, millele pääseb **Ründevaratõrje Assessment** paani klõpsamisel. 

![ründevara hindamine](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Nii nagu mis tahes muud reaalajas paan OMS armatuurlaua saadaval, kui klõpsate seda, **Otsingu** tera avaneb päringu tulem. See suvand, kui klõpsate jaotises **Kaitse oleku** **Mitte aruandlus** suvandi peate päringu tulem, mis näitab selle üks kirje, mis sisaldab arvuti nimi ja selle asukoha, nagu allpool näidatud:

![otsingutulem](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [AZURE.NOTE] *Rank* on vastava hinde, andes kajastamiseks kaitse olekut (, välja värskendatud, jne) ja ohtude, mis ei leita. Mis on nimega arv aitab liitmised teha.

Kui klõpsate nuppu arvuti nimi, on teil selle arvuti oleku kaitse kronoloogilises vaate. See on väga kasulik stsenaariumid, mis on vaja mõista, kui selle ründevaratõrje üks kord installiti ja mingil hetkel see on eemaldatud.   

### <a name="update-assessment"></a>Värskendage hindamine 

See suvand saab kiiresti määrata üldine käes turvalisus võimalikke probleeme, ja kas ja kuidas kriitilised värskendused on teie keskkonnas. OMS turvalisus ja Audit lahenduse ainult pakuvad järgmised värskendused visualiseerimine, tegelikke andmeid pärineb [Värskenduste lahendused](https://technet.microsoft.com/library/mt484096.aspx), mis on erinevate mooduli OMS sees. Siin on näide värskendused:

![süsteemi värskendused](./media/oms-security-getting-started/oms-getting-started-fig6.png)

> [AZURE.NOTE] lahendus värskenduste kohta leiate lisateavet lugeda [värskendada serverid lahendusega süsteemi värskendused](https://technet.microsoft.com/library/mt484096.aspx).

### <a name="identity-and-access"></a>Identiteedi ja juurdepääs

Identiteedi peaks olema juhtelemendi lennuk oma ettevõtte kaitsta teie identiteedi peaks olema teie prioriteet. Kui varem olid piirid ümber ettevõtted ja nende piirid üks esmane kaitsev piirmäärad, tänapäeval rohkem andmeid ja teisaldada pilve veel rakendusi ID muutub uue perimeetri. 

> [AZURE.NOTE] praegu andmed põhineb ainult turvalisuse sündmuste logi sisse andmete (Sündmuse ID 4624) tulevaste Office 365 sisselogimise ja Azure AD andmete kaasatakse ka.

Oma identiteedi tegevuste jälgides on võimalik teha ennetavad toimingud enne juhtum kohas või reaktiivne toimingud rünnaku katse lõpetada. **Identiteedi ja juurdepääsu** armatuurlaua pakub ülevaadet oma identiteedi olekusse, sh sisselogimist nurjunud katsete arv, nende katsete, kontod, mis on lukus, kontod, millel on kasutatud kasutajakonto parooli lähtestamine ja praegu arvu kontod, mis on sisse logitud või muudetud. 

**Identiteedi ja juurdepääsu** paani klõpsamisel kuvatakse järgmine armatuurlaud.

![identiteedi ja juurdepääs](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

Lisateavet selle armatuurlaua kohe aitavad teil tuvastada võimalikud kahtlaste tegevuste. Näiteks on 338 katsete **administraator** ja 100% nende katsete sisselogimiseks nurjus. See võib olla põhjustatud jõuvõtete ründamine selle kontoga. Kui klõpsate selle konto, siis saada lisateavet, mis aitavad teil kindlaks teha see võimalik rünnak target ressurss:

![Otsingutulemid](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Üksikasjalik aruandes esitatakse oluline teave sel juhul, sh: target arvuti, tüüp (see juhul võrku sisselogimine) sisselogimise, tegevuse (sellisel juhul juhul 4625) ja iga katse täielik ajaskaala. 

### <a name="computers"></a>Arvutites

Sellel paanil saab juurdepääsu kõik arvutid aktiivselt turvalisus üritused. Kui klõpsate sellel paanil kuvatakse loendis arvutite turvalisuse sündmused ja sündmuste arv igas arvutis.

![Arvutites](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Saate jätkata oma uurimine igas arvutis, klõpsates ja vaadake üle turvalisuse sündmused, mis on lipuga märgitud.

### <a name="azure-security-center"></a>Azure'i turbekeskus

Sellel paanil on põhimõtteliselt otsetee Azure'i turbekeskus armatuurlaua juurdepääsu. See lahendus kohta lisateavet lugeda [Azure'i turbekeskus töötamise alustamine](../security-center/security-center-get-started.md) .

## <a name="notable-issues"></a>Kuid probleemid

Juhised selle rühma suvandite on esitada küsimusi, et teil on teie keskkonnas nende üksuste kriitiline, hoiatus ja teatised kiiresti vaadata. Visualiseeringu järgmiste probleemide korral, kuid see on aktiivne probleemi tüüp paani ei luba teil uurida täpsemat teavet nende jaoks peate kasutama alumine osa sellel paanil, mis sisaldab probleemi (nimi) nime, mitu objektide oli see juhtub (arv) ja kuidas kriitilise on (raskusaste).

![Kuid probleemid](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Saate vaadata järgmiste probleemide korral ei kuulu erinevatel aladel jaotises **Turve domeenid** , mis suurendab see vaade soovidele: visualiseerimine kõige olulisemad probleemid, mis on teie keskkonnas ühest kohast.

## <a name="detections-preview"></a>Avastused (eelvaade)

See suvand juhised on võimaldada selle kiiresti tuvastada võimalike ohtudega nende keskkonna kaudu ja selle ohtu tõsidust.

![Ohtu Inteli](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Selle suvandi saate kasutada ka käigus langeva vastuse hindamise ja rünnak kohta lisateabe saamiseks.

> [AZURE.NOTE] OMS juhtum vastamise kasutamise kohta lisateabe saamiseks vaadake [Kuidas võimendada Azure'i turbekeskus ja Microsoft Operations Management Suite jaoks vastuse juhtum](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703).

## <a name="threat-intelligence"></a>Ohtu ärianalüüsi

Uue ohtu ärianalüüsi jaotise turvalisus ja Audit lahendus tänav visualiseeritakse võimalik rünnak mustrite on mitu võimalust: serverid pahatahtlik väljaminevate IP liikluse, pahatahtlik ohtu tüübist ja kaardi, mis näitab, kus need IP-d on pärit koguarv. Saate nendega suhelda kaardi ja IP-d lisateabe saamiseks klõpsake nuppu.

Kollane nööpnõeltega kaardil näitab sissetulev liiklus pahatahtlik IP-d. Ei ole serverid, mis on Interneti-kasutajatele pahatahtlik liiklust aeg-ajalt, kuid soovitame üle vaadata nende katsete veendumaks, et ükski neist õnnestus. Nende näitajate põhinevad IIS-i logid WireData ja Windowsi tulemüüri logib.  

![Ohtu Inteli](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Levinud turvalisus päringud

Üldiste turvalisus päringute loendit saab kasulikke ressursi teabele kiiresti juurde ja seda vastavalt teie keskkonnas vajadustele kohandada. On need levinud päringud.

- Kõigi turvalisus tegevuste
- Turvalisus tegevusi arvutis "computer01.contoso.com" (asendaja arvuti nimi)
- Turvalisus tegevusi arvutis "computer01.contoso.com" konto "Administraator" (asendaja oma arvuti ja konto nimed)
- Logige sisse arvutisse tegevuse
- Kontod, kes lõpetada Microsofti ründevaratõrje arvutitest
- Arvutites, kus Microsoft ründevaratõrje protsess on lõpetatud
- Arvutites, kus "hash.exe" on täidetud (asendage muu protsess nimega)
- Kogu protsessi nimed, mis olid täidetud
- Tegevuste, kontole sisse logida
- Kontod, kes kaugühenduse teel sisse loginud arvuti "computer01.contoso.com" (asendaja arvuti nimi)

## <a name="see-also"></a>Vt ka

Selles dokumendis võeti kasutusele OMS turvalisus ja Audit lahenduseks. OMS turvalisuse kohta lisateabe saamiseks lugege järgmisi artikleid:

- [Toimingute haldus komplekti (OMS) ülevaade](operations-management-suite-overview.md)
- [Jälgimine ja toimingute haldus komplekti turvalisus ja Audit lahenduse Turbeteatiste reageerimine](oms-security-responding-alerts.md)
- [Toimingute haldus komplekti turvalisus ja Audit lahenduse jälgimisega seotud ressursid](oms-security-monitoring-resources.md)
