<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine - määratleda directory sünkroonimise nõuetele | Microsoft Azure'i"
    description="Kindlaks teha, millised nõuded on vaja sünkroonimise kõigi kasutajate vahel sisse = tööruumide ja ettevõtte jaoks."
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

# <a name="determine-directory-synchronization-requirements"></a>Directory sünkroonimise nõuete määratlemine
Sünkroonimine on pakkuda kasutajatele identiteedi vastavalt oma kohapealse identiteedi pilveteenuses. Kas nad kasutavad sünkroonitud konto autentimine või väline autentimist, kasutajad vajavad veel on identiteedi pilveteenuses.  Selle identiteedi tuleb säilitada ja perioodiliselt värskendatud.  Värskenduste võib kuluda palju vorme, tiitli muutmise parooli muuta.  

Alustuseks hindamisel ettevõtted kohapealse identiteedi lahenduse ja kasutajale nõuetele. Hindamisel on oluline määratleda tehnilised nõuded kuidas kasutaja identiteedid loodud ja säilitada pilveteenuses.  Enamiku organisatsioonide jaoks Active Directory on kohapealse ja kohapealse kataloogi, mida kasutajad saavad, sünkroonitakse kaudu, kuid mõnel juhul see ei ole täheregistrit.  

Veenduge, et vastavad järgmistele küsimustele.


- Kas teil on ühe AD mets, mitme või mitte keegi?
 - Mitu Azure AD kataloogide on teil olla sünkroonita?
 
    1. Kas kasutate filtreerimine?
    2. Kas teil on mitme Azure'i AD-ühenduse serveriga plaanitud?
  
- Kas teil on praegu sünkroonimise tööriist kohapealse?
  - Kui jah, ei kasutajaid, kui kasutajal on virtuaalse directory/integreerimine identiteedid?
- Kas teil on mõni muu kataloogi asutusesiseselt, mida soovite sünkroonida (nt Lightweight Directory HR andmebaasi jne)?
  - Kas kavatsete mis tahes GALSync tegema?
  - Mis on ettevõtte UPN-ID praegune olek? 
  - Kas teil on erinevate kasutajate autentimiseks kataloogi?
  - Kas teie ettevõte kasutab Microsoft Exchange'i?
    - Kas need, millel on Exchange'i hübriidjuurutuse kavandamine?

Nüüd, kui teil on oma sünkroonimise nõuete kohta aimu, peate kindlaks teha, millised tööriista on õige vastama järgmistele nõuetele.  Microsoft osutab mitu tööriista tegemise kataloogi integreerimise ja sünkroonimise.  Vt lisateavet [hübriid identiteedi kataloogi integreerimise tööriistad võrdlustabelit](active-directory-hybrid-identity-design-considerations-tools-comparison.md) . 
   
Nüüd, kui teil on oma sünkroonimise nõuded ja tööriist, mida selle saavutamiseks oma ettevõtte jaoks, tuleb teil hinnata nende kataloogiteenuste kasutavad rakendused. Hindamisel on oluline määratleda tehnilised nõuded integreerida need rakendused pilve. Veenduge, et vastavad järgmistele küsimustele.

- Need rakendused viiakse pilveteenusesse ja kasutada kataloogi?
- Kas sünkroonida pilveteenusesse, et need rakendused neid kasutada edukalt teisiti atribuute?
- Need rakendused on vaja uuesti kirjutamata cloud auth ära?
- Nende rakenduste jätkuvalt live kohapealse ajal kasutajad neid kasutades cloud identiteedi?

Samuti peate määratlemiseks Kataloogisünkroonimise turvalisus nõuded ja piirangud. Hindamisel on oluline nõuded, mida on vaja selleks, et saate luua ja hallata kasutaja identiteedid pilveteenuses loendit. Veenduge, et vastavad järgmistele küsimustele.

- Kui sünkroonimine serveriga paigutatakse?
- Kas see on ühendatud domeeni?
- Server asub tulemüüriga, nagu on DMZ piiratud võrgus?
  - Mida saab avamiseks nõutav tulemüüri portide toetamiseks sünkroonimine?
- Kas olete katastroofi taastamise kava sünkroonimine serveriga?
- Kas teil on konto kõik metsad vajalikke õigusi soovite sünkroonige koos?
 - Kui teie ettevõte ei tea käesoleva teema küsimusele vastus, vaadake jaotist "Õiguste parooli sünkroonimise" [installida Azure Active Directory sünkroonimise teenuse](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) artiklist ja määratleda, kas teil on juba konto järgmisi õigusi, või kui teil on vaja luua.
- Kui teil on mutli – mets sünkroonimine sünkroonimine serveriga iga mets juurde?
 
>[AZURE.NOTE]
Veenduge, et iga vastus märkmete tegemine ja vastus vaja mõista. [Määratlemine langeva vastuse nõuded](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) lähevad üle saadaolevad suvandid. Millel vastused neile küsimustele valite, millise variandi parima sobib teie ettevõtte vajab.

## <a name="next-steps"></a>Järgmised sammud
[Mitmikautentimise nõuete määratlemine](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)
