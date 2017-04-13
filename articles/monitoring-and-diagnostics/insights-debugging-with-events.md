<properties
    pageTitle="Sündmuste vaatamine ja audit logid"
    description="Saate teada, kuidas näha kõiki sündmusi, mis juhtub teie Azure'i tellimus."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/28/2015"
    ms.author="robb"/>

# <a name="view-events-and-audit-logs"></a>Sündmuste vaatamine ja auditi logid

Kõigi toimingutest Azure ressursid on täielikult auditeerida, Azure'i ressursihaldur, alates loomine ja kustutamised andmise või tühistamise juurdepääsu. Saate sirvida need logid Azure portaali ja programmiliselt juurde pääseda sündmuste täiskomplekti saate kasutada ka [REST API -ga](https://msdn.microsoft.com/library/azure/dn931927.aspx) või [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) .

## <a name="browse-the-events-impacting-your-azure-subscription"></a>Sirvige mõjutavad Azure tellimuse sündmused

1. [Azure'i portaali](https://portal.azure.com/)sisse logima.
2. Klõpsake **Sirvi** ja valige **auditilogid**.  
    ![Liikuge jaoturi](./media/insights-debugging-with-events/Insights_Browse.png)
3. See näitab, et kõik sündmused, mis on mis tahes tellimuste mõjutab viimase 7 päeva tera avab. Ülaosas on skeem, mis näitab andmete taseme ja selle all on täielik logid:  ![kõik sündmused](./media/insights-debugging-with-events/Insights_AllEvents.png)

>[AZURE.NOTE] Saate vaadata ainult 500 viimase sündmuste antud tellimuse Azure'i portaalis.

4. Võite klõpsata mis tahes Logi kirje kuvamiseks sündmusi, et teha see üles. Näiteks kui juurutate midagi ressursirühma, palju erinevaid ressursse võib loodud või muudetud. Iga kirje puhul saate vaadata:
    * **Tase** sündmus – näiteks see võiks olla midagi (**teatised**) jälitamiseks, või kui midagi on valesti, peate teadma (**tõrge**) läinud.
    * **Olek** – lõplik olek on üldiselt **õnnestus** või **nurjus**, kuid võib olla ka **aktsepteeritud** puhul toiminguid.
    * *Kui* sündmus ilmnes.
    * *Kes* sooritatud toimingu, kui keegi. Kõik tegevustega kasutajad, mõned teostamise kirjutamata teenused nii, et need ei oleks **helistaja**.
    * **Korrelatsiooni ID** sündmuse – see on nende toimingute jaoks ainuidentifikaator.

5. Seal saate minna tera üksikasjade kuvamiseks sündmuse üksikasjad.

    ![Ressursi rühmad](./media/insights-debugging-with-events/Insights_EventDetails.png)

    **Nurjunud** sündmused, sisaldab selle lehe tavaliselt on **Substatus** ja silumine eesmärgil vajalikud andmed sisaldavad jaotist **Atribuudid** .

## <a name="filter-to-specific-logs"></a>Filtreerida teatud logid

Selleks, et näha sündmused, mis rakenduvad konkreetse üksuse või kindlat tüüpi, saate filtreerida auditilogi logid tera, klõpsates käsku **Filtreeri** . Filtri tera kasutada ka muuta selle **aja** tiibmutri auditilogi logid.

Kui klõpsate selle käsu, avab uue blade:

![Filtreerimine](./media/insights-debugging-with-events/Insights_EventFilter.png)

On nelja tüüpi filtrid.

1. Märkimise teel
2. **Ressursirühm**
3. **Ressursi tüüp**
4. Kindla **Ressursi** – nii peate viimase olete huvitatud ressursi täielik *Ressursi ID -d*

Lisaks saate filtreerida ka sündmusi, kes läbi sündmus või sündmuse tase.

Kui olete lõpetanud valimise, mida soovite näha, tera allosas nuppu **Värskenda** .

## <a name="monitor-events-impacting-specific-resources"></a>Kuvari sündmused, mis mõjutavad teatud ressursid

1. **Sirvige** olete huvitatud ressursi otsimiseks klõpsake nuppu. Saate vaadata kõik logid terve **Ressursirühma**.
2. Ressursi tera, liikuge kerides allapoole, kuni kuvatakse paani **sündmused** .  
    ![Sündmuste paan](./media/insights-debugging-with-events/Insights_EventsTile.png)
3. Klõpsake selle kuvamiseks filtreeritud, et teie valitud ressursi sündmuste paani. Saate käsk **Filtreeri** ajavahemiku muutmine või rakendamiseks Täpsemad filtrid.

## <a name="next-steps"></a>Järgmised sammud

* Iga kord, kui sündmus juhtub [teatiste vastuvõtmine](insights-receive-alert-notifications.md) .
* [Kuvari teenuse mõõdikute](insights-how-to-customize-monitoring.md) veendumaks, et teie teenus on saadaval ja reageeri.
* [Jälita teenuse seisund](insights-service-health.md) uurida, millal on Azure kogenud jõudluse degradeerumine või teenuse segadusi.  
