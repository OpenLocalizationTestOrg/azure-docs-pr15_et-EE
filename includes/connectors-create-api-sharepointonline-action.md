Nüüd, kui olete lisanud käivitamiseks, huvitav andmetega, mis on loodud käivitab selle aja midagi teha. Järgige **SharePoint Online'i - faili loomiseks** toimingu lisamiseks tehke järgmist. See toiming loob faili SharePoint Online'i iga kord, kui uue üksuse päästik käivitub. 

Konfigureerida see toiming on vaja järgmist teavet. Seda teavet, te teate, et see on loodud päästiku sisendina uue faili atribuutide mõne lihtsa andmete:

|Faili atribuudi loomine|Kirjeldus|
|---|---|
|Saidi URL-i|See on SharePoint Online'i saidil, kus soovite luua uue faili URL-i. Valige loendis esitatud saidile.|
|Kausta tee|See on kaust (veebisaidil saidi URL-i eelmises juhises valitud) kuhu paigutatakse uue faili. Otsige ja valige kaust.|
|Faili nimi|See on loodava faili nimi.|
|Faili sisu|Mis on kirjutatud faili sisu.|

1. Valige **+ uus samm** toimingu lisamiseks.  
![SharePoint Online'i toimingu pilt 1](./media/connectors-create-api-sharepointonline/action-1.png)  
- Valige link **Lisa toiming** . See avatakse välja Otsing, kus saate otsida mis tahes toimingu te soovite teha. Selle näite puhul on huvi SharePointi toimingud.    
![SharePoint Online'i toimingu pilt 2](./media/connectors-create-api-sharepointonline/action-2.png)    
- Sisestage *SharePointi* otsimiseks SharePointi seotud toiminguid.
- **SharePoint Online'i - faili loomiseks** valige toiming.   **Märkus**: teil palutakse lubada loogika rakenduse SharePoint kontole juurdepääsuks, kui te pole loonud ühenduse SharePoint Online'i varem.    
![SharePoint Online'i toimingu pilt 3](./media/connectors-create-api-sharepointonline/action-3.png)    
- Juhtelemendi **loomine fail** avatakse.   
![SharePoint Online'i toimingu pilt 4](./media/connectors-create-api-sharepointonline/action-4.png)     
- Valige **Saidi URL** ja sirvides üles saidil, kuhu soovite faili luua.     
![SharePoint Online'i toimingu pilt 5](./media/connectors-create-api-sharepointonline/action-5.png)  
- Valige **kausta asukoht** ja sirvides üles kaust, kuhu paigutatakse uue faili.  
![SharePoint Online'i toimingu pilt 6](./media/connectors-create-api-sharepointonline/action-6.png)  
- Valige juhtelement, **faili nimi** ja Sisestage loodava faili nimi. Siin saate sisestada faili nime otse või saate kasutada atribuute: päästik, mis on varem loodud. Tehke seda, valides atribuutide loendi **väljundid uue üksuse loomisel**. See loend on ainult ekraani pärast seda, kui valite **faili nimi** juhtelementi. Klõpsake selle walkthough valitud ID (uue loendiüksuse ID) nimega fail on loodud **SharePoint Online'i - faili loomiseks** toimingu nimi.    
![SharePoint Online'i toimingu pilt 7](./media/connectors-create-api-sharepointonline/action-7.png)  
- Valige **faili sisu** juhtelement ja sisestage soovitud sisu on kirjutatud faili, mis luuakse. Faili sisu, Pange tähele, et saate atribuute: päästik, mis on varem loodud. Lihtsalt valige loendis esitatud atribuudid. Teise võimalusena saate sisestada **faili sisu** teksti otse juhtelementi. Selles näites valitud osa atribuute ja tühikuid ja sidekriipsu vahel iga atribuudi lisanud.        
![SharePoint Online'i toimingu pilt 8](./media/connectors-create-api-sharepointonline/action-8.png)  
- Muudatuste salvestamiseks oma töövoo  
- Palju õnne, siis nüüd on täielikult toimiv loogika rakendus, mis käivitab uue üksuse lisamisel SharePoint Online'i loend. Rakendus loob faili, mõne uue loendi üksuse atribuutide abil.  Nüüd saate selle testimiseks uue üksuse loomine SharePointi loendis. 
