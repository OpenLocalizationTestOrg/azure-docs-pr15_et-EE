Nüüd, kui olete lisanud käivitamiseks, huvitav andmetega, mis on loodud käivitab selle aja midagi teha. Järgmiste juhiste abil saate lisada soovitud toimingu **SFTP - ekstrakti kausta** . See toiming on faili sisu ekstraktimiseks, kui määratletud tingimused on täidetud. 

Konfigureerida see toiming on vaja järgmist teavet. Te teate, et see on loodud päästiku sisendina uue faili atribuutide mõne lihtsa andmete:

|SFTP - ekstrakti kausta atribuut|Kirjeldus|
|---|---|
|Andmeallika arhiivi faili tee|See on on ekstraktimist faili tee. Saate valige üks märkide varasemates toimingu või SFTP server faili tee otsimiseks Sirvi.|
|Sihtkausta tee|See on tee, kuhu ekstraktitud failide paigutatakse. Valige üks märkide varasemates toimingu sihtkoha tee või sirvige SFTP server ja valige tee.|
|Kirjutada?|Näitab, kui fail on sama nimi ekstraktitud fail on leitud Sihtkausta tee kui olemasolev fail kirjutatakse või mitte.|

Alustagem lisamise toimingu ekstrakti failid, kui varem määratletud avaldise väärtuseks *True*. 

1. Valige **Lisa toiming**.        
![SFTP toimingu tingimus pilt 6](./media/connectors-create-api-sftp/condition-6.png)   
- Valige toiming **SFTP - ekstrakti kaust**      
![SFTP toimingu tingimus pilt 7](./media/connectors-create-api-sftp/condition-7.png)   
- Valige **Allikas arhiivi faili tee**              
![SFTP toimingu tingimus pilt 9](./media/connectors-create-api-sftp/condition-9.png)   
- Valige luba **faili tee** . See näitab, et kasutate faili tee faili, mis käivitab leitud nimega allika arhiivi faili tee.           
![SFTP toimingu tingimus pilt 10](./media/connectors-create-api-sftp/condition-10.png)   
- Valige **Sihtkausta tee**           
![SFTP toimingu tingimus pilt 11](./media/connectors-create-api-sftp/condition-11.png)   
- Valige luba **faili tee** . See näitab, et kasutate faili tee faili, mis käivitab leitud sihtkoha tee ekstraktitud failide.   
- Sisestage *\ExtractedFile* **Sihtkausta tee** juhtelementi. Tehke seda kohe pärast faili tee luba juhtelemendis sihtkoht kausta tee.         
![SFTP toimingu tingimus pilt 12](./media/connectors-create-api-sftp/condition-12.png)   
- Sisestage *True* on **kirjuta?* juhtelement, mis näitab, et olemasolevaid faile peaks üle, kui neil on sama nimi nagu ekstraktitud failide.      
![SFTP toimingu tingimus pilt 13](./media/connectors-create-api-sftp/condition-13.png)   
- Muudatuste salvestamiseks oma töövoo  
