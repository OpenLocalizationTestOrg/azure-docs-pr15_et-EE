
<properties
    pageTitle="Rakenduse nõuded Azure RemoteApp | Microsoft Azure'i"
    description="Siit saate teada, rakendused, mida soovite kasutada Azure RemoteApp nõuete kohta"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="app-requirements"></a>Rakenduse nõuded

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp toetab streaming 32-bitine või 64-bitise Windowsi põhiseid rakendusi Windows Server 2012 R2 pildilt. Enamik olemasolevaid 32-bitine või 64-bitise Windowsi põhiseid rakendusi käivitada "olemasoleval kujul" Azure RemoteApp (Kaugtöölauateenuseid või varem tuntud terminaliteenused) keskkonnas. On siiski vahe töötab ja töötab ka - mõned rakendused korralikult ja teha ka, teised mitte. Järgmine teave sisaldab juhiseid Kaugtöölauateenuseid keskkonnas rakenduste arendamise ja testimise ühilduvuse tagamiseks.

Näpunäide: Töötame loomise näiteid töötamise rakendused teie eest. Kuvatakse uus teemadele, kus käsitletakse Microsoft Accessi, QuickBooksi ja App-V kasutamine RemoteApp.

## <a name="requirements"></a>Nõuded
Järgmised kolm tingimust, kui jälgitavad, aidata käivitada ka RemoteApp rakenduse.

1.  Rakendusi, mis kõik [Windowsi töölauarakenduste nõuded](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) ja järgida [Kaugtöölauateenuseid kavandamise juhised](https://msdn.microsoft.com/library/aa383490.aspx) on lõpule viidud ühilduvuse RemoteApp.
2.  Rakenduste peaks kunagi salvestada andmete pildi või RemoteApp eksemplarid, mis võib olla kaotsi.  Kui olete loonud RemoteApp kogum, linnanimede on kloonitud ja on ja peaks sisaldama ainult rakendusi. Andmeid salvestada välise andmeallikaga või kasutajaprofiili.
3.  Kohandatud pilte kunagi peaks sisaldama andmeid, mis võivad olla kaotsi.  

## <a name="testing-your-apps"></a>Rakenduste testimine
Kasutage neid juhiseid testimine rakendused.

1.  Windows Server 2012 R2 ja rakenduse installimine
2.  Luba kaugtöölaud
3.  Looge kaks Kasutajakontod UserA ja UserB, mõlemad kasutajakontode kaugtöölaua turberühma.
4.  Kontrollige ühilduvust mitme seansi, luues kaks samaaegne RDS seansid arvuti ajal käivitada rakendusi.
5.  Kinnitage rakenduse käitumine

## <a name="application-development-guidelines"></a>Rakenduste arendamise juhised
RemoteApp jaoks rakenduste arendamise pidage silmas järgmisi juhiseid.

### <a name="multiple-users"></a>Mitu kasutajat

- [Ühe kasutaja jaoks rakenduste ](https://msdn.microsoft.com/library/aa380661.aspx)installimist saate luua probleemide kasutajaga keskkonnas.
- Rakenduste tuleks [salvestada kasutaja teavet](https://msdn.microsoft.com/library/aa383452.aspx) kindla kasutaja asukohad, eraldi globaalne teave, mis kehtib kõigile kasutajatele.
- RemoteApp kasutab mitu [nimeruumid tuum objekte](https://msdn.microsoft.com/library/aa382954.aspx); Globaalne nimeruum kasutatakse peamiselt teenuste klient rakendustes.
- See pole turvaliste eeldatakse, et arvuti nimi või määratud arvuti [IP-aadress](https://msdn.microsoft.com/library/aa382942.aspx) on seostatud ühe kasutaja, kuna mitu kasutajat saab korraga sisselogitud kaugtöölaua seansi hosti (RD seansi hosti) serveris.

### <a name="performance"></a>Jõudluse
- Keelake [graafika efektid](https://msdn.microsoft.com/library/aa380822.aspx) enne, kui lisate oma rakenduse RemoteApp.
- CPU-saadavus kõigi kasutajate maksimeerimiseks keelake [tausttoimingud](https://msdn.microsoft.com/library/aa380665.aspx) või looge tõhusa tausttoimingud, mis pole ressursse.
- Peaksite häälestamine ja jääk rakenduse [jutulõnga kasutus](https://msdn.microsoft.com/library/aa383520.aspx) kasutajaga, mitmeprotsessoriliste keskkonnas.
- Jõudluse optimeerimiseks on hea tava rakenduste [tuvastamiseks](https://msdn.microsoft.com/library/aa380798.aspx) , kas need töötavad kliendi seansi.
