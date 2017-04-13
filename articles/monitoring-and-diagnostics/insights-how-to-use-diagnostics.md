<properties
    pageTitle="Luba jälgimine ja diagnostika Microsoft Azure | Microsoft Azure'i "
    description="Saate teada, kuidas häälestada oma ressursid Azure diagnostika."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="enable-monitoring-and-diagnostics"></a>Diagnostika- ja lubamine

[Azure'i portaalis](https://portal.azure.com)saate konfigureerida rikkalike, sagedased, jälgimine ja diagnostika andmete kohta teie ressursse. Saate konfigureerida diagnostika programmiliselt ka [REST API -ga](https://msdn.microsoft.com/library/azure/dn931932.aspx) või [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) .

Diagnostika-ja jälgimine ja mõõdiku andmed Azure salvestatakse teie valitud salvestusruumi kontole. See võimaldab teil kasutada mis tahes instrumentaarium, mida soovite lugeda andmed, salvestusruumi Explorerist kolmanda osapoole instrumentaarium Power BI.

## <a name="when-you-create-a-resource"></a>Kui loote ressursi

Enamiku teenuste võimaldavad lubamine diagnostika loomisel neid [Azure portaali](https://portal.azure.com).

1. Avage **Uus** ja valige olete huvitatud ressursi.

2. Valige **Valikuline konfigureerimine**.
    ![Diagnostika blade](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. Valige **diagnostika**ja **klõpsake**nuppu. Peate salvestusruumi konto, mida soovite salvestada, et diagnostika. Teile arve tavaline andmete määr salvestusruumi ja tehingud diagnostika saatmisel salvestusruumi konto.

4. Klõpsake nuppu **OK** ja looge ressurss.

## <a name="change-settings-for-an-existing-resource"></a>Olemasoleva ressurss sätete muutmine

Kui olete juba loonud ressursi ja soovite muuta diagnostika sätete (et muuta andmete kogumine, näiteks), mida saab teha õiguse Azure'i portaalis.

1. Minge ressurss ja klõpsake käsku **sätted** .

2. Valige **diagnostika**.

3. **Diagnostika** tera on kõigi võimalike diagnostika- ja selle ressursi saidikogumi andmetele. Mõned ressursid saate valida ka **nende andmete puhastamiseks salvestusruumi kontolt säilituspoliitika** .
    ![Salvestusruumi diagnostika](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. Kui olete valinud oma sätted, klõpsake käsku **Salvesta** . See võib võtta pisut ajal andmete kuvamiseks, kui on selle lubamine esimest korda.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Kategooriate virtuaalmasinates andmete kogumine
Jaoks virtuaalmasinates salvestatakse-minutilise intervalliga, et teil on alati kõige ajakohasemat teavet arvuti kohta kõik mõõdikute ja logid.

- **Tavaline mõõdikute** : seisundi mõõdikute virtual arvuti protsessori ja mälu nt kohta
- **Võrgu ja web mõõdikute** : mõõdikute teie võrguühenduste ja web services kohta
- **.NET mõõdikute** : mõõdikute töötavate virtual arvuti .net-i ja ASP.net-i rakenduste kohta
- **SQL-i mõõdikute** : kui teil on selle jõudluse mõõdikute Microsoft SQL-i teenus
- **Windowsi rakenduse sündmuselogide** : Windows sündmused, mis saadetakse rakenduse kanali
- **Windows system sündmuselogide** : Windowsi sündmuste, mis saadetakse süsteemi kanali. See hõlmab ka [Microsofti ründevaratõrje](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409)kõik sündmused.
- **Windowsi sündmuselogide turvalisus** : Windows sündmused, mis saadetakse turvalisuse kanal
- **Diagnostika taristu logid** : logimise kohta diagnostika saidikogumi taristu
- **IIS-i logid** : logid oma IIS-i serveri kohta

Pange tähele, et sel ajal teatud jaotamine Linux ei toetata, külaline Agent peab olema installitud virtuaalse masina.

## <a name="next-steps"></a>Järgmised sammud

* Iga kord, kui funktsionaalseid sündmuste [teatiste vastuvõtmine](insights-receive-alert-notifications.md) või mõõdikute risti läve.
* [Kuvari teenuse mõõdikute](insights-how-to-customize-monitoring.md) veendumaks, et teie teenus on saadaval ja reageeri.
* Veendumaks, et teie teenuse skaala põhjal nõudmisel [mastaapimiseks arvu automaatselt](insights-how-to-scale.md) .
* Kui soovite mõista täpselt kuidas oma koodi läbimiseks pilveteenuses [kuvari rakenduse jõudlus](../application-insights/app-insights-azure-web-apps.md) .
* [Sündmuste vaatamine ja auditi logid](insights-debugging-with-events.md) – siit leiate kõik, mis on juhtunud teenust.
* Millal Azure on kogenud jõudluse degradeerumine või teenuse segadusi [jälituse teenuse seisund](insights-service-health.md) .
