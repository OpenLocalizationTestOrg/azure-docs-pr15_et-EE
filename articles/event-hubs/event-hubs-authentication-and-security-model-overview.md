<properties 
    pageTitle="Sündmuse jaoturi autentimis- ja turvalisuse mudeli ülevaade | Microsoft Azure'i"
    description="Sündmuse jaoturi autentimis- ja turvalisuse mudeli ülevaade."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Sündmuse jaoturi autentimis- ja turvalisuse mudeli ülevaade

Sündmuse jaoturi mudelit vastab järgmistele tingimustele:

- Ainult seadmetes, mida esitada sobiv mandaate saate saata andmed sündmuse jaoturiga.
- Seade ei saa jäljendada mõnda muud seadet.
- Andmete saatmine sündmuse jaoturiga võib olla blokeeritud petturitest seade.

## <a name="device-authentication"></a>Seadme autentimine

Sündmuse jaoturi mudelit põhineb [Ühiskasutusse Accessi allkirja (SAS)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) märkide ja *sündmuse tootjad*kombinatsiooni. Kuvatakse sündmuse publisher määratleb virtuaalse lõpp-punkti jaoks soovitud sündmus jaoturi. Avaldaja saab kasutada ainult sündmuse jaoturiga sõnumite saatmiseks. Ei ole võimalik avaldaja sõnumeid vastu.

Tavaliselt on sündmuse jaoturi töötab ühe Publisheri seadme kohta. Kõik sõnumid, mis saadetakse ühte mõni sündmus jaoturi avaldajate on enqueued sündmuse jaoturi sees. Tootjad lubada kohandatud juurdepääsu reguleerimine ja pidurdamise.

Igas seadmes on määratud kordumatu luba, mis on laaditud seade. Märkide toodeti nii, et iga kordumatu luba annab erinevate kordumatu Publisher. Seade, mis on märgiks saate saata ainult ühe Publisheri, kuid ei ole muud Publisheri. Kui mitu seadet samamoodi, siis iga seadme jagab avaldaja.

Kuigi pole soovitatav, on võimalik varustada seadmed koos märkide, mis sündmuse jaoturiga otsest juurdepääsu andmiseks. Mis tahes seadmes, kuhu see on Turbeloa saata sõnumeid otse sündmuse jaoturi. Selline seade ei kuulu pidurdamise. Lisaks ei saa seadme kaudu saata sündmuse jaoturi musta nimekirja.

Kõik märgid on allkirjastatud SAS klahvi. Tavaliselt on kõik sõned allkirjastatud sama võti. Seadmete ei tea võti; See takistab seadmete valmistamine sõned.

### <a name="create-the-sas-key"></a>Looge SAS võti

Mõni sündmus jaoturi nimeruum loomisel loob Azure'i sündmuse jaoturi nimega **RootManageSharedAccessKey**256-bitine SAS klahvi. Selle võtme toetused saata, kuulata ja nimeruumi õiguste haldamine. Saate luua täiendavaid võtmed. Soovitatav on, et te aedvili toetused saata õigused teatud sündmuste jaoturi klahvi. Selles teemas ülejäänud, eeldatakse, et te nimega selle klahvi `EventHubSendKey`.

Järgmises näites luuakse saada ainult klahvi sündmuse jaoturi loomisel:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Luua sõned

Märkide SAS võtme abil saate luua. Peab esitama ainult üks märk seadme kohta. Märkide saate esitada siis meetodit kasutades. Kõikide märkide luuakse **EventHubSendKey** võtme abil. Iga luba määratakse ainulaadne URI.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Seda meetodit helistamisel URI peaks olema määratud `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Kõikide märkide, URI on identne, välja arvatud `PUBLISHER_NAME`, mis peaks olema iga luba erinev. Ideaalvariandis mitte `PUBLISHER_NAME` tähistab seade, mida saab, seega ID-d.

See meetod loob luba järgmine struktuur:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

Turbeloa aegumise aeg on määratud sekundite 1 jaan 1970. Järgmine on näide märgiks.

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Märkide on tavaliselt, mis sarnaneb või seadme eluiga ületab eluiga. Kui seade on võimalus saada uue loa, saab märgid, mis on lühem.

### <a name="devices-sending-data"></a>Andmete saatmine seadmed

Kui märkide on loodud, on iga seadme ette valmistatud koos oma kordumatu luba.

Kui seade saadab andmed on sündmuse jaoturiga, sildid seadme selle luba saada taotlus. Selleks, et ründaja pealtkuulamise ja luba varastamine, side seade ja sündmuse jaoturi peab jääma aastasse üle krüptitud kanalit.

### <a name="blacklisting-devices"></a>Musta seadmed

Kui märgiks varastatakse ründaja, saate ründaja jäljendada seadme, mille luba varastatud. Musta seadme renderdab seadme kasutuskõlbmatuks kuni seadme manustatakse uue loa, mis kasutab eri publisher.

## <a name="authentication-of-back-end-applications"></a>Tagaandmebaas taotluste autentimise

Sündmuse jaoturi töötab tagaandmebaas rakendusi, mis kasutavad loodud seadmete andmete kinnitamiseks turvalisus mudelit, mis on sarnane mudelit, mida on kasutatud teenuse siini teemade. Ka sündmuse jaoturi tarbija rühma võrdub teenuse siini teema tellimust. Klient, saate luua tarbija rühma kui taotluse tarbija rühma loomiseks on lisatud märgiks toetused haldamise õigused, sündmuse jaoturi või nimeruumi, kuhu sündmus jaoturi kuulub. Kliendi on lubatud kasutamine andmete tarbija rühmast kui vastuvõtu taotlusele on luba, mis annab vastuvõtu tarbija selle rühma, sündmuse jaoturi või nimeruumi, kuhu sündmus jaoturi kuulub.

Teenuse siini praegune versioon ei toeta üksikute tellimuste SAS reeglid. Sama kehtib sündmuse jaoturi rühmi. SAS tugi lisatakse mõlema funktsiooni tulevikus.

Üksikute rühmi SAS autentimise puudumise korral saate SAS klahvide secure kõiki rühmi levinud võti. Seda moodust võimaldab rakenduse kasutamine andmete mis tahes mõni sündmus jaoturi tarbija rühmad.

## <a name="next-steps"></a>Järgmised sammud

Sündmuse jaoturi kohta leiate lisateavet leiate järgmistest teemadest:

- [Sündmuse jaoturi ülevaade]
- Soovitud [järjekorras sõnumside lahenduse] teenuse siini järjekorrad abil.
- Täieliku [valimi rakendus, mis kasutab sündmuse jaoturi].

[Sündmuse jaoturi ülevaade]: event-hubs-overview.md
[valimi rakendus, mis kasutab sündmuse jaoturi]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[kiirsõnumside lahenduse ootele]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
