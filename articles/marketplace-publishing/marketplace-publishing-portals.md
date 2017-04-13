<properties
   pageTitle="Ülevaade eri portaalide vaja luua pakkumise turuplatsil | Microsoft Azure'i"
   description="Erinevate portaalide turuplatsil pakkumise loomiseks vajalik ülevaade"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/27/2016"
   ms.author="hascipio" />


# <a name="portals-you-will-need"></a>Te peate portaalide
Enne alustamist protsessi avaldamise pakkumise vaatame saate kasutusele erinevaid portaalide, mida vajate. Allpool on lühike Kokkuvõte portaalide--Arenduskeskus, Azure'i Avaldamisportaal ja Azure portaali – selleks, et suhtlete nende kohta.                                                                            
## <a name="developer-center"></a>Arenduskeskus
[http://dev.Windows.com/registration?accountprogram=Azure](http://dev.windows.com/registration?accountprogram=azure)
### <a name="description"></a>Kirjeldus
Microsoft Developer Center konto loomise on ühekordse ülesanne. Veenduge, et ettevõtte juba ei saa Arenduskeskus konto enne, kui proovite luua. Käigus me koguda pangakonto teabe, maksuvabastuse teavet ja ettevõtte aadressi andmed.

> [AZURE.NOTE] Kui avaldate ainult tasuta pakub (või tuua – teie-omanik-litsentsi pakub), me ei nõua maksuvabastuse ja panga teavet.

### <a name="identityaccount-used"></a>Identiteedi ja konto, mida kasutatakse
See on ideaalvariandis mitte leviloendi või turberühma (nt azurepublishing@ *partnercompany*.com). Selle jaotuse loendi või turberühma rühma **peab** registreeritud Microsofti kontona.

> [AZURE.TIP] Soovitame kasutada leviloendi või turberühma, kuna see eemaldab sõltuvus isik, kuigi ühe konto saate kasutada ka.

## <a name="publishing-portal"></a>Avaldamisportaal
[https://Publish.windowsazure.com](https://publish.windowsazure.com)

### <a name="description"></a>Kirjeldus
See on portaali kasutatav pakkumine töötamiseks ja avaldada (turundus, hinnad, avaldamise sertimine võimalusel jne).

### <a name="identityaccount-used"></a>Identiteedi ja konto, mida kasutatakse
Ülaltoodud loendis või turberühma levirühma tuleb kasutada esimest korda avaldamise portaali sisse logida. Hiljem saab lisada teiste kasutajate co-administraatorid kujul. See on, kuidas saab vastendada Arenduskeskus andmeobjektid.

## <a name="azure-portal"></a>Azure'i portaal
[https://Portal.Azure.com](https://portal.azure.com)
### <a name="description"></a>Kirjeldus
See on portaal, kus saate oma etapiviisilise ja avaldatud pakutakse Azure'i turuplatsi (VM, lahenduse mallide ja arendaja Azure'i ressursihaldur-põhiste teenuste korral).
### <a name="identityaccount-used"></a>Identiteedi ja konto, mida kasutatakse
Kui olete lavastus pakkumise avaldamise portaalis, tellimuse ID peab olema whitelisted. Ühe tellimuse (seal on kasutajanimi ja parool, mis on seotud) peab see portaali sisse logida etapiviisilise pakkumine testimiseks kasutada.

## <a name="see-also"></a>Vt ka
- [Alustamine: Kuidas avaldada pakkumise Azure turuplatsiga](marketplace-publishing-getting-started.md)
