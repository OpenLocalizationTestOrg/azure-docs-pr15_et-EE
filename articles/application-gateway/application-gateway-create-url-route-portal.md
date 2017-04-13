<properties
   pageTitle="Rakenduste portaali tee-põhise reegli loomine portaali abil | Microsoft Azure'i"
   description="Saate teada, kuidas luua tee-põhise reegli rakenduste portaali portaali abil"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Portaali kaudu rakenduste portaali tee-põhise reegli loomine

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-create-url-route-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-create-url-route-arm-ps.md)

URL-i tee põhinev marsruutimine võimaldab seostada marsruudib URL-i tee Http-päringu põhjal. See kontrollib, kas marsruudi tagaandmebaas pool konfigureeritud rakenduste portaali URL-i loendite ja saata määratletud tagaandmebaas rakenduskausta võrguliiklust. Ühise kasutamise marsruutimiseks URL-i-põhine on laadimiseks eri sisutüüpide abil muu tagaandmebaas serveri kaustu.

Rakenduste portaali URL-i-põhine marsruutimine tutvustab uue reegli tüüp. Rakenduse lüüsi sisaldab kahte tüüpi toiminguid reeglit: lihtne ja tee reeglid. Lihtsa reegli tüüp pakub round-jaan teenust tagaandmebaas kaustu ajal Lisaks round jaan jaotuse tee vahemikel põhinevaid reegleid, arvestatakse ka taotluse URL-i tee mustri kirjutamata rakenduskausta valimise ajal.

## <a name="scenario"></a>Stsenaarium

Järgmistel läheb läbi tee-põhise reegli loomine olemasoleva rakenduste portaali.
Stsenaarium eeldab, et teil on juba juhiste [Rakenduste portaali](application-gateway-create-gateway-portal.md)loomine.

![URL-i marsruutimiseks][scenario]

## <a name="createrule"></a>Tee-põhise reegli loomine

Tee põhinev reegel on vaja oma kuulajale, enne reegli loomise veenduge, et teil on saadaval kuulajale kasutada.

### <a name="step-1"></a>Samm 1

Liikuge http://portal.azure.com ja valige olemasolev rakenduste portaali. Klõpsake nuppu **reeglid**

![Rakenduse lüüsi ülevaade][1]

### <a name="step-2"></a>Samm 2

**Tee põhinev** lisamiseks klõpsake nuppu Uus tee põhinev reegel.

### <a name="step-3"></a>Samm 3

**Lisa tee põhinev reegel** tera on kaks jaotist. Esimene jaotis on, kus määratletud kuulajale, nimi reegli ja tee vaikesätted. Marsruudib, mis ei kuulu kohandatud tee põhinev marsruutimiseks on tee vaikesätted. **Lisa tee põhinev reegel** tera teine osa on määratleda tee põhinev reeglid.

**Põhisätted**

- **Nimi** – see on sõbralik nimi, mis reegel, mis on juurdepääsetav portaalis.
- **Kuulajale** – see on kuulajale, mida kasutatakse reegli.
- **Kirjutamata vaikekausta** – see säte on säte, mis määratleb tagaandmebaas kasutatakse vaikimisi reegel
- **Vaikimisi HTTP sätted** – see säte on säte, mis määratleb HTTP sätteid reegli vaikimisi kasutada.

**Tee põhinevad reeglid**

- **Nimi** – see on sõbralik nimi, mis tee põhinev reegel.
- **Teed** – see säte määratleb reegel otsib liikluse edasisaatmisel tee
- **Taustväärtus Pool** – see säte on säte, mis määratleb kasutatava reegli tagaandmebaas
- **HTTP säte** – see säte on säte, mis määratleb reegli kasutatakse HTTP sätted.

>[AZURE.IMPORTANT] Teed: Tee mustrite, et need vastaksid loend. Iga peab algama / ja ainult mõne "\*" on lubatud on lõpus. /Xyz, /xyz* või /xyz/*on lubatud.  

![Lisage tee põhinev reegel blade teabega täidetud][2]

Olemasoleva rakenduste portaali tee-põhise reegli lisamine on lihtne protsess portaali kaudu. Kui tee põhinev reegel on loodud, saate seda redigeerida täiendavad reeglid hõlpsalt lisada. 

![täiendavad tee põhinevaid reegleid lisamine][3]

## <a name="next-steps"></a>Järgmised sammud

Siit saate teada, kuidas konfigureerida SSL-i mahalaadimine Azure'i rakenduse Gateway vt [Offload SSL-i konfigureerimine](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png