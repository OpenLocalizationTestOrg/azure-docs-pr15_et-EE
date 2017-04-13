
<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine - määratleda juurdepääsu kontrollimise nõuete | Microsoft Azure'i"
    description="Hõlmab alustalasid identiteedi ja tuvastamise Accessi nõuded ressursside hübriidkeskkonnas kasutajate jaoks."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Juurdepääsu juhtimine nõuded lahendusse hübriid identiteedi määratlemine
Ettevõtte kujundamisel nende hübriid identiteedi lahendus saab kasutada ka seda võimalust üle vaadata Accessi nõuded ressursse, mis need kavatsete kasutajatele kättesaadavaks teha. Juurdepääs andmetele cross kõik neli sammast identiteedi, mis on:

- Haldus
- Autentimine
- Luba
- Auditi

Jaotised järgneva hõlmab autentimise ja luba lähemalt, haldus ja auditeerimine kuuluvad hübriid identiteedi elutsükli. Lugege lisateavet nende võimaluste [määratlemine hübriid identiteedi haldamisega seotud toiminguid](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) .

>[AZURE.NOTE]
Lugeda Lisateavet need sambad igat [The neli sammast isikut - identiteedi haldus vanus, hübriid IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) .

## <a name="authentication-and-authorization"></a>Autentimise ja luba
Stsenaariumide autentimise ja luba, on need stsenaariumid teatud nõuetele, mis peab vastama hü identiteedi lahenduse, mis ettevõtte saab vastu võtta. Stsenaariumid, mille Business Business (B2B) side saate lisada mõne eest ülesanne seda administraatorid kuna peavad nad ise tagada, et organisatsiooni autentimise ja luba meetodit saate suhelda äripartnerite. Nõuded autentimise ja luba käigus kujundamisel tagama järgmised küsimustele:

- Ettevõtte autentimiseks ja lubada ainult need kasutajad saavad identiteedihaldussüsteemi asub?
 - Kas on kavas B2B stsenaariumid?
 - Kui jah, kas teate milliseid protokolle (SAML OAuthi, Kerberos, sõned või serdid) kasutatakse nii ettevõtted ühendada?
- Kas hübriid identiteedi lahenduse, mida te ei kavatse vastu tugi nende?

Oluline silmas pidada on kuhu paigutatakse autentimise hoidlas, mis kasutab kasutajate ja partnerite ja haldus mudeli kasutada. Kaaluge võimalust kaks põhilist järgmistest suvanditest:
- Tsentraliseeritud: mudelis kasutaja mandaat, poliitika ja haldus võib olla tsentraliseeritud kohapealse või pilveteenuses.
- Hübriid: mudelis kasutaja mandaat, poliitika ja haldus saab tsentraliseeritud kohapealse ja on tiražeeritud pilveteenuses.

Milline mudel ettevõtte võtab muutuvad vastavalt oma ettevõtte nõuetele, soovite tuvastada, kus on identiteedihaldussüsteemi hakkavad asuma ja haldus režiimis kasutamiseks järgmised küsimused:

- Kas ettevõtte praegu on mõni identiteetide haldus kohapealse?
 - Kui jah, kas nad kavas alles jätta?
 - On ka, et teie asutus järgima selle dikteerib kus soovitud identiteedihaldussüsteemi peaks elavad määrust või vastavuse nõuded?
- Teie asutus kasutab ühekordse sisselogimise kohapealse rakendused asuvad või pilves?
 - Kui jah, hübriid identiteedi mudeli vastuvõtmine mõjutab selle protsessi?

## <a name="access-control"></a>Juurdepääsu reguleerimine
Kuigi autentimise ja luba on ettevõtte andmete valideerimine kasutaja kaudu juurdepääsu lubamiseks core elemente, on ka oluline juhtida seda taset, nendel kasutajatel on juurdepääs administraatorid tase on üle ressursse, mis on need haldamine Accessi. Teie hübriid identiteedi lahendus peate saama Varundustöö juurdepääsu ressursid, delegeerimine ja rolli base juurdepääsu reguleerimine. Veenduge, et juurdepääsu reguleerimine kohta järgmised küsimusele:

- Kas teie ettevõttel on mitu administraatoriõigustes õiguste haldamiseks oma identiteedi süsteem kasutaja?
 - Kui jah, iga kasutaja vajab sama juurdepääsutase?
- Oleks vaja oma ettevõtte volitatud esindaja juurdepääsu kasutajatel hallata teatud ressursse?
 - Kui jah, kui sageli see juhtub?
- Ettevõtte oleks vaja integreerida juurdepääsu juhtimine võimaluste kohapealse ja pilveteenuse vahel ressursse?
- Oleks vaja oma ettevõtte ressursse teatud tingimustel juurdepääsu piirata?
- Teie ettevõttel on mis tahes rakenduses, mis tuleb kohandatud juhtelemendi juurdepääsu mõned ressursid?
 - Kui jah, kui need rakendused asuvad (kohapealse või pilves)?
 - Kui jah, kus on need suunata ressursse, mis asub (kohapealse või pilves)?

>[AZURE.NOTE]
Veenduge, et iga vastus märkmete tegemine ja vastus vaja mõista. [Andmete kaitsmise strateegia määratlemine](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) läheb üle saadaolevad suvandid ja kummagi variandi eelised ja puudused.  Nende küsimustele, saate valida, millise variandi parima sobib teie ettevõtte vajadustele.

## <a name="next-steps"></a>Järgmised sammud

[Langeva vastuse nõuete määratlemine](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)
