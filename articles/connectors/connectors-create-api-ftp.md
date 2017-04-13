<properties
pageTitle="Saate teada, kuidas kasutada FTP konnektor loogika rakendustes | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. Failide haldamiseks FTP serveriga ühendust luua. Saate sooritada erinevaid toiminguid, näiteks üles, värskendada, leida ja kustutada FTP-server."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/22/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-ftp-connector"></a>Alustamine FTP konnektor

Konnektor FTP abil saate jälgida, hallata ja luua faile FTP-serveris. 

[Mis tahes konnektor](./apis-list.md)kasutamiseks peate esmalt loogika rakenduse loomine. Saate alustada, [luues loogika rakenduse kohe](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-ftp"></a>Ühenduse loomine FTP

Enne oma loogika rakenduse juurdepääsu mis tahes teenuse, peate esmalt teenusega *ühenduse* loomiseks. [Ühenduse](./connectors-overview.md) pakub Ühenduvus loogika rakendus ja muu teenus.  

### <a name="create-a-connection-to-ftp"></a>FTP ühenduse loomine

>[AZURE.INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]

## <a name="use-a-ftp-trigger"></a>Kasutage FTP päästik

Käivitamiseks on sündmus, mida saab kasutada määratletud loogika rakenduse töövoo käivitamiseks. [Lisateavet päästikute kohta](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.IMPORTANT]FTP konnektor nõuab FTP-server, mis on kättesaadavad Interneti kaudu ja on konfigureeritud passiivset töötamiseks. Samuti on FTP konnektor **ei ühildu peidetud FTPS (FTP SSL)**. FTP konnektor toetab ainult konkreetsete FTPS (FTP-SSL).  

Selles näites näitab teile, kuidas kasutada **FTP - kui fail on lisatud või muudetud** päästik loogika rakenduse töövoo käivitamiseks faili lisamisel või muutmine serverist. Näiteks ettevõtte saate kasutada seda päästik jälgida FTP-kaust uusi faile, mis tähistavad klientide tellimused.  Võib seejärel abil saate FTP konnektor toimingu nagu **faili sisu** edasiseks töötlemiseks ja salvestusruumi sisu andmebaasi tellimused.

1. Sisestage otsinguväljale *ftp* loogika rakenduste designer, seejärel valige **FTP - kui fail on lisatud või muudetud** päästik   
![FTP päästik pilt 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
**Kui fail on lisatud või muudetud** juhtelemendi avab  
![FTP päästik pilt 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
- Valige **...** juhtelemendi paremal asub. Avatakse kaust Kuupäevavalija juhtelement  
![FTP päästik pilt 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
- Valige soovitud **>** (paremnool) ja Sirvi, et leida kaust, kus soovite jälgida uusi või muudetud failid. Valige kaust ja pöörake tähelepanu sellele, kausta, kuvatakse **kausta** juhtelementi.  
![FTP päästik pilt 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   


Selles etapis rakenduse loogika on konfigureeritud päästik, mis algab kestab päästikute ja töövoo toimingutes, kui faili on muudetud või loodud teatud FTP kausta. 

>[AZURE.NOTE]Loogika rakenduse olema funktsionaalne, peab sisaldama vähemalt ühte päästik ja ühe toimingu. Toimingu lisamiseks järgmise jaotise juhised.  



## <a name="use-a-ftp-action"></a>Kasutage FTP toimingut

Toimingu on poolt määratletud loogika rakenduse töövoo toimingu. [Lisateavet leiate teemast toimingute kohta](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

Nüüd, kui olete lisanud käivitamiseks, järgige toimingu, mis käivitab leitud uusi või muudetud faili sisu lisamiseks tehke järgmist.    

1. Valige **+ uus samm** lisamiseks soovitud toimingu saada sisu faili FTP-serveris  
- Valige link **Lisa toiming** .  
![FTP toimingu pilt 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
- Sisestage *FTP* kõik toimingud, mis on seotud FTP otsimiseks.
- Valige toiming uusi või muudetud faili leidmisel FTP kausta **FTP - faili sisu** .      
![FTP toimingu pilt 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
Avab **faili sisu** juhtelement. **Märkus**: teil palutakse lubada juurdepääsu FTP-serveri konto, kui te pole seda teinud varem rakenduse loogika.  
![FTP toimingu pilt 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
- Valige juhtelement, **File** ( **fail**all asuv tühimiku *). Siin saate kasutada mitmesuguste atribuute uusi või muudetud faili FTP-serveris.  
- Valige suvand **faili sisu** .  
![FTP toimingu pilt 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
-  Juhtelemendi on värskendatud, mis näitab, et **FTP - faili sisu** toimingu saavad selle *faili sisu* uusi või muudetud faili FTP-serveris.      
![FTP toimingu pilt 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
- Salvestage oma töö ja seejärel faili lisamine oma töövoo testimiseks FTP kausta.    

Selles etapis loogika rakendus on konfigureeritud käivitamiseks jälgida kausta FTP-serveris ja algatada töövoo, kui see leiab uue faili või muudetud faili FTP-serveris. 

Ka loogika rakendus on konfigureeritud toimingu saada uusi või muudetud faili sisu.

Nüüd saate lisada mõne muu toimingu näiteks [SQL serveri - rea lisamine](./connectors-create-api-sqlazure.md#insert-row) toimingu lisada uusi või muudetud faili sisu SQL-andmebaasi tabelisse.  

## <a name="technical-details"></a>Tehnilised andmed

Siin on päästikute, toiminguid ja vastuseid, mis toetab sellega üksikasjad.

## <a name="ftp-triggers"></a>Päästikute FTP

FTP on järgmine trigger(s).  

|Päästik | Kirjeldus|
|--- | ---|
|[Kui fail on lisatud või muudetud](connectors-create-api-ftp.md#when-a-file-is-added-or-modified)|See toiming käivitab vool, kui fail on lisatud või muudetud kausta.|


## <a name="ftp-actions"></a>FTP toimingud

FTP on järgmised toimingud.


|Toiming|Kirjeldus|
|--- | ---|
|[Hankida faili metaandmed](connectors-create-api-ftp.md#get-file-metadata)|Selle toimingu saab faili metaandmed.|
|[Update-faili](connectors-create-api-ftp.md#update-file)|See toiming värskendab faili.|
|[Faili kustutamine](connectors-create-api-ftp.md#delete-file)|See toiming kustutab faili.|
|[Faili metaandmete teed kasutades hankimine](connectors-create-api-ftp.md#get-file-metadata-using-path)|Selle toimingu saab metaandmete faili tee kaudu.|
|[Faili sisu teed kasutades](connectors-create-api-ftp.md#get-file-content-using-path)|Selle toimingu saab faili tee sisu.|
|[Faili sisu](connectors-create-api-ftp.md#get-file-content)|Selle toimingu saab faili sisu.|
|[Faili loomine](connectors-create-api-ftp.md#create-file)|See toiming loob faili.|
|[Faili kopeerimine](connectors-create-api-ftp.md#copy-file)|Selle toimingu kopeerib faili serverist.|
|[Kausta failide loend](connectors-create-api-ftp.md#list-files-in-folder)|Selle toimingu saab failid ja alamkaustad kausta loendit.|
|[Juurkausta failide loend](connectors-create-api-ftp.md#list-files-in-root-folder)|Selle toimingu saab failid ja alamkaustad juurkaustas loendit.|
|[Kausta eraldamiseks](connectors-create-api-ftp.md#extract-folder)|Selle toimingu ekstraktib arhiivifaili kausta (näide: zip).|
### <a name="action-details"></a>Toimingu üksikasjad

Siin on toimingud üksikasjad ja käivitab selle konnektor koos vastustega.



### <a name="get-file-metadata"></a>Hankida faili metaandmed
Selle toimingu saab faili metaandmed. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|ID *|Faili|Valige fail|

Mõne * näitab, et atribuut on nõutav

#### <a name="output-details"></a>Väljundi üksikasjad

BlobMetadata


| Atribuudi nimi | Andmetüüp |
|---|---|---|
|ID|string|
|Nimi|string|
|DisplayName|string|
|Tee|string|
|LastModified|string|
|Suurus|täisarv|
|Meediumitüübil|string|
|IsFolder|kahendmuutuja|
|ETag|string|
|FileLocator|string|




### <a name="update-file"></a>Update-faili
See toiming värskendab faili. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|ID *|Faili|Valige fail|
|kehateksti *|Faili sisu|Faili sisu|

Mõne * näitab, et atribuut on nõutav

#### <a name="output-details"></a>Väljundi üksikasjad

BlobMetadata


| Atribuudi nimi | Andmetüüp |
|---|---|---|
|ID|string|
|Nimi|string|
|DisplayName|string|
|Tee|string|
|LastModified|string|
|Suurus|täisarv|
|Meediumitüübil|string|
|IsFolder|kahendmuutuja|
|ETag|string|
|FileLocator|string|




### <a name="delete-file"></a>Faili kustutamine
See toiming kustutab faili. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|ID *|Faili|Valige fail|

Mõne * näitab, et atribuut on nõutav




### <a name="get-file-metadata-using-path"></a>Faili metaandmete teed kasutades hankimine
Selle toimingu saab metaandmete faili tee kaudu. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tee *|Faili tee|Valige fail|

Mõne * näitab, et atribuut on nõutav

#### <a name="output-details"></a>Väljundi üksikasjad

BlobMetadata


| Atribuudi nimi | Andmetüüp |
|---|---|---|
|ID|string|
|Nimi|string|
|DisplayName|string|
|Tee|string|
|LastModified|string|
|Suurus|täisarv|
|Meediumitüübil|string|
|IsFolder|kahendmuutuja|
|ETag|string|
|FileLocator|string|




### <a name="get-file-content-using-path"></a>Faili sisu teed kasutades
Selle toimingu saab faili tee sisu. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tee *|Faili tee|Valige fail|

Mõne * näitab, et atribuut on nõutav




### <a name="get-file-content"></a>Faili sisu
Selle toimingu saab faili sisu. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|ID *|Faili|Valige fail|

Mõne * näitab, et atribuut on nõutav




### <a name="create-file"></a>Faili loomine
See toiming loob faili. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|folderPath *|Kausta tee|Valige kaust|
|nimi *|Faili nimi|Faili nimi|
|kehateksti *|Faili sisu|Faili sisu|

Mõne * näitab, et atribuut on nõutav

#### <a name="output-details"></a>Väljundi üksikasjad

BlobMetadata


| Atribuudi nimi | Andmetüüp |
|---|---|---|
|ID|string|
|Nimi|string|
|DisplayName|string|
|Tee|string|
|LastModified|string|
|Suurus|täisarv|
|Meediumitüübil|string|
|IsFolder|kahendmuutuja|
|ETag|string|
|FileLocator|string|




### <a name="copy-file"></a>Faili kopeerimine
Selle toimingu kopeerib faili serverist. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|andmeallika *|Lähte-url|URL-i lähtefail|
|sihtkoha *|Sihtkoha faili tee|Sihtkoha faili tee, sh sihtrakenduse nimi|
|ülekirjutamine|Kirjutada?|Sihtfail kirjutatakse üle, kui on seatud "true"|

Mõne * näitab, et atribuut on nõutav

#### <a name="output-details"></a>Väljundi üksikasjad

BlobMetadata


| Atribuudi nimi | Andmetüüp |
|---|---|---|
|ID|string|
|Nimi|string|
|DisplayName|string|
|Tee|string|
|LastModified|string|
|Suurus|täisarv|
|Meediumitüübil|string|
|IsFolder|kahendmuutuja|
|ETag|string|
|FileLocator|string|




### <a name="when-a-file-is-added-or-modified"></a>Kui fail on lisatud või muudetud
See toiming käivitab vool, kui fail on lisatud või muudetud kausta. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|kausta ID *|Kausta|Valige kaust|

Mõne * näitab, et atribuut on nõutav




### <a name="list-files-in-folder"></a>Kausta failide loend
Selle toimingu saab failid ja alamkaustad kausta loendit. 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|ID *|Kausta|Valige kaust|

Mõne * näitab, et atribuut on nõutav



#### <a name="output-details"></a>Väljundi üksikasjad

BlobMetadata


| Atribuudi nimi | Andmetüüp |
|---|---|---|
|ID|string|
|Nimi|string|
|DisplayName|string|
|Tee|string|
|LastModified|string|
|Suurus|täisarv|
|Meediumitüübil|string|
|IsFolder|kahendmuutuja|
|ETag|string|
|FileLocator|string|




### <a name="list-files-in-root-folder"></a>Juurkausta failide loend
Selle toimingu saab failid ja alamkaustad juurkaustas loendit. 


Pole selle kõne parameetrid

#### <a name="output-details"></a>Väljundi üksikasjad

BlobMetadata


| Atribuudi nimi | Andmetüüp |
|---|---|---|
|ID|string|
|Nimi|string|
|DisplayName|string|
|Tee|string|
|LastModified|string|
|Suurus|täisarv|
|Meediumitüübil|string|
|IsFolder|kahendmuutuja|
|ETag|string|
|FileLocator|string|




### <a name="extract-folder"></a>Kausta eraldamiseks
Selle toimingu ekstraktib arhiivifaili kausta (näide: zip). 


|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|andmeallika *|Andmeallika arhiivi faili tee|Arhiivi faili tee|
|sihtkoha *|Sihtkausta tee|Sihtkausta tee|
|ülekirjutamine|Kirjutada?|Sihtkoha failid kirjutatakse üle, kui on seatud "true"|

Mõne * näitab, et atribuut on nõutav



#### <a name="output-details"></a>Väljundi üksikasjad

BlobMetadata


| Atribuudi nimi | Andmetüüp |
|---|---|---|
|ID|string|
|Nimi|string|
|DisplayName|string|
|Tee|string|
|LastModified|string|
|Suurus|täisarv|
|Meediumitüübil|string|
|IsFolder|kahendmuutuja|
|ETag|string|
|FileLocator|string|



## <a name="http-responses"></a>HTTP vastused

Toimingute ja käivitab ülaltoodud saate tagastavad ühte või mitut HTTP olek järgmisi koode. 

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|202|Aktsepteeritud|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge.|
|Vaikimisi|Toiming nurjus.|







## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)