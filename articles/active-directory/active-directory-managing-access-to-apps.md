<properties
  pageTitle="Rakenduste Azure AD kaudu juurdepääsu haldamine |  Microsoft Azure'i"
  description="Kirjeldab, kuidas Azure Active Directory võimaldab asutustel määrata rakendusi, millele igal kasutajal on juurdepääs."
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>Accessi rakenduste haldamine

Alalise juurdepääsu haldamine, kasutus hindamise ja aruandluse jätkuvalt olla keeruline, kui rakendus on integreeritud ettevõtte identiteedi süsteem. Paljudel juhtudel IT-administraatorid või kasutajatugi tegema poolelioleva aktiivselt juurdepääsu oma rakenduste haldamine. Mõnikord on ülesande sooritatud üld- või jagatud IT-meeskonna. Ülesande otsust on sageli ette nähtud business otsust maker oma heakskiitu enne, kui see muudab ülesande delegeerida.  Teiste asutuste investeerida integreerimine olemasoleva automatiseeritud identiteedi ja juurdepääsu juhtimine süsteemi, nt Rollipõhine juurdepääsu reguleerimine (RBAC) või atribuut põhinev juurdepääsu reguleerimine (ABAC). Nii ja reegli tavaliselt eripärase ja kallis. Jälgimine, või kas lähenemisviisi aruandlus on oma eraldi, kulukas ja keerukas investeering.

## <a name="how-does-azure-active-directory-help"></a>Kuidas aitab Azure Active Directory?

 Azure AD toetab olulisel juurdepääsu juhtimine konfigureeritud rakendused, mis võimaldab asutustel hõlpsasti saavutada vahemikus automaatne, atribuut ülesande (ABAC või RBAC stsenaariumid) kaudu delegeerimine ja administraatori halduse sh õige juurdepääsupoliitikaid. Azure AD saate hõlpsalt saavutada keerukate poliitikad, kombineerides mitme halduse mudelite ühe rakenduse ja saate isegi uuesti kasutada halduse reeglid sama sihtrühmade rakenduste üle.

 - [Lisada uusi või olemasolevaid rakendusi](active-directory-sso-integrate-saas-apps.md)


 Azure AD Rakenduse ülesande keskendub esmane ülesande kahel viisil:

- **Üksikute ülesanne** IT-administraator directory üldadministraatori õigustega saate valida kasutajakontode ja anda neile juurdepääsu rakendus.
- **(Makstud ainult Azure AD) rühma-põhiste ülesanne** IT-administraator directory üldadministraatori õigustega saate määrata rakenduse rühma. Kas nad on rühma liikmed need proovivad juurde pääseda rakenduse ajal määratakse kindlatele kasutajatele juurdepääs. Teisisõnu, saate administraator märkides "praegune liikme määratud rühma on juurdepääs rakenduse" ülesande reegli tõhus loomine. Selle ülesande suvandi abil administraatorid saavad kasutada mis tahes Azure AD rühma haldamise võimalusi, sealhulgas [atribuut põhinev dünaamiline rühmad](active-directory-accessmanagement-manage-groups.md), välise süsteemiga rühmade (nt kohapealse Active Directory või Workday) või administraatori hallatud või enda-kasutuselevõtmiseks hallatavate rühmad. Ühte rühma saate hõlpsalt määrata mitme rakendused, tagada ülesande osaleja rakendusi saate ühiskasutusse anda ülesande reeglid haldamisel keerukuse vähendamine. Pange tähele selle pesastatud rühma liikmelisused rühma vastavalt ülesande rakendustele praegu ei toetata.

Kasutades kahte ülesande režiimidest, administraatorid võite saavutada mis tahes soovitav ülesande lähenemisviisi.

Azure AD, kasutus ja ülesande aruandlus on täielikult integreeritud, administraatorid hõlpsasti aruandluseks ülesande oleku, ülesande tõrgete ja isegi kasutuse lubamine.

## <a name="complex-application-assignment-with-azure-ad"></a>Keerukate rakenduse ülesande Azure AD

Kaaluge võimalust Salesforce'i rakendust. Paljud ettevõtted, kasutatakse peamiselt organisatsioonide turundus- ja Salesforce'i. Sageli turundustegevuse meeskonna liikmed on väga eeliseid, Salesforce, samal ajal müügi meeskonna liikmed on piiratud juurdepääs. Paljudel juhtudel Teabetöötajad laialdane rahvaarv on piiratud juurdepääs rakenduse. Erandeid teevad. Sageli on õigust turundus- või liidripositsioon meeskonnad kasutaja juurdepääsu andmiseks või muuta oma rolli sõltumatult üldise reegleid.

Azure AD, võib olla eelnevalt konfigureeritud ühekordse sisselogimise (SSO) ja automatiseeritud ettevalmistamise rakendusi nagu Salesforce'i. Kui rakendus on konfigureeritud, saab administraator võtta ühekordse toimingu loomine ja määrake sobiv rühmad. Selles näites administraator võib täita järgmised ülesanded:

- [Dünaamiliste rühmad](active-directory-accessmanagement-manage-groups.md) saab määratleda automaatselt tähistada kõik rühmaliikmed turundus- ja atribuute, nt osakonna või rolli abil:

    - Kõik turundustegevuse rühma liikmete oleks määratud Salesforce'i "turundus" rolli

    - Rühmade oleks määratud Salesforce'i rolli "müük" müügimeeskond kõikidele liikmetele. Edasine täiustamine kasutada mitut rühmad, mis tähistavad erinevate Salesforce'i rollidele määratud piirkondliku müügi meeskondadel.

- Süsteemi erandi lubamiseks saanud luua iseteenindusliku rühma iga rolli. Näiteks "Salesforce'i turundus erand" rühma loomist iseteenindusliku rühmana. Rühma saab määrata Salesforce'i turundustegevuse roll ja turundusmaterjalide liidripositsioon meeskond saab teha omanikud. Turundusmaterjalide liidripositsioon meeskonna liikmed võivad lisamine või eemaldamine kasutajad, Liitu poliitika, määramine või isegi kinnitamine või keelamiseks üksikud kasutajad taotlusi liituda. See on toetatud teavet töötaja vastav kogemus, mis ei vaja eriotstarbelisi koolitus omanikud või liikmed.

Sel juhul kõik määratud kasutajad soovite automaatselt ette valmistada Salesforce, et nagu need lisatakse eri rühmade Salesforce'i soovite värskendada oma rolli määramine. Kasutajaid oleks võimalik leida ja pääsete Salesforce'i läbi Microsoft rakenduse access paneeli Office web kliendid, või isegi oma ettevõtte Salesforce'i sisselogimislehe liikumine. Administraatorid oleks hõlpsalt vaadata kasutus- ja ülesande oleku Azure AD aruannete abil.

Administraatorid saavad tööle [Azure AD juurdepääsu](active-directory-conditional-access.md) Accessi juurutada poliitikad teatud rollide jaoks. Need poliitikad võivad hõlmata kas väljaspool ettevõttekeskkonnas ja isegi Mitmikautentimise või seadme nõuded juurdepääsu eri juhtudel saavutamiseks on lubatud juurdepääs.

## <a name="how-can-i-get-started"></a>Kuidas saan alustada?

Esiteks, kui te ei kasuta veel Azure AD ja te olete IT-administraator:

 - [Proovige järele!](https://azure.microsoft.com/trial/get-started-active-directory/) – saate registreeruda tasuta prooviversiooni 30 päeva täna ja juurutada oma esimese pilve lahendus all 5 minutit selle lingi kaudu

Azure'i AD-funktsioone, mis konto ühiskasutuse lubamine on järgmised.

- [Rühmakuuluvus](active-directory-accessmanagement-self-service-group-management.md)
- Azure AD rakenduste lisamine
- Ülesande töötamise alustamine
- Rakenduse ülesande FAQ
- [Armatuurlaua/kasutusaruannete rakendus](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Kust saada lisateavet?

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Tingimusvormingu Accessi rakenduste kaitsmine](active-directory-conditional-access.md)
- [Iseteeninduslik rühma haldamine/SSAA](active-directory-accessmanagement-self-service-group-management.md)
