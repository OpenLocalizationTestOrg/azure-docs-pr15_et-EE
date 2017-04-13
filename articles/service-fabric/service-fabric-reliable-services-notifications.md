<properties
   pageTitle="Usaldusväärne teenuste teatised | Microsoft Azure'i"
   description="Kontseptuaalne dokumentatsioonist teenuse struktuuri usaldusväärne teenuste teatised"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Usaldusväärne teenuste teatised

Teatiste võimaldavad klientidel objekti, mida nad huvi tehtud muutuste jälitus. Kahte tüüpi objektid toetavad teatised: *Usaldusväärne olekus juhataja* ja *Usaldusväärne sõnastikku*.

Teatiste abil levinud põhjused on:

- Koosteüksuse juhtumiga vaateid, nt teisene registrid või kokkuvõtliku koopia riigi filtreeritud vaadetes. Näide on sorditud index kõigi klahvide, usaldusväärseid sõnastikus.
- Saatmise jälgimise andmed, nt lisatud viimase tunni kasutajate arv.

Teatiste on töötas toimingute rakendamise osana. Seetõttu teatised tuleks käsitleda nii kiiresti kui võimalik ja sünkroonse sündmused ei tohiks kaasata mis tahes kallis toimingud.

## <a name="reliable-state-manager-notifications"></a>Usaldusväärne halduri teatiste

Usaldusväärne olekus halduri pakub järgmist toimuvate sündmuste teatised.

- Tehingu
    - Kinnitamine
- Maakond haldur
    - Taastada
    - Usaldusväärne olekus lisamine
    - Usaldusväärne oleku eemaldamine

Usaldusväärne olekus halduri jälitab praeguse reisikonsultandi tehingud. Tehingu olek, mis põhjustab teate saab ainult muudatus on tehingu on kinnitatud.

Usaldusväärne olekus haldur säilitab kogumi usaldusväärne olekus nagu usaldusväärne sõnastik ja usaldusväärne järjekorda. Usaldusväärne olekus halduri käivitub teatisi, kui selle saidikogumi muutub: usaldusväärne olek on lisada või eemaldada, või terve saidikogumi luuakse.
Usaldusväärne olekus halduri saidikogumi luuakse kolme juhtudel:

- Taastamine: Koopia käivitamisel see taastab eelmise seisu ketas. Taastamise lõppu kasutab **NotifyStateManagerChangedEventArgs** tule sündmus, mis sisaldab komplekt taastatud usaldusväärne olekus.
- Täielik koopia: enne koopia saate liituda konfiguratsiooni määramine, peab olema ehitatud. Mõnikord selleks täielik koopia usaldusväärne olekus ülemuse olek peaks rakenduma jõude teise koopia peamine koopia. Usaldusväärne olekus halduri teisene koopia kasutab **NotifyStateManagerChangedEventArgs** tule sündmus, mis sisaldab usaldusväärne märgitud, et see peamine koopia saadud määramine.
- Taastamine: Avariijärgne taaste stsenaariumid, klõpsake selle koopia olekut saab taastada varukoopia **RestoreAsync**kaudu. Sel juhul kasutab usaldusväärne olekus halduri esmane koopia **NotifyStateManagerChangedEventArgs** tule sündmus, mis sisaldab määramine usaldusväärseks märgitud, et seda taastada varukoopia.

Tehingu teatised ja/või halduri teatiste registreerida, peate registreerida **TransactionChanged** või **StateManagerChanged** sündmuste usaldusväärne olekus haldur. Levinud koht registreeruda nende sündmuseohjur on konstruktori stateful teenust. Ehitaja registreerimisel ei jääks märkamata teatest, mis on põhjustatud muudatuste kohta **IReliableStateManager**jooksul.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Sündmuse üksikasjade **TransactionChanged** sündmuseohjuri kasutab **NotifyTransactionChangedEventArgs** . See sisaldab toimingu atribuut (nt **NotifyTransactionChangedAction.Commit**), mis määrab soovitud tüüpi muudatust. See sisaldab ka see tehingu atribuut, mida pakub muudetud kande viide.

>[AZURE.NOTE] Täna, **TransactionChanged** sündmused on tõstetud ainult juhul, kui tehingu on kinnitatud. Toimingu võrdub siis **NotifyTransactionChangedAction.Commit**. Kuid tulevikus sündmuste võib tõsta muud tüüpi tehingu muutuste. Soovitame toimingu kontrollimine ja sündmuse töötlemiseks ainult siis, kui see on üks, et ootate.

Järgmine on näide **TransactionChanged** sündmuseohjuri.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Sündmuse üksikasjade **StateManagerChanged** sündmuseohjuri kasutab **NotifyStateManagerChangedEventArgs** .
**NotifyStateManagerChangedEventArgs** on kaks alaliikide: **NotifyStateManagerRebuildEventArgs** ja **NotifyStateManagerSingleEntityChangedEventArgs**.
Kasutate toimingut atribuudi **NotifyStateManagerChangedEventArgs** enamus **NotifyStateManagerChangedEventArgs** õige alamklassi:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** ja **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Järgmine on näide **StateManagerChanged** teatis sündmuseohjuri.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Usaldusväärne sõnastiku teatised

Usaldusväärne sõnastiku pakub järgmist toimuvate sündmuste teatised.

- Taastada: Nimetatakse kui **ReliableDictionary** on taastatud seisu taastatud või kopeeritud kohaliku riigi ja varundamine.
- Eemalda: Nimetatakse kui **ReliableDictionary** olek on kustutatud **ClearAsync** meetodi abil.
- Lisamine: Kui üksus on lisatud **ReliableDictionary**nimega.
- Update: Nimetatakse värskendamisel üksuse **IReliableDictionary** .
- Eemalda: Nimetatakse kui üksuse **IReliableDictionary** on kustutatud.

Usaldusväärne sõnastiku teatiste saamiseks peate **DictionaryChanged** sündmuseohjuri **IReliableDictionary**sisse registreerima. Levinud koht registreeruda nende sündmuseohjur on **ReliableStateManager.StateManagerChanged** lisada teatis.
Kui **IReliableDictionary** lisatakse **IReliableStateManager** registreerimisel tagab ei jääks märkamata teated.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** on valimi meetod, mis eelmises näites **OnStateManagerChangedHandler** kõned.

Eelmise koodi määrab **IReliableNotificationAsyncCallback** kasutajaliides, koos **DictionaryChanged**. Kuna **NotifyDictionaryRebuildEventArgs** sisaldab liidest **IAsyncEnumerable** --mis peab olema loetletud asünkroonselt--taasloomine teatised on töötas **RebuildNotificationAsyncCallback** asemel **OnDictionaryChangedHandler**kaudu.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] Eelmise koodi, osana töötlemine taastada teate, tühjendatakse esmalt säilitada liidetud olek. Kuna usaldusväärne saidikogumi luuakse uus olek uuesti, on oluline kõik eelmise teatised.

Sündmuse üksikasjade **DictionaryChanged** sündmuseohjuri kasutab **NotifyDictionaryChangedEventArgs** .
**NotifyDictionaryChangedEventArgs** on viis alaliikide. **NotifyDictionaryChangedEventArgs** atribuudi toimingu abil enamus **NotifyDictionaryChangedEventArgs** õige alamklassi:

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** ja **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Soovitused

- *Tehke* täielik teade sündmusi nii kiiresti kui võimalik.
- *Ärge* käivitada mis tahes kallis toiminguid (nt/v-toimingud) sünkroonsed sündmused osana.
- *Kas* vaadata tegevuse tüüp enne töötlete sündmus. Uue toimingu tüüpi võib tulevikus lisada.

Siin on mõned asjad, mida meeles pidada:

- Teatiste on töötas toimingu teostamine osana. Näiteks taastamine teatis on töötav taastamine toimingu Viimane etapp. Taastamise lõpetab pole kuni töödeldakse teatis sündmus.
- Kuna rakendamine toimingud on töötas teatised, nähtaval ainult kohalikult kinnitatud toimingute teatised. Ja kuna toimingud on tagatud ainult kohalikult kinnitatud (ehk teisisõnu logitud), nad võivad või ei pruugi tulevikus tagasi võetud.
- Tee uuesti tegemiseks, on üks teade töötav iga rakendatud toimingu. See tähendab, et kui tehingu T1 sisaldab Create(X), Delete(X) ja Create(X), saate ühe teatise X, üks kustutamise ja ühte loomiseks klõpsake uuesti loomiseks samas järjekorras.
- Toimingute rakendatakse tehingud, mis sisaldavad mitut toimingute jaoks, kus on saadud kasutaja esmane koopia.
- Osana töötlemine pooleli väär, võib olla tagasi võetud mõned toimingud. Teatiste on tõstetud selliste Võta tagasi teenuste selle koopia tagasipööramine ühed punkti. Üks oluline erinevus Võta tagasi teatiste on, et liidetakse sündmused, mis on dubleeritud võtmed. Näiteks kui tehingu T1 on on tagasi võetud, kuvatakse Delete(X) ühe teate.

## <a name="next-steps"></a>Järgmised sammud

- [Usaldusväärne saidikogumid](service-fabric-work-with-reliable-collections.md)
- [Usaldusväärne teenuste Lühijuhend](service-fabric-reliable-services-quick-start.md)
- [Usaldusväärne teenuste varundus ja taaste (avariitaastet)](service-fabric-reliable-services-backup-restore.md)
- [Tootearendusmaterjal, usaldusväärseid saidikogumid](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
