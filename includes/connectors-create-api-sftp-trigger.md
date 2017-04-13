Lisada käivitamiseks.

1. Sisestage otsinguväljale *sftp* loogika rakenduste designer, seejärel valige **SFTP – kui fail on lisatud või muudetud** päästik   
![SFTP päästik pilt 1](./media/connectors-create-api-sftp/trigger-1.png)  
- **Kui fail on lisatud või muudetud** juhtelemendi avab  
![SFTP päästik pilt 2](./media/connectors-create-api-sftp/trigger-2.png)  
- Valige **...** asuv juhtelement paremas servas. Avatakse kaust Kuupäevavalija juhtelement  
![SFTP päästik pilt 3](./media/connectors-create-api-sftp/action-1.png)  
- Valige **SFTP** juurkausta jälgimiseks uusi või muudetud failide kausta valimiseks. Nüüd kuvatakse teade juurkausta **kausta** juhtelemendis.  
![SFTP päästik pilt 4](./media/connectors-create-api-sftp/action-2.png)   

Selles etapis rakenduse loogika on konfigureeritud päästik, mis algab kestab päästikute ja töövoo toimingutes, kui faili on muudetud või loodud teatud SFTP kausta. 

>[AZURE.NOTE]Loogika rakenduse olema funktsionaalne, peab sisaldama vähemalt ühte päästik ja ühe toimingu. Toimingu lisamiseks järgmise jaotise juhised.  