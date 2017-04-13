<properties
    pageTitle="Log Analytics juurdepääsu haldamine | Microsoft Azure'i"
    description="Saate hallata juurdepääsu Log Analytics haldustoiminguid erinevaid kasutamine kasutajate, kontod, OMS tööruumide ja Azure kontod."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>

# <a name="manage-access-to-log-analytics"></a>Log Analytics juurdepääsu haldamine

Log Analytics juurdepääsu haldamiseks, saate kasutada mitmesuguseid haldustoiminguid kasutajate, kontod, OMS tööruumide ja Azure kontod. Uue tööruumi loomine rakenduses toimingud halduse komplekti (OMS), valige tööruumi nimi, seostada teie konto ja geograafilise asukoha valimine. Tööruum on põhiosas container, mis sisaldab kontoteave ja lihtne konfiguratsioon konto teave. Teie või teie organisatsiooni teiste liikmetega võite kasutada mitut OMS tööruumid erinevaid andmeid, mis on kogutud kõigi või osade oma IT-taristu haldamise.

[Alustamine Log Analytics](log-analytics-get-started.md) artiklis kirjeldatakse, kuidas kiiresti leida ja töötab ja ülejäänud selles artiklis kirjeldatakse üksikasjalikumalt mõned toimingud, mida peate OMS juurdepääsu haldamiseks.

Kuigi ei peate esmalt iga halduse toimingu sooritamiseks, käsitleme levinumaid ülesanded, mida võib kasutada järgnevalt:

- Tööruumide vajaliku arvu
- Kontod ja kasutajate haldamine
- Mõne olemasoleva tööruumi rühma lisamine
- Azure'i tellimuse mõne olemasoleva tööruumi linkimine
- Tööruumi versioonile tasulisele andmete loetelu
- Lepingu andmetüübi muutmine
- Azure Active Directory ettevõtte lisamiseks mõne olemasoleva tööruumi
- Sulgege OMS tööruumis

## <a name="determine-the-number-of-workspaces-you-need"></a>Tööruumide vajaliku arvu

Tööruum on Azure ressurss ja on container, kus andmed on kogutud, kokkuvõtliku, analüüsida ja OMS portaalis esitatud.

On võimalik mitme OMS Log Analytics tööruumide ja kasutajatel on juurdepääs ühe või mitme tööruumid. Üldiselt soovite vähendada tööruumide arv, kui see võimaldab teil päringu ja oleksid kogu kõige rohkem andmeid. Selles jaotises kirjeldatakse seda, kui see võib olla kasulik rohkem kui üks tööruumi loomine.

Täna, pakub Log Analytics tööruumi.

- Andmete talletamine geograafiline asukoht
- Granulaarsus arveldamine
- Andmete eraldi

Ülaltoodud omaduste alusel, võite luua mitu töölauda, kui:

- Teil on globaalne ettevõte ja peate piirkonnas suveräänsuse või vastavuse andmed põhjustel talletatud andmed.
- Kasutate Azure ja Väljamineva meili andmesidetasude vältimiseks Log Analytics tööruumi hallatavate Azure vahendite samasse piirkonda, millel soovite.
- Soovite jaotada erinevate osakondade või ettevõtte rühmad vastavalt nende kasutamist. Iga osakonna või ettevõtte jaoks tööruumi loomisel kuvatakse Azure arve ja kasutamine lause kulude iga tööruumi eraldi.
- Teil on hallatavate teenusepakkuja ja tuleb säilitada log analytics andmete iga kliendi haldate eraldatud teiste klientide andmeid.
- Saate hallata mitut klientide ja soovite, et iga kliendi või osakonna või business rühma vaadata oma andmeid, kuid mitte andmed klientide või osakondade või business rühmi.

Andmete kogumine agentide kasutamisel saate konfigureerida iga agent nõutav tööruumi teatada.

Kui kasutate süsteemi keskmist toiminguhalduri, saate iga rühma Toiminguhalduri seotud ainult ühte tööruumi. Saate installida Microsoft Agenti jälgimine haldab Toiminguhalduri arvutisse ja on nii Toiminguhalduri kui ka mõnda teise tööruumi Log Analytics agent aruande.

### <a name="workspace-information"></a>Tööruumi teabe

OMS portaalis, saate vaadata oma tööruumi teabe ja valige, kas soovite, et saada teavet Microsofti.

#### <a name="view-workspace-information"></a>Tööruumi teabe vaatamine

1. OMS, klõpsake paani **sätted** .
2. Klõpsake vahekaarti **kontod** .
3. Klõpsake vahekaarti **Tööruumi teabe** .  
  ![Tööruumi teabe](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Kontod ja kasutajate haldamine

Iga tööruumi võib olla seotud mitu kasutajakontot ja mitme OMS tööruumide juurdepääsu igale kasutajakontole (Microsofti või organisatsiooni konto).

Vaikimisi on Microsofti või organisatsiooni konto tööruumi loomiseks kasutatud muutub tööruumi administraator. Administraator saab seejärel kutsuda Microsofti konto või valige kasutajad Azure Active Directory kaudu.

Annab inimestele juurdepääsu OMS tööruumi kontrollida 2 kohas:

- Azure'i, saate Rollipõhine juurdepääsu reguleerimine juurdepääsu Azure tellimuse ja seotud Azure ressursid. Seda kasutatakse PowerShelli ja REST API juurdepääsu.
- OMS portaalis juurdepääs ainult OMS portaali - pole seotud Azure'i tellimus.

Kui inimesed juurdepääsu andmiseks OMS portaali, kuid mitte Azure'i tellimus, mis on seotud seejärel automatiseerimine, varundamine ja taastamine saidi lahenduse paanid ei kuvata andmeid kasutajatele kui nad sisselogimist OMS portaali.

Kõigil kasutajatel näha järgmisi lahendusi andmed, veenduge, et need on vähemalt juurde **lugeja** jaoks soovitud automatiseerimise konto, varundamise Vault ja saidi taastamise hoidla, mis on seotud OMS tööruumi.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Log Analytics Azure portaali kaudu juurdepääsu haldamine

Kui inimesed juurdepääsu andmiseks Log Analytics tööruumi kasutamise Azure'i õiguste Azure'i portaalis näiteks siis sama kasutajad portaali saab Log Analytics. Kui kasutajad on Azure portaali, nad liikuge OMS portaali, klõpsates **OMS portaali** tööülesande Log Analytics tööruumi ressursi kuvamisel.

Mõned punkte meeles pidama seoses Azure portaali.

- See pole *Rollipõhine juurdepääsu reguleerimine*. Kui teil on *lugemise* juurdepääsuõigusi Azure'i portaalis Log Analytics tööruumi, siis saate muuta OMS portaalis. OMS portaalis on mõiste administraator, kaasautor ja kasutajale kirjutuskaitstud. Kui olete sisse loginud sisse kontoga on seotud tööruumi Azure Active Directory OMS portaalis administraator saab, vastasel juhul kaasautori.

- Kui te sisselogimist OMS portaali kaudu http://mms.microsoft.com, siis vaikimisi kuvatakse loendis **Valige tööruumi** . See sisaldab ainult tööruumid, mis on lisatud OMS portaali kaudu. Tööruumide kuvamiseks teil on juurdepääs Azure'i tellimused, siis peate määrama rentniku jaoks URL-i osana. Näiteks:

  `mms.microsoft.com/?tenant=contoso.com`Rentniku identifikaator on sageli selle Viimane osa e-posti aadress, millele te logige sisse.

- Kui te sisselogimine ja konto on konto rentniku Azure Active Directory, mis on tavaliselt juhul, kui olete sisselogimise-kui ka CSP, siis saab *administraator* OMS portaalis. Kui teie konto pole rentniku Azure Active Directory, siis saab *kasutaja* OMS portaalis.

- Kui soovite liikuda portaali, et teil on juurdepääs Azure'i õiguste abil, siis peate määramiseks ressursi URL-i osana. On võimalik, et seda URL-i PowerShelli kaudu.

  Näiteks `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  URL-i näeb:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### <a name="managing-users-in-the-oms-portal"></a>OMS portaali kasutajate haldamine

Saate hallata kasutajate ja **Kasutajate haldamine** vahekaardi sätted lehel vahekaardil **kontod** . Seal saate teha järgmistes jaotistes tööülesanded.  

![kasutajate haldamine](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Mõne olemasoleva tööruumi kasutaja lisamine

Järgmiste juhiste abil saate lisada kasutaja või rühma OMS tööruumi. Kasutajal või rühmal on võimalik vaadata ja kõik teatised, mis on seotud selle tööruumi toimivad.

>[AZURE.NOTE] Kui soovite lisada kasutaja või rühma Azure Active Directory ettevõtte konto, peate esmalt veenduma, et teil on seotud konto OMS Active Directory domeeni. Lugege teemat [Azure Active Directory ettevõtte olemasoleva tööruumi lisamine](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. OMS, klõpsake paani **sätted** .
2. Klõpsake vahekaarti **kontod** ja seejärel klõpsake vahekaarti **Kasutajate haldamine** .
3. Valige jaotises **Hallata kasutajate** lisamiseks soovitud kontotüüp: **Organisatsioonikonto**, **Microsofti konto**, **Microsofti tugiteenuste**.
    - Kui valite suvandi Microsoft Account, Tippige meiliaadress, Microsoft Account kasutaja.
    - Kui valite organisatsioonikonto, saate sisestada osa kasutaja või rühma nimi või meilipseudonüüm ja kuvatakse loendi kasutajad ja rühmad. Valige kasutaja või rühma.
    - Microsoft Support abil anda Microsoft Support insener ajutise juurdepääsu oma tööruumi tõrkeotsingu.

    >[AZURE.NOTE] Jõudluse parimate tulemuste saamiseks Active Directory rühmad, mis on seotud ühe OMS konto kolm arvu piiramine – üks administraatorid, üks kaasautorid ja üks kirjutuskaitstud kasutajate jaoks. Abil veel rühmi võib mõjutada Log Analytics jõudlus.

5. Valige kasutaja või rühma lisamine tüüp: **administraator**, **kaasautor**või **Kirjutuskaitstud kasutaja** .  
6. Klõpsake nuppu **Lisa**.

  Microsofti konto lisamisel tööruumi kutse saadetakse teile e-posti. Pärast kasutaja järgib OMS liitumise kutse juhiseid, kasutaja saab vaadata, teatiste ja kontoteavet OMS konto ja siis saab kasutaja teabe vaatamiseks lehel **sätted** vahekaardil **kontod** .
  Ettevõtte konto lisamisel kasutaja saab juurdepääsu Log Analytics kohe.  
  ![kutse e-posti](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Mõne olemasoleva kasutaja type redigeerimine

Saate muuta OMS kontoga seostatud kasutaja konto roll. Teil on rolli järgmistest suvanditest:

 - *Administraator*: saate kasutajate haldamine, vaadata ja kõik teatised, toimivad ja lisamine ja eemaldamine serverid

 - *Kaasautor*: saate vaadata ja kõik teatised, toimivad ja lisamine ja eemaldamine serverid

 - *Kirjutuskaitstud kasutaja*: märgitud kirjutuskaitstuks kasutajad ei saa abil:
   1. Lisa/eemalda lahendusi. Lahendusegalerii on peidetud.
   2. **Armatuurlaua minu**paanide lisamine või muutmine ja eemaldamine.
   3. **Üldsätete** lehtede vaatamine Lehed on peidetud.
   4. Otsingu vaade, PowerBI konfiguratsiooni, salvestatud otsingute ja teatiste tööülesanded on peidetud.


#### <a name="to-edit-an-account"></a>Kui soovite konto redigeerimine

1. OMS, klõpsake paani **sätted** .
2. Klõpsake vahekaarti **kontod** ja seejärel klõpsake vahekaarti **Kasutajate haldamine** .
3. Valige roll, kasutaja, mida soovite muuta.
2. Klõpsake kinnitamise dialoogiboksis nuppu **Jah**.

### <a name="remove-a-user-from-a-oms-workspace"></a>Tööruumi OMS Kasutaja eemaldamine

Järgmiste juhiste abil saate mõne OMS tööruumi Kasutaja eemaldamine. Pange tähele, et see ei sulgu kasutaja tööruumi. Selle asemel see eemaldab kasutaja ja tööruumi vahel seose. Kui kasutaja on seotud mitu tööruumid, kasutaja siiski saab OMS sisse logida ja muude tööruumide vaadata.

1. OMS, klõpsake paani **sätted** .
2. Klõpsake vahekaarti **kontod** ja seejärel klõpsake vahekaarti **Kasutajate haldamine** .
3. Klõpsake kasutaja nimi, mida soovite eemaldada kõrval käsku **Eemalda** .
4. Klõpsake kinnitamise dialoogiboksis nuppu **Jah**.


### <a name="add-a-group-to-an-existing-workspace"></a>Mõne olemasoleva tööruumi rühma lisamine

1.  Järgige juhiseid 1 – 4 sisse "Lisage kasutaja olemasoleva tööruumi", ülaltoodud.
2.  Klõpsake jaotises **Valige kasutaja/rühma**, valige **rühm**.
    ![mõne olemasoleva tööruumi rühma lisamine](./media/log-analytics-manage-access/add-group.png)
3.  Sisestage jaotises, mida soovite lisada kuvatav nimi või meiliaadress.
4.  Valige jaotise tulemite loend ja seejärel klõpsake nuppu **Lisa**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Azure'i tellimuse mõne olemasoleva tööruumi linkimine

On võimalik luua tööruumi [microsoft.com/oms](https://microsoft.com/oms) veebisaidilt.  Siiski teatud piirangud olemas nende tööruumide kõige märkimisväärne, on limiit 500MB päevas andmete üles, kui kasutate tasuta konto. Selle tööruumi muudatuste tegemiseks peate *oma olemasoleva tööruumi Azure märkimiseks linkida*.

>[AZURE.IMPORTANT] Link: tööruumi, et Azure'i konto peab olema juba juurdepääs tööruum, mida soovite linkida.  Teisisõnu, saate kasutada Azure portaali konto peab olema **sama** konto abil oma OMS tööruumi juurdepääsu. Kui see ei ole, lugege teemat [lisamine kasutaja olemasoleva tööruumi](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Tööruumi linkimiseks Azure tellimuse OMS portaalis

Tööruumi lingi Azure tellimuse OMS portaalis, et juba olema sisse logitud kasutajale tasulisele Azure'i konto. Tööruumi, mida kasutate aktiivselt saab linkida Azure'i konto.

1. OMS, klõpsake paani **sätted** .
2. Klõpsake vahekaarti **kontod** ja seejärel klõpsake vahekaarti **Azure'i tellimus ja andmete kavandamine** .
3. Klõpsake andmete leping, mida soovite kasutada.
4. Klõpsake nuppu **Salvesta**.  
  ![tellimuse ja andmete lepingud](./media/log-analytics-manage-access/subscription-tab.png)

OMS portaali lindi veebilehe ülaosas kuvatakse teie uute andmete leping.

![OMS lindil](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Tööruumi linkimiseks Azure tellimuse Azure'i portaalis

1.  [Azure'i portaali](http://portal.azure.com)sisse logida.
2.  Liikuge sirvides **Log Analytics (OMS)** ja valige see.
3.  Näete oma olemasoleva tööruumi loendi. Klõpsake nuppu **Lisa**.  
    ![tööruumi loendi](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  Klõpsake jaotises **OMS tööruumi** **lingi olemasoleva**.  
    ![olemasoleva link](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Klõpsake nuppu **Konfigureeri nõutav sätted**.  
    ![nõutav sätete konfigureerimine](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Kui näete loendis tööruumid, mida pole veel seotud Azure'i kontosse. Valige tööruumis.  
    ![Valige tööruumid](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  Vajadusel saate muuta väärtuste järgmised üksused:
    - Tellimuse
    - Ressursirühm
    - Asukoht
    - Esimese taseme hinnad  
        ![väärtuste muutmine](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Klõpsake nuppu **Loo**. Tööruum on nüüd lingitud Azure'i kontosse.

>[AZURE.NOTE] Kui te ei näe tööruum, mida soovite linkida, siis Azure tellimuse ei pääse OMS tööruumi loodud OMS veebisaidi kaudu.  Peate selle konto kaudu sees OMS tööruumi OMS veebisaidi kaudu juurdepääsu. Selleks, lugege teemat [lisamine kasutaja olemasoleva tööruumi](#add-a-user-to-an-existing-workspace).



## <a name="upgrade-a-workspace-to-a-paid-data-plan"></a>Tööruumi versioonile tasulisele andmete loetelu

On kolme tööruumiandmed OMS tüüpi kavandamine: **tasuta**, **Standard**ja **Premium**.  Kui teil on *tasuta* leping, võib tulemus oma andmete ots 500 MB.  Peate uuendada oma tööruumi ***pensionitingimustega leping*** selleks, et koguda andmeid piirmäärast. Igal ajal saate teisendada lepingu tüübist.  OMS hinnad kohta leiate lisateavet teemast [Hinnad üksikasjad](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

>[AZURE.IMPORTANT] Tööruumi lepingute saab muuta ainult siis, kui need on *lingitud* Azure märkimiseks.  Kui lõite tööruumi Azure või kui olete *juba* lingitud tööruumi, saate seda teadet ignoreerida.  Kui olete loonud oma tööruumi [OMS veebisaidile](http://www.microsoft.com/oms), peate juhiste linki [mõne olemasoleva tööruumi Azure märkimiseks](#link-an-existing-workspace-to-an-azure-subscription).

### <a name="using-entitlements-from-the-oms-add-on-for-system-center"></a>Õiguste kaudu OMS lisandmooduli kasutamise System Center

OMS lisandmooduli System Center pakub Premium plaani OMS Log Analytics, mida on kirjeldatud aadressil [OMS hinnad](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx)õigus.

Kui ostate OMS lisandmooduli System Center, lisatakse OMS lisandmooduli teie süsteemi Center lepingu õigus. Azure'i tellimus, mis on loodud käesolevas lepingus saate teha kasutamise õigus. See võimaldab teil, näiteks on mitu OMS tööruumid, mida õigust OMS lisandmooduli kaudu.

Selleks, et mõne OMS tööruumi kasutamist on rakendatud teie õiguste OMS lisandmooduli kaudu, peate:

1. Azure'i tellimus, mis on osa ettevõtteleping, mis sisaldab nii OMS lisandmooduli osta ja Azure tellimuse kasutamine tööruumis OMS linkimine
2. Valige tööruumi Premium kavandamine

Azure'i või OMS portaalis oma kasutuse läbi ei kuvata OMS lisandmooduli õigused. Siiski saate vaadata õiguste ettevõtteportaali.  

Kui teil on vaja muuta oma OMS tööruumi lingitud Azure tellimuse, saate kasutada Azure PowerShelli [Teisalda-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) cmdlet-käsk.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Azure'i kohustuse Enterprise'i leping abil

Kui otsustate kasutada eraldi hinnad OMS komponentide, maksate OMS iga osa eraldi ja kasutamine on teie arvele Azure.

Kui teil on mõni Azure rahaliste kinnitamine ettevõtte registreerimine, mis on seotud teie Azure'i tellimused, Pangakaart mis tahes kasutamist Log Analytics automaatselt vastu kõik ülejäänud rahaliste kinnitamine.

Kui teil on vaja muuta Azure'i tellimus teile lingitud OMS tööruumi saate kasutada cmdlet Azure PowerShelli [Teisalda-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) .  



### <a name="to-change-a-workspace-to-a-paid-data-plan"></a>Tööruumi lepingu tasulisele andmete muutmiseks

1.  [Azure'i portaali](http://portal.azure.com)sisse logida.
2.  Liikuge sirvides **Log Analytics (OMS)** ja valige see.
3.  Näete oma olemasoleva tööruumi loendi. Valige tööruumis.  
    ![tööruumi loendi](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  Klõpsake jaotises **sätted**käsku **hinnakirjad taseme**.  
    ![esimese taseme hinnad](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  **Hinnakirjad taseme**, klõpsake jaotises Valige andmed leping ja klõpsake nuppu **Valige**.  
    ![Valige leping](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Azure'i portaalis vaate värskendamisel näete **hinnakirjad taseme** värskendatud valitud lepingu raames.  
    ![esimese taseme hinnad värskendamine](./media/log-analytics-manage-access/manage-access-change-plan04.png)

Nüüd saate koguda andmeid lisaks ots "tasuta" andmed.


## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Azure Active Directory ettevõtte lisamiseks mõne olemasoleva tööruumi

Log Analytics (OMS) tööruumi saate seostada Azure Active Directory domeenis. Võimaldab lisada kasutajaid Active Directory otse oma OMS tööruumi ei ole vaja eraldi Microsofti kontoga.

Tööruumi loomine Azure portaali kaudu või tööruumi linkimine Azure tellimuse seotakse oma Azure Active Directory nimega ettevõtte konto.

Portaalist OMS tööruumi loomisel küsitakse Azure tellimuse ja organisatsioonikonto link.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Azure Active Directory ettevõtte lisamiseks mõne olemasoleva tööruumi

1. OMS lehel Sätted käsku **kontod** ja seejärel klõpsake vahekaarti **Tööruumi teabe** .  
2. Ettevõtte kontod teave läbi ja klõpsake **Ettevõtte lisada**.  
    ![ettevõtte lisamine](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Sisestage identiteedi teave Azure Active Directory domeeni administraator. Pärast seda, näete, märkides lingitud tööruumi Azure Active Directory domeeni kinnitus.
    ![lingitud tööruumi kinnitus](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Kui teie konto on lingitud organisatsioonikonto, linkimise ei saa olla eemaldatud või muudetud.

## <a name="close-your-oms-workspace"></a>Sulgege OMS tööruumis

Mõne OMS tööruumi sulgemisel kustutatakse kõik andmed, mis on seotud teie tööruumi OMS teenuse sulgemise tööruumi 30 päeva jooksul.

Kui olete administraator ja on seostatud tööruumi mitu kasutajat, on katkenud nende kasutajate ja tööruumi vahel seose. Kui kasutajad on seostatud muude tööruumid, siis need edasi need teiste tööruumide OMS abil. Kui nad pole seostatud muude tööruumide siis need pead kasutada OMS uue tööruumi loomine.

### <a name="to-close-an-oms-workspace"></a>Mõne OMS tööruumi sulgemiseks

1. OMS, klõpsake paani **sätted** .
2. Klõpsake vahekaarti **kontod** ja seejärel klõpsake vahekaarti **Tööruumi teabe** .
3. Klõpsake nuppu **Sule tööruum**.
4. Valige üks tööruumi sulgemise põhjus või muu põhjus Sisestage tekstiväljale.
5. Klõpsake nuppu **Sule tööruum**.

## <a name="next-steps"></a>Järgmised sammud

- Teemast [ühenduse loomine Windows arvutite Log Analytics](log-analytics-windows-agents.md) lisamine agentide ja andmete kogumine.
- Funktsioonide lisamine ja andmete kogumine [lisamine Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md) .
- Kui teie ettevõte kasutab tulemüüri või puhverserveri nii, et agentide saaksid suhelda Log Analytics teenuse [puhverserveri ja tulemüüri sätete konfigureerimine Log Analytics](log-analytics-proxy-firewall.md) .
