<properties
   pageTitle="Luua rakenduse abil ASP.net-i Core vastendamisel web | Microsoft Azure'i"
   description="Nähtavaks tegemine veebis teenuse struktuuri rakenduse abil on ASP.net-i Core veebi-API projekt ja omavahel teenuse ServiceProxy kaudu suhtlemine."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2016"
   ms.author="seanmck"/>


# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Web teenuse vastendamisel Core ASP.net-i abil rakenduse koostamine

Vaikimisi ei paku Azure teenuse struktuuri teenuste avaliku kasutajaliidese veebis. Jätke rakenduse funktsioonid HTTP klientidele, peate tegutseda sisendpunkti ja seejärel sealt edastama üksikute teenuste web projekti loomise kohta.

Selles õpetuses me kättesaamine, kus meil pooleli jäite [loomise esimese rakenduse Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) õpetuse ja veebiteenuse stateful letiteeninduse ette lisada. Kui te pole seda juba teinud, peaksite tahapoole ja samm kaudu selle õpetuse esmalt.

## <a name="add-an-aspnet-core-service-to-your-application"></a>ASP.net-i Core teenuse lisamiseks rakenduse

ASP.net-i tuum on kerge, mitu platvormi web arendamise raamistik, mille abil saate luua tänapäevane web UI ja web API-d. Meie olemasoleva rakendusega lisamine on ASP.net-i veebi-API projekt.

>[AZURE.NOTE] Selle õpetuse lõpuleviimiseks teil on vaja [installida .NET Core 1.0][dotnetcore-install].

1. Lahenduste Explorer, paremklõpsake **teenuste** rakenduse project ja valige **Lisa > uus struktuuri teenus**.

    ![Taotluse uue teenuse lisamine][vs-add-new-service]

2. Klõpsake lehe **loomine teenuse** , valige **ASP.net-i Core** ja sellele nime panna.

    ![ASP.net-i veebiteenus valimine dialoogiboksis Uus teenus][vs-new-service-dialog]

3. Järgmisel lehel pakub ASP.net-i Core komplekti projekti mallid. Pange tähele, et need on sama malle, mida soovite näha, kui olete loonud mõne ASP.net-i Core projekti väljaspool teenuse struktuuri rakenduse. Selles õpetuses mõeldud valisime **Web API -ga**. Siiski saate rakendada sama põhimõtet koostamise täielik veebirakenduse.

    ![ASP.net-i projekti tüübi valimine][vs-new-aspnet-project-dialog]

    Kui teie project Web API on loodud, on teil kaks teenuste oma rakenduse. Kui rakenduse jätkama lisate rohkem teenuseid täpselt samamoodi. Iga saab sõltumatult versiooniga ja uuele versioonile üle viidud.

>[AZURE.TIP] ASP.net-i Core services koostamise kohta leiate lisateavet teemast [ASP.net-i Core dokumentatsiooni](https://docs.asp.net).

## <a name="run-the-application"></a>Käivitage rakendus

Saada tunnet, mida olete teinud, loome uue rakenduse juurutamine ja Heitke pilk ASP.net-i Core Veebiteenuste malli sisaldava vaikekäitumise.

1. Vajutage klahvi F5, et silumine rakenduse Visual Studio.

2. Kui juurutamise on lõpule jõudnud, käivitab Visual Studio brauseri ASP.net-i veebi-API juurdomeen--midagi http://localhost:33003. Pordi number on määratud juhusliku ID-ga ja võivad olla erinevad teie arvutis. ASP.net-i Core Veebiteenuste malli ei paku vaikekäitumist juures nii tõrke saate brauseris.

3. Lisage `/api/values` brauseris asukohta. See käivitub selle `Get` meetod ValuesController Veebiteenuste malli. See vastus vaikimisi, mis on esitatud mall--JSON massiiv, mis sisaldab kaks tekstistringi tagasi:

    ![Vaikimisi ASP.net-i Core veebi-API malli tagastatud väärtused][browser-aspnet-template-values]

    Õpetuse lõpus me ei on asendada need vaikeväärtused on viimase väärtuse meie stateful teenusest.


## <a name="connect-the-services"></a>Teenuste ühendamine

Teenuse struktuuri pakub täielikku paindlikkust kuidas kellega suhtlete, usaldusväärseid teenuseid. Ühe rakendusest võib olla mis teenused on kättesaadavad TCP, muude teenustega, mis on kättesaadavad HTTP REST API ja veel muid teenuseid, mis on kättesaadavad web sockets. Saadaolevad suvandid ja seotud kompromissidega leiate [suhtlemine teenused](service-fabric-connect-and-communicate-with-services.md). Selles õpetuses järgige ühte lihtsam võimalustest ja kasutage soovitud `ServiceProxy` / `ServiceRemotingListener` tunnid, mis on toodud SDK.

Klõpsake soovitud `ServiceProxy` lähenemine (eeskujul kaugprotseduurikutse kõned või kaugprotseduurikutsete), määratlete liidest tegutseda lepingu teenuse. Seejärel kasutage seda kasutajaliidese loomiseks puhverserveri klassi teenuse suhtlemiseks.


### <a name="create-the-interface"></a>Kasutajaliideses loomine

Alustame loomise tegutseda stateful teenuse ja oma klientidele, sh projekti ASP.net-i kasutajaliidese abil.

1. Lahenduste Explorer, paremklõpsake teie lahendus ja valige **Lisa** > **Uue projekti**.

2. Valige vasakpoolsel navigeerimispaanil **Visual C#** kirje ja seejärel valige **Klassiteek** mall. Veenduge, et .NET Framework versioon on määratud **4.5.2**.

    ![Mõne oma stateful teenuse kasutajaliidese projekti loomine][vs-add-class-library-project]

3. Selleks, et olla kasutatavaks liidest `ServiceProxy`, peab see olema saadud IService kasutajaliidese kaudu. Selle kasutajaliidese sisaldub teenuse struktuuri NuGet-paketid. Paketi lisamiseks paremklõpsake oma tunni teeki uue projekti ja valige **Haldamine NuGet-paketid**.

4. **Microsoft.ServiceFabric.Services** paketi otsida ja installida.

    ![Teenuste Nugeti paketi lisamine][vs-services-nuget-package]

5. Klassi teegis luua liides ühe meetodi, `GetCountAsync`, ja laiendamine kasutajaliideses IService kaudu.

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### <a name="implement-the-interface-in-your-stateful-service"></a>Rakendada kasutajaliideses stateful teenust

Nüüd, kui meil on määratletud kasutajaliides, läheb vaja seda rakendada stateful teenus.

1. Lisage stateful teenust, klassi teegi projekti, mis sisaldab kasutajaliideses viide.

    ![Klassi Raamatukogu projekti viite stateful teenuse lisamine][vs-add-class-library-reference]

2. Leidke pärib klassi `StatefulService`, näiteks `MyStatefulService`, ja laiendamine seda rakendada soovitud `ICounter` kasutajaliidese.

    ```c#
    using MyStatefulService.Interfaces;

    ...

    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. Nüüd rakendada soovitud meetod, mis on määratletud funktsiooni `ICounter` liidest `GetCountAsync`.

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Teenuse remoting kuulajale stateful teenuse nähtavaks tegemine

Kus on `ICounter` kasutajaliidese rakendada, viimane toiming, võimaldades olema sissenõutav muude teenuste stateful teenus on side kanali avamiseks. Stateful teenuste, teenuse struktuuri pakub overridable meetodit, mida kutsutakse `CreateServiceReplicaListeners`. Selle meetodi abil saate määrata ühe või mitme side kuulajatele, sõltuvalt suhtlusviisid, mida soovite lubada teenust.

>[AZURE.NOTE] Võrdväärne meetodi avamine side kanali kodakondsuseta teenuste nimetatakse `CreateServiceInstanceListeners`.

Selles näites me asendada olemasoleva `CreateServiceReplicaListeners` meetod ja sisestage eksemplari `ServiceRemotingListener`, mis loob RPC lõpp-punkti, mis on sissenõutav klientide kaudu `ServiceProxy`.  

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>ServiceProxy klassi abil saate suhelda teenus

Meie stateful teenus on nüüd valmis liikluse saadud muude teenustega. Seega on jääb lisades suhelda veebiteenusest ASP.net-i kood.

1. ASP.net-i projektis lisamine ainekursuse teek, mis sisaldab soovitud `ICounter` kasutajaliidese.

2. Avage menüü **koostada** **Configuration Manager**. Peaksite nägema umbes selline:

    ![Konfiguratsiooni halduri kuvav klassi Raamatukogu AnyCPU][vs-configuration-manager]

    Pange tähele, et luua mis tahes CPU on konfigureeritud klassi Raamatukogu projekti **MyStatefulService.Interface**. Teenuse struktuuri õigesti töötada, see peab olema konkreetselt suunatud x64. Klõpsake platvormi ripploendit ja valige **Uus**ja seejärel loomine x x64 platvormi konfigureerimine.

    ![Uue platvormi klassiteek loomine][vs-create-platform]

3. Lisage Microsoft.ServiceFabric.Services paketi ASP.net-i projekti, nagu klassi teegi projekti varasemas versioonis. See on `ServiceProxy` klassi.

4. Avage kaustas **kontrollerid** on `ValuesController` klassi. Pange tähele, et selle `Get` meetod just praegu on massiiv raske koodiga string "väärtus1" ja "väärtus2" – mis vastab, mida me kuvati varem brauseris. Järgmine kood selle asendamiseks tehke järgmist.

    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;

    ...

    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    Koodi esimene rida on oluline. ICounter puhverserveri stateful teenusega loomiseks peate pakuvad teavet kahe andmeühikuga: sektsiooni ID ja teenuse nimi.

    Saate kasutada eraldamine skaala stateful Services kaupa üles erinevate kibudega sisse oma oleku põhjal klahvi, mis määratlevad, nt Tellijaid või sihtnumber. Meie triviaalne rakenduses on stateful teenuse ühe sektsiooni, ainult nii, et võti järjekord oluline. Suvalist klahvi, mis pakub viib sama sektsiooni. Teavet eraldamine teenuste kohta leiate teemast [sektsiooni teenuse struktuuri usaldusväärsed teenused](service-fabric-concepts-partitioning.md).

    Teenuse nimi on URI vormi struktuuri: /&lt;rakenduse_nimi&gt;/&lt;service_name&gt;.

    Nende kahe andmeühikuga teabe abil saate teenuse struktuuri kordumatult arvutisse, mis peaks saadab. Funktsiooni `ServiceProxy` klassi töötleb ka sujuvalt stateful teenuse sektsiooni majutava nurjub ja mõnes muus arvutis lasta end ülendada oma kohta. See võtmiseks teeb kliendi koodi tegelemiseks märkimisväärselt lihtsam muude teenustega.

    Kui oleme puhverserverist, saame lihtsalt autonoomsest selle `GetCountAsync` meetod ja selle tulemi tagastamiseks.

5. Vajutage klahvi F5 uuesti muudetud rakenduse käivitada. Nagu enne, Visual Studio käivitab automaatselt brauseri web project juurkausta. Lisage "api/väärtused" tee ja näete selle tagasi praeguse väärtuse.

    ![Funktsiooni stateful väärtust kuvatakse brauseris][browser-aspnet-counter-value]

    Värskendage brauserit, et näha selle väärtuse värskendada.


>[AZURE.WARNING] ASP.net-i Core veebiserver lähtutud malli, mida nimetatakse tuuletallaja, on [Otsene interneti-liikluse käsitlemise pole praegu toetatud](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). Peamised, on soovitatav majutada oma Core ASP.net-i lõpp-punktid taha [API halduse] [ api-management-landing-page] või mõne muu Interneti-ühendusega lüüsi. Pange tähele, et teenuse struktuuri ei toetata sees IIS-i juurutamiseks.


## <a name="what-about-actors"></a>Kuidas on lood osalejate?

Selle õpetuse suunatud web esiosa, mida stateful teenuse lisamine. Siiski saate jälgida üsna sarnased mudeli osalejate rääkida. Tegelikult on mõnevõrra lihtsam.

Kui loote mõnda näitleja projekti, loob Visual Studio kasutajaliidese projekt, mis teie jaoks automaatselt. Selle kasutajaliidese abil saate luua ka näitleja puhverserveri web projekti osaleja suhelda. Kanali side on esitatud automaatselt. Seega pole vaja midagi, mis on võrdne, millega on `ServiceRemotingListener` sarnaselt selles õpetuses stateful teenuse kohta.

## <a name="how-web-services-work-on-your-local-cluster"></a>Veebiteenused töötamisviisi kohaliku klaster

Üldiselt saate täpselt sama teenuse struktuuri rakenduse mitme masina kobar, klõpsake kohaliku klaster juurutatud ja olla kindel, et ta töötab ootuspäraselt. Selle põhjuseks on kohaliku klaster on lihtsalt viie sõlme konfiguratsiooni, mis on ahendatud ühe masina.

Kui tegemist on veebiteenused, aga on üks klahv nüanss. Klaster asub taga laadi koormusetasakaalustusteenuse, kuna see Azure, peab tagate oma veebiteenuste juurutatud iga arvutisse Kuna laadi koormusetasakaalustusteenuse on lihtsalt round jaan liikluse masinad üle. Saate seda teha, seades selle `InstanceCount` teenuse, et teisiti väärtuse "-1".

Aga kui käivitate veebiteenuse kohalikult, peate veenduge, et ainult ühel teenus töötab. Muul juhul kuvatakse tekib konfliktide sama tee ja pordi kuulamise mitme protsessidest. Selle tulemusena seadma "1" kohaliku juurutuste web teenuse eksemplari arv.

Erinevate väärtuste erinevate keskkonna konfigureerimise kohta leiate teemast [haldamine rakenduse parameetrid mitme keskkonna jaoks](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Järgmised sammud

- [Luua klaster Azure pilveteenusesse rakenduse juurutamine](service-fabric-cluster-creation-via-portal.md)
- [Lisateavet teenuste suhtlemine](service-fabric-connect-and-communicate-with-services.md)
- [Lisateavet eraldamine stateful teenused](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/en-us/services/api-management/
