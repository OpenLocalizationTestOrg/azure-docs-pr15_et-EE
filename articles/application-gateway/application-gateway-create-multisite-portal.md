<properties
   pageTitle="Olemasoleva rakenduste portaali mitme saitide Azure'i portaalis majutamiseks konfigureerimine | Microsoft Azure'i"
   description="Sellelt lehelt leiate juhiseid olemasoleva Azure rakenduste portaali hosting mitme veebirakenduste Azure'i portaalis sama lüüsi konfigureerimine."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Olemasoleva rakenduste portaali hosting mitme veebirakenduste konfigureerimine

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-create-multisite-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Mitme saidi hosting võimaldab teil rohkem kui üks veebirakenduse sama rakenduse lüüsi juurutamine. See tugineb hosti päise sissetulevate HTTP-päringu, kindlaks teha, millised kuulajale saaksid liikluse olemasolu. Kuulajale siis suunab liikluse vastav kirjutamata rakenduskausta konfigureeritud lüüsi määratluses reeglid. SSL-i lubatud veebirakendusi, sõltub rakenduse lüüsi serveri teave (SNI) laiendamiseks valige õige kuulajale web liikluse. Ühise kasutamise mitme saidi hosting on laadimiseks jaoks eri web domains eri tagaandmebaas server kaustu. Samuti võib mitme alamdomeenide sama juurdomeeni majutatud rakenduse samas lüüsis.

## <a name="scenario"></a>Stsenaarium

Järgmises näites on rakenduse lüüsi serveeritakse liiklus contoso.com ja fabrikam.com kaks tagaandmebaas serveri kaustu: contoso serveri pool ja fabrikam serveri pool. Sarnane setup võib kasutada host alamdomeene nagu app.contoso.com ja blog.contoso.com.

![mitmes kohas stsenaarium][multisite]

## <a name="before-you-begin"></a>Enne alustamist

Selle stsenaariumi lisab olemasolevate rakenduste portaali mitme saidi tugi. Nendeks toiminguteks peab stsenaarium olemasoleva rakenduste portaali kättesaadav konfigureerimine. Külastage [loomine rakenduste portaali portaali abil](./application-gateway-create-gateway-portal.md) saate teada, kuidas luua lihtsa rakenduse lüüsi portaalis.

## <a name="requirements"></a>Nõuded

- **Tagaandmebaas serveri pool:** Tagaandmebaas serverite IP-aadresside loend. IP-aadresside loetletud kuuluma kas virtuaalse alamvõrku või tuleks avaliku IP/VIP. Saate kasutada ka FQDN.
- **Tagaandmebaas serverisätete pool:** Igal pool on sätted, nt portide, Protocol (protokoll) ja küpsise vastavalt osaleja. Need sätted on seotud on ja rakendatakse kõikides serverites maksimaalselt pool.
- **Ees port:** See port on avatud rakenduse lüüsi avaliku port. Liikluse tabab seda porti ja seejärel ümbersuunamist üks tagaandmebaas serverid.
- **Kuulajale:** Kuulajale on ees port, protokolli (Http- või Https need väärtused on tõstutundlik), ja SSL-i serdi nimi (kui offload SSL-i konfigureerimine). Mitme saidi lubatud rakenduse lüüside jaoks ka lisatakse hosti nimi ja SNI näidikuid.
- **Reegel:** Reegli seob kuulajale, tagaandmebaas serveri pool, ja millist tagaandmebaas serveri rakenduskausta liiklus tuleks suunata kui see tabab kindla kuulajale määratleb.
- **Serdid:** Iga kuulajale nõuab kordumatu sert, järgmises näites luuakse 2 kuulajatele mitme saidi jaoks. Kahe pfx serdid ja paroolide nende jaoks on vaja luua.

## <a name="create-an-application-gateway"></a>Rakenduste portaali loomine

Rakenduse lüüsi värskendamine vajalikud toimingud on järgmised:

1. Luua tagaandmebaas iga saidi jaoks.
2. Luua uue kuulajale iga saidi rakenduse lüüsi toetab.
3. Saate luua reeglid iga kuulajale koos vastav tagaandmebaas vastendamiseks.

## <a name="create-back-end-pools-for-each-site"></a>Luua tagaandmebaas iga saidi jaoks

Tagaandmebaas pool iga saidi jaoks selle rakenduse lüüsi kas tugi on vajalik, kui 2 luuakse üks contoso11.com ja üks fabrikam11.com.

### <a name="step-1"></a>Samm 1

Avage olemasolev rakenduste portaali Azure'i portaalis (https://portal.azure.com). Valige **Taustväärtus kaustu** ja klõpsake nuppu **Lisa**

![taustväärtus kaustu lisada.][7]

### <a name="step-2"></a>Samm 2

Sisestage soovitud tagaandmebaas rakenduskausta **pool1**, lisada ip-aadresside või FQDN-i serverite tagaandmebaas teave ja klõpsake nuppu **OK**

![kirjutamata rakenduskausta pool1 sätted][8]

### <a name="step-3"></a>Samm 3

Enne taustväärtus-kaustu lisamiseks klõpsake nuppu **Lisa** mõni täiendavad tagaandmebaas rakenduskausta **pool2**, lisada ip-aadresside või FQDN-i serverite tagaandmebaas ja klõpsake nuppu **OK**

![kirjutamata rakenduskausta pool2 sätted][9]

### <a name="create-listeners-for-each-back-end"></a>Looge iga tagaandmebaas jaoks kuulajatele

Rakenduse lüüsi tugineb HTTP 1.1 hosti päised majutada samas avaliku IP-aadress ja pordi rohkem kui üks veebisaiti. Tavaline kuulajale portaali loodud ei sisalda selle atribuudi.

### <a name="step-1"></a>Samm 1

**Kuulajatele** olemasoleva rakenduse lüüsi ja seejärel klõpsake käsku **mitme saidi** lisamiseks esimene kuulajale.

![blade kuulajatele ülevaade][1]

### <a name="step-2"></a>Samm 2

Täitke kuulajale, selles näites SSL-i lõpetamise teave on konfigureeritud, Loo uus frontend port. Laadige üles sert pfx, kasutatakse SSL-i lõpetamine. Erinevus ainult selle tera võrreldes standardse lihtsa kuulajale tera on hostname.

![kuulajale atribuudid blade][2]

### <a name="step-3"></a>Samm 3

Klõpsake **mitme saidi** loomine teise kuulajale, nagu on kirjeldatud teise saidi eelmist toimingut. Veenduge, et kasutada mõne muu serdi teise kuulajale. Erinevus ainult selle tera võrreldes standardse lihtsa kuulajale tera on hostname. Täitke kuulajale teave ja klõpsake nuppu **OK**.

![kuulajale atribuudid blade][3]

> [AZURE.NOTE] Kuulajatele Azure'i portaalis rakenduste portaali loomine on pikaajalise tööülesande, võib kuluda aega luua kaks kuulajatele selle stsenaariumi. Kui täielik kuulajatele kuvamine portaalis, nagu järgmisel pildil näha.

![kuulajale ülevaade][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Vastendage kuulajatele kirjutamata kaustu reeglite loomine

### <a name="step-1"></a>Samm 1

Avage olemasolev rakenduste portaali Azure'i portaalis (https://portal.azure.com). Valige **reeglid** ja valige olemasolev vaikimisi reegel **rule1** ja klõpsake nuppu **Redigeeri**.

### <a name="step-2"></a>Samm 2

Täitke reeglid tera, nagu järgmisel pildil näha. Valides esimese kuulajale ja esimese kausta ja klõpsake nuppu **Salvesta** , kui olete lõpetanud.

![olemasoleva reegli redigeerimine][6]

### <a name="step-3"></a>Samm 3

Klõpsake nuppu **tavaline reegel** 2 reegli loomiseks. Täitke vorm teise kuulajale ja teine kirjutamata rakenduskausta ja klõpsake siis nuppu **OK** salvestada.

![lihtsa reegli blade lisamine][10]

See on lõpule jõudnud, olemasoleva rakenduste portaali seadistamine mitme saidi tugi Azure portaali kaudu.

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas kaitsta teie veebisaiti [rakenduse](application-gateway-webapplicationfirewall-overview.md) Gateway - Web rakenduse tulemüüri

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png