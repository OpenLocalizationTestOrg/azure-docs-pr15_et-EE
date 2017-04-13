<properties
    pageTitle="Automaatse SaaS rakenduse kasutaja ettevalmistamise Azure AD | Microsoft Azure'i"
    description="Sissejuhatus kasutamist Azure AD automaatselt ette valmistada asutusest ja pidevalt värskendada kasutajakontode mitu kolmanda osapoole SaaS rakendust."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Kasutajate ettevalmistamine ja Azure Active Directory SaaS rakenduste Deprovisioning automatiseerimine

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>Mis on automatiseeritud kasutaja ettevalmistamise SaaS rakendusi?

Azure Active Directory (Azure AD) võimaldab automatiseerida loomise, hooldus ja eemaldamine kasutaja identiteedid pilveteenuse ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) rakenduste (nt Dropboxi, Salesforce, ServiceNow jm).

**Allpool on näited sellest, mida see funktsioon võimaldab teil teha.**

- Automaatselt luua uued kontod õige SaaS rakendused uue inimeste jaoks, kui nad oma meeskonda.
- Automaatselt desaktiveerida kontod SaaS rakenduste kaudu, kui inimesed paratamatult jäta meeskond.
- Tagada, et identiteedid SaaS rakendused on ajakohased muudatused kataloogis põhjal.
- Sätte mittekasutaja objektid, nt rühmad, SaaS rakenduste, mis toetavad neid.

**Automatiseeritud kasutaja ettevalmistamise sisaldab ka järgmisi funktsioone:**

- Võimalus vastavad olemasoleva identiteetide vahel Azure AD ja SaaS rakendused.
- Spikri Azure AD kohandussuvandid mahu praeguse konfiguratsioone SaaS apps, mis on teie ettevõttes on praegu kasutusel.
- Valikuline meiliteatiste ettevalmistamise tõrkeid.
- Aruandlus- ja tegevuse logide aitab jälgida ja tõrkeotsing.

##<a name="why-use-automated-provisioning"></a>Miks kasutada automatiseeritud ettevalmistamise?

Mõned levinud põhjuste selle funktsiooni kasutamise kohta on järgmised:

- Kulude, ebatõhusust ja inimeste viga, mis on seotud käsitsi ettevalmistamise protsesside vältimiseks.
- Ettevõtte tagada kiire eemaldamine kasutajate identiteetide võtme SaaS rakenduste kaudu, kui nad väljuvad organisatsiooni.
- Hõlpsasti importida kasutajate hulgilisamine arv SaaS rakenduseks.
- Lahendus, millel on teie ettevalmistamise ning pakub käivitage sama rakenduse access poliitikate jaoks Azure AD ühekordse sisselogimise määratletud välja.

##<a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

**Kui sageli kirjutamine Azure AD directory muudatuste SaaS rakendus?**

Azure AD kontrollib muudatuste iga 5 – 10 minuti järel. Kui SaaS rakendus on tagasi mitu tõrgete (nt sobimatud administraatori identimisteave puhul), siis Azure AD järk-järgult aeglane selle sagedus kuni üks kord päevas kuni tõrked.

**Kui kaua võtab minu kasutajaid ette valmistada?**

Täiendavad muudatused ei juhtu peaaegu kohe, kuid kui proovite ettevalmistamise enamik kataloogi, siis see sõltub kasutajad ja rühmad, et teil on arv. Väike kataloogide teha vaid paar minutit, keskmise suurusega kataloogide võib kuluda mitu minutit ja väga suurte kataloogide võib kuluda mitu tundi.

**Kuidas saab praeguse ettevalmistamise töö edenemist jälgida?**

Saate vaadata konto ettevalmistamise aruande kataloogi jaotises aruanded. Teine võimalus on külastada SaaS rakendus, mida teil on ettevalmistamine ja vaadake, kas lehe allosas jaotises "Integreerimine Status" vahekaart armatuurlaud.

**Kuidas ma tean, kui kasutajad ei saada ette valmistatud õigesti?**

Ettevalmistamise konfiguratsiooni lõpus viisardi seal on võimalus tellida meiliteatised ettevalmistamise ebaõnnestumist. Saate vaadata ka ettevalmistamise tõrgete aruande kuvamiseks, millistel kasutajatel nurjus ette valmistada ja miks.

**Kas Azure AD kirjutada muudatused rakendusest SaaS tagasi kataloogi?**

Enamik SaaS rakenduste ettevalmistamise on väljaminev ainult, mis tähendab, et kasutajaid kirjutada kataloogist rakendusse ning muudatuste rakenduse kaudu ei saa tagasi kirjutada kataloogi. [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx), kuid ettevalmistamise on sissetulev ainult, mis tähendab, et kasutajad imporditakse kataloogi Workday kaudu, mis samuti kataloogis muudatusi ei saada kirjutatud tagasi Workday.

**Kuidas saata tagasisidet engineering meeskond?**

Võtke meiega ühendust – [Azure Active Directory tagasiside Foorum](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="how-does-automated-provisioning-work"></a>Kuidas automaatne ettevalmistamise toimib?

Azure AD sätted SaaS rakendusi kasutajatele, luues ühenduse ettevalmistamise iga rakenduse tarnija lõpp-punktid. Nende lõpp-punktid luba Azure AD programmiliselt luua, värskendada ja eemaldada kasutajaid. Allpool on lühiülevaade erinevaid toiminguid, mis Azure AD võtab ettevalmistamise automatiseerimiseks.

1. Kui lubate ettevalmistamise rakenduse esimest korda, tehakse järgmist:
 - Azure AD proovib olemasolevatele kasutajatele SaaS rakenduse kataloogis vastavate identiteetidega vastavaks. Kui kasutaja on täidetud, saavad nad *pole* lubatud automaatselt ühekordse sisselogimise jaoks. Et kasutajatel on juurdepääs rakenduse, ta peab olema selgesõnaliselt määratud rakendusse Azure AD, otse või rühmakuuluvust kaudu.
 - Kui olete juba määratlenud, millistel kasutajatel peaks olema määratud rakendus ja Azure AD ei leia olemasolevaid kontosid, nende kasutajate jaoks, on Azure AD ettevalmistamise uute kontode neid rakenduse.
2. Kui algse sünkroonimine on lõpule viidud, nagu on kirjeldatud ülal, kontrollib Azure AD iga 10 minuti järel jaoks järgmisi muudatusi:
 - Kui rakendus on määratud uute kasutajate (otse või rühmakuuluvust kaudu), siis neid ette valmistatud SaaS rakenduse uus konto.
 - Kui kasutaja juurdepääs on eemaldatud, siis oma konto SaaS rakenduses kuvatakse tähisega keelatud (kasutajad on kunagi täielikult kustutada, mis kaitseb teie andmete kaotsimineku korral olekuteenus).
 - Kui kasutaja viimati määratud rakendusse ja nad on juba konto SaaS rakendus, selle konto märgitakse lubatud ja teatud kasutaja atribuutide võib värskendada, kui need on aegunud võrreldes kataloogi.
 - Kui kasutaja teabe (nt telefoninumbrit, kontori asukoht jne) on muudetud kataloog, siis värskendatakse ka seda teavet rakenduse SaaS.

Kuidas atribuudid on vastendatud vahel Azure AD kohta lisateabe saamiseks ja SaaS rakendust, leiate artiklist [Atribuudi vastendused kohandamise](active-directory-saas-customizing-attribute-mappings.md)kohta.

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>Selle tugi automatiseeritud kasutaja ettevalmistamise rakenduste loend

Klõpsake rakenduse kuvamiseks kohta, kuidas konfigureerida automatiseeritud ettevalmistamise seda õpetus:

- [Väljale](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [Nõus](http://go.microsoft.com/fwlink/?LinkId=309575)
- [DocuSign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [Dropbox for Business](http://go.microsoft.com/fwlink/?LinkId=309581)
- [Google Appsi](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [Salesforce'i](http://go.microsoft.com/fwlink/?LinkId=286017)
- [Salesforce'i Liivakasti](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [WORKDAY](http://go.microsoft.com/fwlink/?LinkId=690250) (Sissetuleva meili ettevalmistamise)

Selleks rakenduse toetamiseks automatiseeritud kasutaja ettevalmistamise, esmalt määrama vajalikud lõpp-punktid, mis võimaldavad väliste programmide automatiseerimiseks loomise, hoolduse ja kasutajate eemaldamine. Seetõttu ei kõik SaaS rakendused ühilduvad seda funktsiooni. Rakendused, mis toetavad seda Azure AD engineering meeskonnatöö siis võimalik ettevalmistamise konnektor nende rakenduste koostamine ja selle töö prioriteetne ja võimalikke klientide vajaduste järgi.

Azure AD matemaatika meeskonnatöö taotlemiseks ettevalmistamise tugi täiendavad rakendused, esitage sõnumi [Azure Active Directory tagasiside Foorum](https://feedback.azure.com/forums/169401-azure-active-directory/)kaudu.

##<a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Kasutajate ettevalmistamine atribuudi vastendused kohandamine](active-directory-saas-customizing-attribute-mappings.md)
- [Avaldiste atribuudi vastendused kirjutamine](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Ulatuse määramine filtrid kasutajate ettevalmistamine](active-directory-saas-scoping-filters.md)
- [Luba automaatne ettevalmistamise kasutajatele ja rühmadele Azure Active Directory rakendustele SCIM abil](active-directory-scim-provisioning.md)
- [Konto ettevalmistamise teatised](active-directory-saas-account-provisioning-notifications.md)
- [Õpetused kuidas integreerida SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
