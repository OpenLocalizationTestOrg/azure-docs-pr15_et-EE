<properties
   pageTitle="Azure Active Directory aruandlus - eelvaade | Microsoft Azure'i"
   description="Loendab erinevate saadaval Azure Active Directory eelvaade"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory aruandlus - eelvaade

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-reporting-azure-portal.md)
- [Azure'i klassikaline portaal](active-directory-reporting-guide.md)

*Need dokumendid on osa [Azure Active Directory aruandluse juhend](active-directory-reporting-guide.md).*

Aruandlusteenuste eelvaates Azure Active Directory, saate kõik andmed, mida vajate määrata, kuidas teie keskkond, mis teeb. [Mis on eelvaateversioonis?](active-directory-preview-explainer.md)

On kaks peamist teatamine.

- **Sisselogimise tegevuste** – teavet hallatavate rakenduste ja kasutajale sisselogimine tegevuste kasutamine

- **Auditilogide** - tegevuse Süsteemiteave kasutaja ja rühma haldamine, teie hallatavate rakenduste ja directory tegevuste kohta

Sõltuvalt andmeid, mida otsite, pääsete need aruanded ühte, klõpsates nuppu **kasutajad ja rühmad** või **ettevõtte rakenduste** [Azure portaalis](https://portal.azure.com)teenuste loendis.

## <a name="sign-in-activities"></a>Sisselogimise tegevuste

### <a name="user-sign-in-activities"></a>Kasutaja sisselogimine tegevuste

Aruande kasutaja sisselogimise teave leiate vastused küsimustele nagu:

- Mis on kasutaja mustrit?
- Mitu kasutajat on kasutajate sisse loginud üle nädala?
- Mis on need sisselogimist olekut?

Oma andmete pääseb kasutaja sisselogimise graafik jaotises **Ülevaade** jaotises **kasutajad ja rühmad**.

 ![Aruandlus] (./media/active-directory-reporting-azure-portal/05.png "Aruandlus")

Kasutaja sisselogimise graafik esitab nädala liitmised märgi lisandmoodulid antud aja jooksul kõikide kasutajate jaoks. Aja jooksul vaikeväärtus on 30 päeva.

![Aruandlus] (./media/active-directory-reporting-azure-portal/02.png "Aruandlus")

Päev kuvatakse diagrammil klõpsamisel saate sisselogimise tegevuste üksikasjalik loend.

![Aruandlus] (./media/active-directory-reporting-azure-portal/03.png "Aruandlus")

Iga rea sisselogimise tegevuste loend annab teile üksikasjalikku teavet valitud sisselogimist nagu:

- Kes on sisse logitud?

- Mis on seotud UPN-i?

- Millist rakendus ei Logi sisse suunatud?

- Mis on IP-aadress on sisselogimine?

- Kui suur oli sisselogimise olekut?

### <a name="usage-of-managed-applications"></a>Hallatavate rakenduste kasutamist

Rakenduse-kesksele ülevaate oma andmeid, saate vastata küsimustele nagu:

- Kes on kasutusel minu rakendusi?

- Mis on ülemisel 3 rakenduste ettevõttes?

- Mul on võetud viimati välja rakenduse. Kuidas seda teha?


Oma andmete pääseb ülemisel 3 rakenduste ettevõtte viimase 30 päeva aruandes jaotises **ettevõtte rakenduste**jaotises **Ülevaade** .

 ![Aruandlus] (./media/active-directory-reporting-azure-portal/06.png "Aruandlus")


Rakenduse kasutamine Graphi nädala liitmised, logige oma top 3 rakenduste lisandmoodulid antud aja jooksul. Aja jooksul vaikeväärtus on 30 päeva.

![Aruandlus] (./media/active-directory-reporting-azure-portal/78.png "Aruandlus")

Kui soovite, saate mõne kindla rakenduse liikumine.

![Aruandlus] (./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Aruandlus")


Rakenduse kasutamine Graphi päeva klõpsamisel saate sisselogimise tegevuste üksikasjalik loend.


![Aruandlus] (./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Aruandlus")



**Sisselogimist** suvand annab teile täieliku ülevaate kõikide sündmuste oma rakendusi.

![Aruandlus] (./media/active-directory-reporting-azure-portal/85.png "Aruandlus")

Veeru valija abil saate valida andmete väljad, mida soovite kuvada.

![Aruandlus] (./media/active-directory-reporting-azure-portal/column_chooser.png "Aruandlus")



### <a name="filtering-sign-ins"></a>Filtreerimine sisselogimist

Saate filtreerida ajavahemik kuvatava andmehulga piiritlemiseks sisselogimist.

![Aruandlus] (./media/active-directory-reporting-azure-portal/927.png "Aruandlus")


Mõne muu meetodi sisselogimise tegevuste kirjete filtreerimiseks on kindlate kirjete otsimine.
Otsingu meetod võimaldab teil oma sisselogimist teatud **kasutajate**, **rühmade** või **rakenduste**ümber ulatus.


![Aruandlus] (./media/active-directory-reporting-azure-portal/84.png "Aruandlus")

## <a name="audit-logs"></a>Auditilogide

Azure Active Directory Auditeerimispoliitika logid esitada süsteemi tegevuse nõuetele vastavuse andmeid.

On peamised kolmele jaoks seotud auditeerimine Azure'i portaalis.

- Kasutajad ja rühmad   

- Rakenduste

- Kataloog   


Auditilogi aruande tegevuste täieliku loendi leiate [auditilogi aruande sündmuste loendi](active-directory-reporting-audit-events.md#list-of-audit-report-events).


Teie kõik Auditeerimispoliitika andmetele pääseb **auditilogid** **Azure Active Directory** **tegevuse** osas.


![Auditi] (./media/active-directory-reporting-azure-portal/61.png "Auditi")


Mõne auditilogi on loendivaate, kus kuvatakse osalejate (kes) (mis) tegevuste ja eesmärkide.


![Auditi] (./media/active-directory-reporting-azure-portal/345.png "Auditi")


Üksuse loendivaates klõpsates saate täpsemat teavet.

![Auditi] (./media/active-directory-reporting-azure-portal/873.png "Auditi")




### <a name="users-and-groups-audit-logs"></a>Kasutajate ja rühmade auditilogid


Kasutaja ja rühma-põhiste auditilogi aruanded, võite saada vastuseid küsimustele nagu:

- Mis tüüpi värskendused on rakendatud kasutajad?

- Mitu kasutajat, on muutunud?

- Kui palju paroole on muutunud?

- Mis on kataloog teinud administraator?

- Mis on rühmad, mis on lisatud?

- Kas rühmade liikmelisuse muudatustega?

- Rühma omanikud on muudetud?

- Millised litsentsid on määratud rühmale või kasutajale?


Kui soovite lihtsalt Auditeerimispoliitika andmeid, mis on seotud kasutajad ja rühmad, leiate jaotises **auditilogid** filtreeritud vaate **tegevuse** jaotises **kasutajad ja rühmad**.


![Auditi] (./media/active-directory-reporting-azure-portal/93.png "Auditi")


### <a name="application-audit-logs"></a>Rakenduse auditilogid

Koos rakenduse-põhiste auditilogi aruanded, saate vastused küsimustele, näiteks:

- Millised rakendused, mis on lisatud või uuendatud?

- Millised rakendused on eemaldatud?

- Teenuse põhimõte rakendus on muutunud?

- Rakenduste nimed on muudetud?

- Kes andis nõusoleku rakendus?


Kui soovite lihtsalt Auditeerimispoliitika andmeid, mis on seotud rakendusi, leiate **auditilogid** alusel filtreeritud vaate **ettevõtte rakenduste** **tegevuste** osas.


![Auditi] (./media/active-directory-reporting-azure-portal/134.png "Auditi")


### <a name="filtering-audit-logs"></a>Auditilogide filtreerimine

Aruande saate filtreerida ajavahemik kuvatava andmehulga piiritlemiseks.

![Auditi] (./media/active-directory-reporting-azure-portal/324.png "Auditi")

Teine meetod on auditilogi kirjete filtreerimiseks on kindlate kirjete otsimine.

![Auditi] (./media/active-directory-reporting-azure-portal/237.png "Auditi")

## <a name="next-steps"></a>Järgmised sammud

Teemast [Azure Active Directory aruandluse juhend](active-directory-reporting-guide.md).
