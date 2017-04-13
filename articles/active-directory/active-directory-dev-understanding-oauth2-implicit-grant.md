<properties
   pageTitle="Peidetud OAuth2 mõistmine anda Azure Active Directory kulgemist | Microsoft Azure'i"
   description="Lugege lisateavet Azure Active Directory rakendamine peidetud OAuth2 anda meilivoo, ja kas see on õige rakenduse."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>OAuth2 peidetud anda voogu rakenduses Azure Active Directory (AD) mõistmine

OAuth2 peidetud anda on tuntud on anda pikima loetelu turvakaalutlused OAuth2 kirjelduses. Ja veel, mis rakendatakse ADAL JS ja soovitame SPA rakenduste kirjutamisel üks lähenemine on. Mis annab? See on kõik küsimus kompromisse: ja peidetud anda selgub, on parim lahendus teostate rakendusi, mis kasutavad Web API kaudu JavaScript brauseri kaudu.

## <a name="what-is-the-oauth2-implicit-grant"></a>Mis on OAuth2 peidetud anda?

Põhiliselt [koodi OAuth2 loa andmine](https://tools.ietf.org/html/rfc6749#section-1.3.1) on loa andmine, mis kasutab kahte eraldi lõpp-punktid. Luba lõpp-punkti kasutatakse kasutaja suhtluse etapp, mille tulemuseks on kood. Turbeloa lõpp-punkti kasutatakse kliendi poolt vahetamine juurdepääsu sümboolse ja sageli ühte värskendamise luba kood. Veebirakenduste vajalike esitada oma rakenduse mandaat Turbeloa lõpp-punkti, et loa serveris saate kliendi autentimiseks.

[OAuth2 peidetud anda](https://tools.ietf.org/html/rfc6749#section-1.3.2) on lubamisel saada juurdepääsu luba (ja id_token puhul [OpenId Connect](http://openid.net/specs/openid-connect-core-1_0.html)) klient variant otse autoriseerimine lõpp-punkti, ilma poole pöördumist Turbeloa lõpp-punkti ega autentimisel klientrakendust. See variant oli spetsiaalselt JavaScripti põhineva veebibrauseris töötavad rakendused: algne OAuth2 kirjelduses, tagastatakse sõned URI fragment. Mida teeb Turbeloa bittide kättesaadavaks JavaScripti koodi kliendi, kuid see tagab, et need ei lisata ümbersuunamised suunas server. Brauseri kaudu sõned esitus suunab otse autoriseerimine lõpp-punkti. Samuti on see eelis, mis tahes nõuded cross origin kõnesid, mis on vajalikud JavaScripti rakenduse vajadusel pöörduda Turbeloa lõpp-punkti kõrvaldamise.

OAuth2 peidetud anda olulisem omadus on asjaolu, näiteks liigub sujuvalt kunagi saatja värskendamise sõned kliendile. Nagu näeme järgmises jaotises, mis pole siiski vajalik ja tegelikult oleks turvalisuse probleem.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Sobiva OAuth2 peidetud anda stsenaariumid

Nagu OAuth2 määratlus, ise teatab, et on peidetud anda lubamiseks kasutajaagendi rakendused – see tähendab JavaScripti rakenduste käivitamisel sees brauseris välja töötatud. Rakenduste asjaolu on JavaScripti koodi kasutatakse juurdepääs serveri ressursse (tavaliselt Web API) ja rakendus Kasutuskogemuse vastavalt värskendamine. Mõelda, nt Gmail või Outlook Web Accessi rakenduste: kui valite oma sisendkaustas sõnumi, ainult sõnumi visualiseeringu paani muutub uue valiku kuvamiseks ülejäänud lehe jääb muutmata. See on erinevalt traditsiooniline redirect-põhiste veebirakenduste, kus iga kasutaja suhtluse tulemuseks tagasipostitamine tervet lehte ja terve lehe renderdamise serveri uus tagasiside.

Rakendusi, mis võtta moodust JavaScripti vastavalt oma äärmise nimetatakse ühe lehe rakenduste või spaad: idee on, et need rakendused olla ainult HTML-lähtelehe- ja seotud JavaScripti, koos kõigi järgnevate kasutusviisid käitatava Web API kõned teostada JavaScripti kaudu. Siiski hü võimalused, kui rakendus on peamiselt postback põhinev, kuid aeg-ajalt JS kõned sooritab, ei ole aeg-ajalt – arutelu peidetud meilivoo kasutuse kohta on oluline nende jaoks.

Redirect põhiseid rakendusi secure tavaliselt oma taotlusi kaudu cookies, kuid see lähenemine ei tööta ka JavaScript rakendusi. Ainult töö küpsised, loodud, kuigi JavaScripti kõned võib suunatud muude domeenide domeeni suhtes. Tegelikult sageli see juhul: mõelda kasutada Microsoft Graphi API, Office API, Azure'i API – väljaspool domeeni kaudu, kui rakendus on kätte elavate kõik rakendused. Kasvutrendi JavaScripti rakenduste on pole kirjutamata, tuginedes 100%, 3 isikule rakendada oma Web API-d.

Eelistatud meetod kaitsmine Web API kõned praegu kasutada OAuth2 esitaja Turbeloa moodust, kus iga kõne on kaasas OAuth2 juurdepääsu sümboolse. Veebi-API uurib sissetulevad juurdepääsu luba ja leidmisel ei vaja otsinguulatuste seda määrab juurdepääsu toiming. Peidetud voogu võimaldab mugav JavaScripti rakendusi hankida sõned access Web API, pakub mitmeid eeliseid seoses küpsiseid:

- Märkide saab usaldusväärselt ilma vajaduseta rist origin kõned – URI sõned on saatja tagab, et sõned on pole liigutatakse uuesti kohustuslik registreerimine
- JavaScripti rakendusi saate hankida nii palju Accessi sõned, nagu on vaja, nii palju Web API nad suunata – koos piirang domeenide jaoks
- HTML5 funktsioone, nagu seansi või kohalikku anda täielik kontroll Turbeloa vahemällu talletamine ja neid elu jooksul haldus küpsiseid haldus on läbipaistmatu rakendusse
- Accessi sõned ei suhtes Cross taotluse võltsimist (CSRF) eest

Peidetud anna voogu ei anna värskendamise märkide enamasti turvalisuse põhjustel. Värskenda luba pole rakendatud nii kitsa nimega märkide juurdepääsu andmise seega tekitades palju kahju juhuks, kui see on lekkis palju power. Peidetud voogu sõned edastatakse URL-i, seega pealtkuulamine risk on suurem kui autoriseerimine koodi anda.

Pange tähele, et JavaScript rakenduse teise süsteemi käsutuses pikendamiseks Accessi sõned mandaadi kasutajalt küsimata korduvalt. Rakenduse abil saate peidetud IFRAME'i uue Turbeloa taotluste suhtes autoriseerimine lõpp-punktile Azure AD: kui brauseri on veel aktiivse seansi (vt: on seansi küpsise) suhtes Azure AD domeeni, autentimine taotluse ilma vajaduseta kasutaja suhtluse edukalt kulgevate. 

See mudel annab JavaScripti rakenduse võimalus sõltumatult pikendamise Accessi sõned ja isegi hankida uusi uue API (eeldades, et kasutaja on varem nende jaoks kohustuslik. See aitab vältida lisatud koormust hankimisega, haldamine ja kaitsmine kõrge väärtus artefakt, nt märgiks värskendamine. Artefakt, mis võimaldab vaikne pikendamise Azure AD seansi küpsise, hallatakse väljaspool rakendus. Seda moodust eelis on kasutaja välja logida: Azure'i AD, kasutades rakendusi Azure AD, mis tahes brauseri vahekaartide töötab sisse logitud. Selle tulemuseks Azure AD seansi küpsis kustutamise ja JavaScripti rakenduse automaatselt kaotada võimalus pikendada sõned allkirjastatud välja kasutaja.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Sobib peidetud anda rakendus?

Peidetud anda esitab rohkem riske, kui muud toetused. Peate tähelepanu pöörama alad on ka dokumenteerida (vt näiteks [Väärkasutamine, Accessi Turbeloa jäljendada ressursi omanikule peidetud Flow] [ OAuth2-Spec-Implicit-Misuse] ja [OAuthi 2.0 ohtu mudeli ja turvakaalutlused][OAuth2-Threat-Model-And-Security-Implications]). Suurem risk profiili aga suuresti asjaolu, et see on mõeldud rakendusi, mis tee active koodi, kätte remote ressursi brauseris lubada. Kui kavatsete SPA arhitektuur, pole taustväärtus komponendid või seda autonoomsest Web API kaudu JavaScripti, on soovitatav kasutada varjatud kulgemist Turbeloa omandamise.

Kui teie taotlus on kohalikke klient, ei ole peidetud voogu väga sobivad. Azure AD seansi küpsise kontekstis on omakliendi puudumine jätab oma rakenduse tähendab kaua elanud seansi säilitada. Mis tähendab, et teie taotlus korduvalt Küsi kasutaja saamiseks Accessi sõned uue ressursid.

Kui teil on tekkinud veebirakenduse, mis sisaldab soovitud taustväärtus ja tarbimine API kaudu oma koodi kirjutamata, peidetud vool ei ole hea sobib. Muud annab teile palju power. Näiteks OAuth2 kliendi mandaadi andmine pakub võimalust saada märgid, mis vastavalt rakendusse, mitte kasutaja esinduste määratud õigustele. See tähendab, et kliendil on võimalus säilitada Programmeerimisjuurdepääs ressursse isegi siis, kui kasutaja pole aktiivselt seanss ja jne. Mitte ainult seda, kuid sellise toetused annavad tagab suurem turvalisus. Näiteks Accessi sõned kunagi läbimine kasutaja brauseri, nad ei salvestata brauseri ajalugu risk jne. Klientrakenduse saate teha ka tugeva autentimise märgiks taotlemisel.

## <a name="next-steps"></a>Järgmised sammud

- Arendaja ressursid täieliku loendi sh teavet protokollid ja OAuth2 luba anda puhul tugi Azure AD, Soovitage [Azure AD arendaja juhend][AAD-Developers-Guide]
- Vaadake, [Kuidas integreerida taotluse ja Azure AD]  [ ACOM-How-To-Integrate] jaoks põhjalikumat integreerimine taotlemise kohta.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

