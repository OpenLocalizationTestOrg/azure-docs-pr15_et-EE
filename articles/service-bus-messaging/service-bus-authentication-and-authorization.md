<properties 
    pageTitle="Teenuse siini autentimise ja luba | Microsoft Azure'i"
    description="Ühiskasutusse antud juurdepääs allkirja (SAS) autentimine ülevaade."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="service-bus-authentication-and-authorization"></a>Teenuse siini autentimise ja luba

Rakendused saate autentimiseks Azure'i teenus siini kummagi ühiskasutusse Accessi allkirja (SAS) autentimist kasutades või Azure Active Directory juurdepääsu reguleerimine (nimetatakse ka juurdepääsu juhtimine teenuse või ACS) kaudu. Ühiskasutusse antud juurdepääs allkirja autentimine lubab rakendusi autentida teenuse siini konfigureeritud nimeruumi või üksus, millega on seotud õigusi kiirklahvide abil. Seejärel saate selle klahvi märgiks ühiskasutusse Accessi allkirja, mis kliendid saavad kasutada teenuse siini autentida loomiseks.

>AZURE'I. OLULISTE SAS soovitatakse üle ACS, pakub teenuse siini jaoks lihtne, paindlik ja lihtne kasutada autentimise skeem. Rakendusi saate kasutada SAS stsenaariumid, kus nad peavad haldama mõiste volitatud "kasutaja".

## <a name="shared-access-signature-authentication"></a>Ühiskasutusega juurdepääsu allkirja autentimine

[SAS autentimise](service-bus-sas-overview.md) võimaldab kasutaja juurdepääsu teenuse siini ressursid teatud õigused. Teenuse siini SAS autentimine hõlmab krüptimisvõtme seotud õigustega siini Service ressursi konfiguratsiooni. Kliendid võivad seejärel juurde pääseda selle ressursi esitades SAS luba, mis koosneb ressursile juurdepääsu URI ja aegumist allkirjastatud konfigureeritud võti.

Saate konfigureerida klahvid SAS teenuse siini nimeruumi. Võti kehtib kõik selle nimeruumi sõnumside üksuste. Samuti saate konfigureerida klahvid teenuse siini järjekorrad ja teemade kohta. SAS toetavad ka teenuse siini edastab.

SAS kasutamiseks saate konfigureerida [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) objekti nimeruumi, järjekorda või teema, mis koosneb järgmistest:

- *KeyName* , mille abil tuvastatakse reegel.

- *PrimaryKey* on kasutatud Logi/valideerimiseks SAS sõned krüptimisvõtme.

- *SecondaryKey* on kasutatud Logi/valideerimiseks SAS sõned krüptimisvõtme.

- *Õiguste* tähistav kuulata, saatmine või haldamine õigused, mis on antud kogum.

Luba reeglid, mis on konfigureeritud nimeruum tasemel anda juurdepääsu kõigi üksuste nimeruumi klientidele koos märkide allkirjastatud vastavat klahvi. Kuni 12 luba reegleid saab konfigureerida teenuse siini nimeruumi, järjekorda või teema. Vaikimisi on iga nimeruum konfigureeritud [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) kõik õigused kui esmalt on ette valmistatud.

Juurdepääsu üksusele nõuab kliendi teatud [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx)abil loodud SAS märgiks. Koos krüptimisvõtme aegumise seotud reegli autoriseerimine ja SAS luba on loodud, kasutades HMAC-i-SHA256 ressursi stringi, mis koosneb ressursi URI, millele juurdepääsu taotletakse.

SAS autentimise tugi teenuse siini on kaasatud Azure'i .NET SDK versioonid 2.0 ja uuemad versioonid. SAS sisaldab mõnda [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx)tugi. Kõigi API-de ühendusstringi parameetrina aktsepteerib toetavad SAS ühendusstringi.

## <a name="acs-authentication"></a>ACS autentimine

Teenuse siini autentimise kaudu ACS hallatakse kaudu soovitud companion "-sb" ACS nimeruum. Kui soovite luua teenuse siini nimeruumi companion ACS nimeruumi, ei saa luua oma teenuse siini nimeruum Azure klassikaline portaalis; peate looma nimeruumi [New-AzureSBNamespace](https://msdn.microsoft.com/library/azure/dn495165.aspx) PowerShelli cmdlet-käsu abil. Näiteks:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Mõne ACS nimeruum loomise vältimiseks välja järgmine käsk:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Näiteks kui loote nimetatakse **contoso.servicebus.windows.net**teenuse siini nimeruumi, companion ACS nimeruum nimega **contoso-sb.accesscontrol.windows.net** on ette valmistatud automaatselt. Kõik nimeruumid, mis on loodud enne August 2014 ka loodi on lisatud ACS nimeruum.

Teenuse vaikeidentiteedi "omanik" kõik õigused, on see companion ACS nimeruum vaikimisi ette valmistatud. Kohandatud juhtelemendi teenuse siini üksus ACS kaudu saate hankida vastav usalda seosed konfigureerimisega. Saate konfigureerida lisateenuse identiteedid juurdepääsu teenuse siini üksuste haldamiseks.

Juurdepääsu üksusele kliendi taotleb mõne SWT luba ACS asjakohaste nõuete esitades oma mandaadi. Luba SWT siis tuleb saata taotluse osana teenuse siini lubamiseks kliendi ettevõte juurdepääsu luba.

ACS autentimise tugi teenuse siini on kaasatud Azure'i .NET SDK versioonid 2.0 ja uuemad versioonid. Seda autentimist sisaldab mõnda [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx)tugi. Kõigi API-de ühendusstringi parameetrina aktsepteerib toetavad ACS ühendusstringi.

## <a name="next-steps"></a>Järgmised sammud

Jätkata lugemist SAS kohta lisateabe saamiseks [teenuse siini ühiskasutusse Accessi allkirja autentimine](service-bus-shared-access-signature-authentication.md) .

Üksikasjalik ülevaade Lennureisi ajal teenuse siini leiate [Ühiskasutusse Accessi allkirjad](service-bus-sas-overview.md).

Leiate lisateavet ACS märkide [kohta: taotleda märgiks ACS OAuthi MURRA protokolli](https://msdn.microsoft.com/library/hh674475.aspx).



