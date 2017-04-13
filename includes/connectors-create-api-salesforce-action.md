Nüüd, kui olete lisanud tingimus, huvitav andmetega, mis on loodud käivitab oma aega midagi teha. Järgige **Salesforce'i - objekti toomine** toimingu lisamiseks tehke järgmist. Seda toimingut saavad andmeid iga kord, kui luuakse uus müügivihje. Teine toiming, Salesforce - Get objekti toiming ja saatke meil Office 365 kasutamisega andmeid kasutavate ka tuleb lisada.  

Konfigureerida see toiming on vaja järgmist teavet. Te teate, et see on loodud päästiku sisendina uue faili atribuutide mõne lihtsa andmete:

|Faili atribuudi loomine|Kirjeldus|
|---|---|
|Objekti tüüp|See on teid huvitab Salesforce'i objekti tüüp. Näited juhi konto, jne.|
|Objekti ID|See tähistab identifikaatori objekti.|


1. Valige link **Lisa toiming** . See avatakse välja Otsing, kus saate otsida mis tahes toimingu te soovite teha. Selle näite puhul on huvi Salesforce'i toimingud.      
![Salesforce'i toimingu pilt 1](./media/connectors-create-api-salesforce/action-1.png)  
- Sisestage *salesforce'i* otsimiseks salesforce'i seotud toiminguid.
- Valige **Salesforce'i - Get objekti** toiming.   **Märkus**: teil palutakse lubada rakenduse loogika Salesforce'i kontole juurdepääsuks, kui te pole seda teinud varem.    
![Salesforce'i toimingu pilt 2](./media/connectors-create-api-salesforce/action-2.png)    
- Avab **saada objekti** juhtelement.  
- Objekti tüüp valige *juhtimine* .
- Valige juhtelement, **Objekti ID -d** .
- Valige märgid, mida saab kasutada sisendina toimingute loendi laiendamiseks **…** .       
![Salesforce'i toimingu pilt 3](./media/connectors-create-api-salesforce/action-3.png)    
- Valige **Juhi** juhtelemendile avatakse.   
![Salesforce'i toimingu pilt 4](./media/connectors-create-api-salesforce/action-4.png)     
- Pange tähele, et viia ID luba on nüüd juhtelemendis objekti ID, mis näitab, et saada objekti toiming otsib müügivihje ID, mis on võrdne juhi loogika rakenduse käivitanud müügivihje ID-ga.  
![Salesforce'i toimingu pilt 5](./media/connectors-create-api-salesforce/action-5.png)  
- Salvestage oma töö. See on õige, olete lisanud oma loogika rakenduse toomine objekti toiming. Oma toomine objekti juhtelement peaks välja nägema umbes järgmine:    
![Salesforce'i toimingu pilt 6](./media/connectors-create-api-salesforce/action-6.png)  

Nüüd, kui olete lisanud toimingu kaasa, võite teha midagi huvitavat vastloodud viia. Ettevõte, võite saada e-posti leviloendi teatavad, et on loodud uue juhi. Vaatame saatmine e-posti olulise teabe mõned uue juhi objekti Salesforce'i konnektor Office 365 abil.  

1. Valige **Lisa toiming** ja seejärel sisestage *e-posti* otsingu juhtelemendi. See filtreerib toimingud, mis on seotud saata ja vastu võtta meilisõnumeid.  
- **Office 365 Outlook - e-posti saatmine** loendis üksuse valimine. Kui te pole juba loonud *ühenduse* oma Office 365 kontosse, palutakse sisestada mandaat Office 365 nüüd loomiseks. Kui olete lõpetanud, avab juhtelement **meilisõnumit saata** .        
![Salesforce'i toimingu pilt 7](./media/connectors-create-api-salesforce/action-7.png)  
- Sisestage meiliaadress, mida soovite saata meilisõnumeid juhtelemendi **abil** .
-  **Teema** juhtelemendi, sisestage *Uus juhtimine loodud* – valige Luba *ettevõtte* . See kuvab välja *ettevõte* Salesforce'i jaoks luua uue müügivihje põhjal.  
-  Juhtelemendi **sisu** , võite valida märkide uue juhi objektist ja võite sisestada ka mis tahes teksti, mida soovite meilisõnumi kehas kuvada. Siin on näide:  
![Salesforce'i toimingu pilt 8](./media/connectors-create-api-salesforce/action-8.png)   
- Salvestage oma töövoo.  

See on õige. Rakenduse loogika on lõppenud.  

Nüüd saate testida rakenduse loogika: looge Salesforce, mis vastab määratud tingimusele, mis on loodud uue müügivihje.  Kui järgisite selle walk-through täielikult, looge lihtsalt müügivihje e-posti aadressi, mis sisaldab *amazon.com* see. Mõne sekundi pärast loogika rakenduse käivitatakse ja tulemused võib välja näha umbes järgmine:  
![Salesforce'i toimingu pilt 9](./media/connectors-create-api-salesforce/action-9.png)  

