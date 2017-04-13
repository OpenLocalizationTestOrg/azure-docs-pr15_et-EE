### <a name="prerequisites"></a>Eeltingimused
- Azure'i konto; [tasuta konto](https://azure.microsoft.com/free) loomisel
- [Azure'i bloobimälu konto](../articles/storage/storage-create-storage-account.md) salvestusruumikonto nimi ja selle juurdepääsu võti. See teave on loetletud atribuutide Azure portaali konto salvestusruumi. Lugege lisateavet [Azure Storage](../articles/storage/storage-introduction.md).

Enne konto Azure'i bloobimälu loogika rakenduse kasutamisel Azure'i bloobimälu kontoga ühendada. Saate teha seda hõlpsalt oma loogika rakenduses Azure'i portaalis.  

Kontoga ühenduse loomiseks Azure'i bloobimälu tehes järgmist:  

1. Loogika rakenduse loomine. Designeris loogika rakenduste käivitamiseks lisada, ja seejärel lisage toimingu. Valige rippmenüüst **Kuva Microsoft hallatav API-d** , ja sisestage otsinguväljale "bloobimälu". Tehke toimingud:  

    ![Azure'i bloobimälu ühenduse loomise etappi](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  

2. Kui te pole varem loonud kõik ühendused Azure'i salvestusruumi, küsitakse teilt ühenduse üksikasju:   

    ![Azure'i bloobimälu ühenduse loomise etappi](./media/connectors-create-api-azureblobstorage/connection-details.png)  

3. Sisestage salvestusruumi konto üksikasjad. Tärniga atribuudid on nõutav.

    | Atribuut | Üksikasjad |
|---|---|
| Ühenduse nimi * | Sisestage mis tahes ühenduse nimi. |
| Azure'i Salvestusruumikonto nimi * | Sisestage salvestusruumikonto nimi. Azure'i portaalis salvestusruumi atribuudid kuvatakse salvestusruumikonto nimi. |
| Azure'i salvestusruumi konto kiirklahv * | Sisestage salvestusruumi konto võti. Kiirklahvide kuvatakse salvestusruumi atribuutide Azure'i portaalis. |

    Neid mandaate ühenduse loomiseks ja teie andmetele juurde pääseda rakenduse loogika autoriseerida. 

4. Valige **Loo**.

5. Pange tähele, et ühendus on loodud. Nüüd jätkake loogika rakenduse juhiseid: 

    ![Azure'i bloobimälu ühenduse loomise etappi](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
