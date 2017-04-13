<properties
    pageTitle="Azure'i klahvi Vault lahenduse Log Analytics | Microsoft Azure'i"
    description="Saate Azure'i klahvi Vault lahendus Log Analytics läbivaatamiseks Azure'i klahvi Vault logid."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="richrund"/>

# <a name="azure-key-vault-preview-solution-in-log-analytics"></a>Azure'i klahvi Vault (eelvaade) lahenduse Log Analytics

>[AZURE.NOTE] See on [Eelvaade lahendus](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Saate Azure'i klahvi Vault lahendus Log Analytics Azure'i klahvi Vault AuditEvent logid üle vaadata.

Azure'i klahvi Vault auditilogi sündmuste logimise lubamine Azure'i bloobimälu, kus ta saab siis indekseerida Log Analytics otsimiseks ja analüüsi kirjutada need logid.

## <a name="install-and-configure-the-solution"></a>Installima ja konfigureerima lahendus

Järgmised juhised abil saate installida ja konfigureerida Azure klahvi Vault lahendus.

1.  [Diagnostika logimise klahvi Vault](../key-vault/key-vault-logging.md) ressursid, millest soovite jälgida lubamine
2.  Log Analytics kirjeldatud [JSON](../log-analytics/log-analytics-azure-storage-json.md)failid järgmises vormingus bloobimälu abil lugemise logid bloobimälu konfigureerimine.
3.  Azure'i klahvi Vault lahendus lubada [lisada Log Analytics](log-analytics-add-solutions.md)Solutions lahendusegaleriist kirjeldatud.  

## <a name="review-azure-key-vault-data-collection-details"></a>Vaadake üle Azure'i klahvi Vault andmete kogumise üksikasjad

Azure'i klahvi Vault lahenduse kogub diagnostika logid Azure'i bloobimälu Azure'i klahvi Vault jaoks.
Pole agent on vaja andmete kogumine.

Järgmises tabelis on andmete kogumise meetodite ja muud üksikasjad kohta, kuidas koguda andmeid Azure'i klahvi Vault.

| Platvorm | Otsest agent | Süsteemide Center toimingute Manager (SCOM) agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | Saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Azure'i|![Ei](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Ei](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Jah](./media/log-analytics-azure-keyvault/oms-bullet-green.png)|            ![Ei](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Ei](./media/log-analytics-azure-keyvault/oms-bullet-red.png)| 10 minuti|

## <a name="use-azure-key-vault"></a>Azure'i võtme Vault kasutamine

Pärast installimist lahenduse, saate vaadata taotluse olekud aja jooksul teie jälgitud klahvi võlvid, kasutades **Azure klahvi Vault** paani Kokkuvõte Log Analytics lehel **Ülevaade** .

![Azure'i klahvi Vault paani pilt](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Kui olete klõpsanud **Ülevaade** , saate vaadata oma logid kokkuvõtted ja seejärel Mine järgmisi üksikasju:

- Kõik võtme vault tehingute aja jooksul
- Aja jooksul toiming mahud nurjus
- Keskmise funktsionaalseid latentsuse alusel
- Arv, mis võtavad rohkem kui 1000 ms ja toiminguid, mis võtavad rohkem kui 1000 ms loendi teenuse kvaliteeti

![Azure'i klahvi Vault armatuurlaua pilt](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Azure'i klahvi Vault armatuurlaua pilt](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Mis tahes toimingu üksikasjade vaatamiseks

1. Klõpsake lehe **Ülevaade** **Azure'i klahvi Vault** paani.
2. Armatuurlaual **Azure'i klahvi Vault** läbi kokkuvõtlik teave ühes labad ja seejärel klõpsake ühte vaadata üksikasjalikku teavet lehel log.

    Mis tahes Logi otsing, saate vaadata tulemusi aeg, üksikasjalikud tulemused ja ajaloo log otsing. Samuti saate filtreerida omadustega tulemeid piiritleda.

## <a name="log-analytics-records"></a>Kasutusanalüüsi kirjed

Azure'i klahvi Vault lahendus analüüsib kirjed, mis on teatud tüüpi **KeyVaults** , mis on kogutud Azure'i diagnostika [AuditEvent logid](../key-vault/key-vault-logging.md) .  Järgmises tabelis on need kirjed atribuudid.  

| Atribuut | Kirjeldus |
|:--|:--|
| Tüüp | *KeyVaults* |
| SourceSystem | *AzureStorage* |
| CallerIpAddress | Taotluse esitanud kliendi IP-aadress |
| Kategooria      | Klahv Vault logide, AuditEvent on üks, mis on saadaval väärtus.|
| CorrelationId | Valikuline GUID, mis oleksid teenuse-side (võtme Vault) logid kliendipoolseid logisid saab edastada kliendi. |
| DurationMs | Teenuse REST API taotluse, millisekundites kulunud aeg. See ei sisalda võrgu latentsus, et aega, mida kliendi poolel mõõta ei kattu seekord. |
| HttpStatusCode_d | HTTP olekukoodi tagastatud taotlus |
| Id_s       | Taotluse Ainuidentifikaator |
| Identity_o | Identiteedi märgiks ei esitatud REST API taotluse tegemisel. See on tavaliselt "kasutaja", "teenuse põhilise" või "kasutaja + appId" kombinatsiooni nagu taotluse tuleneva Azure PowerShelli cmdlet-käsk. |
| OperationName      | Toimingu, nagu dokumenteerida [Azure'i klahvi Vault logimine](../key-vault/key-vault-logging.md) nimi|
| OperationVersion      | Kliendi poolt küsitud REST API versioon|
| RemoteIPLatitude | Laiuskraad kliendi taotluse esitanud |
| RemoteIPLongitude | Kliendi taotluse esitanud pikkuskraad |
| RemoteIPCountry | Taotluse esitanud kliendi riigi  |
| RequestUri_s | Taotluse URI |
| Ressurss   | Võtme vault nimi |
| ResourceGroup | Võtme Vault ressursirühm |
| ResourceIdkasutamisel | Azure'i ressursihaldur ressursi ID-ga. Klahv Vault logid, on alati võti Vault ressursi ID-ga. |
| ResourceProvider | *MICROSOFT. KEYVAULT* |
| ResultSignature  | HTTP olek|
| ResultType      | Tulemi REST API taotluse.|
| SubscriptionId | Azure'i Tellimuse ID, mis sisaldavad klahvi Vault tellimuse |


## <a name="next-steps"></a>Järgmised sammud

- [Log otsingud Log Analytics](log-analytics-log-searches.md) abil Azure'i klahvi Vault üksikasjalike andmete kuvamine.
