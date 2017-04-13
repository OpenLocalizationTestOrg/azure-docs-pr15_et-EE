## <a name="azure-storage-linked-service"></a>Azure'i salvestusruumi lingitud teenus

**Azure'i mäluruumi lingitud teenuse** võimaldab Azure storage konto linkimine on Azure andmete factory **konto võti**abil. See pakub andmete factory globaalse juurdepääsu Azure Storage. Järgmises tabelis on ära toodud JSON elementide teatud Azure Storage lingitud teenuse kirjeldus.

| Atribuut | Kirjeldus | Nõutav |
| :-------- | :----------- | :-------- |
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **AzureStorage** | Jah |
| connectionString | Määrake Azure salvestusruum atribuut connectionString ühenduse loomiseks vajalik teave. | Jah |

Leiate järgmistest artiklitest vaade/kopeerida konto võti üksikasjalikud juhised mõne Azure Storage: [Vaade, kopeerimine ja uuesti luua salvestusruumi kiirklahvide](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

**Näide:**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Azure'i salvestusruumi Sas lingitud teenus  
Ühiskasutusega juurdepääsu signatuuri (SAS) pakub delegeeritud juurdepääsu teie salvestusruumi konto ressursse. See tähendab, et saate anda mõnda muud klienti piiratud õiguste objektide konto salvestusruumi määratud perioodi ja määratud kogumi õigused ilma jagada oma konto kiirklahvide. MUUDE on URI, mis hõlmab selle päringu parameetrid kogu vajaliku teabe autenditud juurdepääsu ressursile salvestusruumi. Salvestusruumi ressursid koos muude juurdepääsu kliendi ainult tuleb läbida muude vastava konstruktori või meetod. Üksikasjalikku teavet SAS, lugege teemat [ühiskasutusse Accessi allkirjad: SAS andmemudeli mõistmine](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
Azure'i mäluruumi SAS lingitud teenuse võimaldab Azure'i salvestusruumi konto linkimine on Azure andmete factory abil ühiskasutusse Accessi allkirja (SAS). See pakub andmete factory piiratud/ajalised juurdepääsu talletamist kõik/konkreetset ressurssidele (bloobimälu/container). Järgmises tabelis on ära toodud JSON elemendid konkreetsed Azure'i salvestusruumi SAS lingitud teenuse kirjeldus. 

| Atribuut | Kirjeldus | Nõutav |
| :-------- | :----------- | :-------- |
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **AzureStorageSas**  | Jah |
| sasUri | Määrata ühiskasutusse antud juurdepääs allkirja URI Azure Storage ressursse, nt bloobimälu, container või tabel. Lisateavet all asuvate märkmete vaatamiseks. | Jah | 


**Näide:**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

Luues sellise **SAS URI**, võttes arvesse järgmist:  

- Azure'i andmed Factory toetab ainult **Teenuse SAS**, mitte konto SAS. [Tüüpi ühiskasutusse Accessi allkirjade](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) kohta vaadake teavet kahte tüüpi.
- Sobiv lugemis-ja kirjutamisõigusega **õiguste** vaja olema määratud objektid, kuidas lingitud teenuse (lugemine, kirjutamine, lugemis-ja kirjutamisõigusega) kasutatakse teie andmete factory põhjal.
- **Kehtivuse aeg** peab olema õigesti seatud. Veenduge, et juurdepääs Azure Storage objekte ei lõpe tulemas aktiivse aja jooksul.
- URI luuakse õige container/bloobimälu või tabeli tasandil vastavalt vajadusele. SAS Uri, et mõne Azure'i bloobimälu võimaldab juurde pääseda selle kindla bloobimälu teenuse andmete Factory. SAS Uri, et mõne Azure'i bloobimälu container võimaldab andmete Factory teenuse kordamiseks plekid selle ümbrises kaudu. Kui teil on vaja juurdepääsu rohkem/vähem objektide hiljem, või värskendada SAS URI, pidage meeles värskendada lingitud teenuse uue URI-ga.   
