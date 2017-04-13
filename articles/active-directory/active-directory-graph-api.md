<properties
   pageTitle="Azure Active Directory Graph API | Microsoft Azure'i"
   description="Ülevaade ja kiirjuhend juhendi Graph API, mis võimaldab Programmeerimisjuurdepääs Azure AD kaudu REST API lõpp-punktid."
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API

> [AZURE.IMPORTANT] Azure'i AD Graph API funktsioonid on saadaval ka läbi [Microsoft Graphi](https://graph.microsoft.io/)ühendatud API, mis sisaldab API-de muude Microsofti teenuste, nt Outlook, OneDrive, OneNote'i, Plaanur ja Office Graphi, puuetega inimestele juurdepääsetavate läbi ühe lõpp-punkti ja ühe juurdepääsu luba.

Azure Active Directory Graph API pakub Programmeerimisjuurdepääs Azure AD kaudu REST API lõpp-punktid. Rakenduste abil saate Graph API loomine, lugemine, värskendamine ja kustutamiseks directory andmete ja objektide (CRUD) toiminguid. Näiteks Graph API toetab järgmisi levinud toiminguid kasutaja objekti.

- Looge uus kasutaja kataloog

- Kasutaja üksikasjalik atribuudid, nt nende rühmade hankimine

- Kasutaja atribuutide, näiteks nende asukoht ja telefoninumber, värskendamine või oma parooli muutmine

- Kasutaja rühmakuuluvus Rollipõhine juurdepääsu kontrollimine

- Kasutajakonto keelamine või kustutama

Lisaks kasutaja objektid, te saate toiminguid sarnaselt muude objektide, näiteks rühmade ja rakenduste. Kataloogi Graph API helistamiseks rakendus peab olema registreeritud Azure AD ja kataloogi juurdepääsu lubamiseks konfigureeritud. See on tavaliselt saavutada kasutaja või administraator nõusolekut kulgemist.

Azure Active Directory Graph API kasutamise alustamiseks [Graph API Kiirjuhend juhendi](active-directory-graph-api-quickstart.md)leiate või Kuva [interaktiivne Graph API dokumentides](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).


## <a name="features"></a>Funktsioonid

Graph API pakub järgmisi funktsioone:

- **Lõpp-punktid REST API**: The Graph API on rahulik teenuse lõpp-punktid, mis on kättesaadav, kasutades HTTP-päringud kuuluvad. Graph API toetab XML-i ja JavaScripti Object märke (JSON) sisutüübid taotlusi ja koosolekutaotluste. Lisateabe saamiseks lugege teemat [Azure AD Graphi REST API viide](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- **Azure AD autentimine**: iga taotluse Graph API peavad olema kinnitatud mõnel on JSON Web Turbeloa (JWT) taotluse päises autoriseerimine. Selle märgiks omandatud paluda Azure AD Turbeloa lõpp-punkti ning esitada sobiv mandaate. Saate kasutada OAuth 2.0 kliendi identimisteabe voogu või kood loa andmine meilivoo omandada märgiks helistamiseks graafik. Lisateavet leiate [Azure'i ad OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).

- **Rollipõhine autoriseerimine (RBAC)**: turberühmad kasutatakse RBAC Graph API. Näiteks, kui soovite määrata, kas kasutajal on juurdepääs teatud ressursi, rakenduse saate helistada [Kontrollida rühma liikmeks (sihiliste)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) toimingut, mis tagastab väärtuse true või false.

- **Vahe päringu**: kui soovite kataloogi vahel kaks ajaperioodide ilma sagedased päringute tegemiseks Graph API muudatusi kontrollida, saate vahe päringu taotluse. Seda tüüpi taotluse tagastab ainult eelmise erinevat päringu taotluse ja praeguse taotluse tehtud muudatused. Lisateabe saamiseks vt [Azure AD graafiku API erinevat päring](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).

- **Directory laiendid**: kui arendate rakendus, mida on vaja lugeda või kirjutada kordumatu directory objekti atribuudid, saate registreerida ja kasutada laiend väärtused Graph API abil. Näiteks kui teie rakendus nõuab Skype'i ID atribuut iga kasutaja jaoks, saate registreerida uue atribuudi kataloogis ja oleks saadaval iga kasutaja objekti. Lisateabe saamiseks lugege teemat [Azure AD Graph API Directory skeemi laiendid](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).

- **Tagatud õiguste otsinguulatuste alusel**: AAD Graph API seab õiguse otsinguulatuste AAD andmete turvaline/andnud juurdepääsu lubamine, mis toetavad erinevaid rakenduse KLIENDITÜÜPIDE, sh:
 - need kasutajaliides, mis on esitatud delegeeritud juurdepääsu andmetele kaudu autoriseerimine (delegeeritud) sisselogitud kasutaja
  - need, mis kasutavad rakenduse-määratleda Rollipõhine juurdepääsu reguleerimine nt teenuse/daemon klientidele (rakenduse rollid)

    Nii delegeeritud ja rakenduse rolli õigusi otsinguulatuste tähistada õigusi, mis on esitatud Graph API ja soovi korral klientrakendused registreerimise õiguste [funktsioonid Azure klassikaline portaali](https://manage.windowsazure.com)kaudu. Saate kliendid anda neile õiguse otsinguulatuste kontrollida, saadud juurdepääsu luba delegeeritud õiguste ulatuse ("scp") nõude kontrollimise käigus ja rakenduse rolli õigusi nõuda rollid ("rollid"). Lugege lisateavet [Azure AD Graph API õiguste otsinguulatuste](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).


## <a name="scenarios"></a>Stsenaariumid

Graph API võimaldab mitmel rakenduste puhul. Järgmistel juhtudel on kõige levinumad:

- **Rakenduse ärivaldkonna (ühe rentnik)**: selle stsenaariumi korral on ettevõtte arendaja töötab ettevõttega, kellel on Office 365 tellimus. Arendaja on hoone suhtleb Azure AD oma tööülesannete täitmiseks veebirakenduse selline määramine litsentsi kasutajale. Selle toimingu jaoks on vaja juurdepääsu Graph API, nii, et arendaja registrid ühe rakenduse Azure AD rentniku ja konfigureerib lugemine ja kirjutamine õiguste Graph API. Seejärel rakendus on konfigureeritud kasutama oma mandaadi või need praegu sisselogimine kasutaja omandada märgiks helistamiseks Graph API.

- **Tarkvara teenuserakenduse (mitme rentniku)**: selle stsenaariumi korral on sõltumatu tarkvara müüja (ISV) on arendada majutatud mitme rentniku veebirakenduse sisaldava kasutaja halduse funktsioonide teiste asutuste, mis kasutavad Azure AD. Nende funktsioonide kasutamiseks on vaja juurdepääsu kataloogiobjektide ja seega rakendus peab olema Graph API kõne. Arendaja registreerib rakenduse Azure AD konfigureerib selle nõua lugemis- ja kirjutamisõigused Graph API ja seejärel võimaldab välise juurdepääsu nii, et teiste asutuste nõusoleku saate kasutada rakendust oma kataloogi. Kui kasutaja teise ettevõtte autendib rakendusse esimest korda, kuvatakse need nõusolekut dialoogiboksi rakenduse taotleb õigustega.  Andmise nõusolekut seejärel annab rakenduse need, mida nõutakse Graph API Directorys kasutaja õigused. Nõusolekut raamistiku kohta lisateabe saamiseks vt [Ülevaade nõusoleku raames](active-directory-integrating-applications.md).

## <a name="see-also"></a>Vt ka

[Azure'i AD Graph API Kiirjuhend juhend](active-directory-graph-api-quickstart.md)

[AD Graphi ülejäänud dokumentatsioon](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Azure Active Directory arendaja juhend](active-directory-developers-guide.md)
