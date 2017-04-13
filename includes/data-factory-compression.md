### <a name="compression-support"></a>Tihendamise tugi  
Suurte andmekogumite töötlemine, võivad põhjustada SV- ja võrgu kitsaskohtade. Seetõttu salvestab tihendatud andmete saate mitte ainult kiirendamiseks Andmeedastus üle võrgu ja säästa kettaruumi, kuid ka too big andmete töötlemise olulisi jõudlustäiustusi. Praegu on toetatud tihendamise failipõhiste andmete poed nagu Azure'i bloobimälu või kohapealse failisüsteemis.  

> [AZURE.NOTE] Andmete **AvroFormat**, **OrcFormat**või **ParquetFormat**tihendussätted ei toetata. 

Tihendamise andmepaani, saate määrata atribuudi **tihendamise** andmekogumis JSON, nagu järgmises näites:   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
**Tihendamine** jaotises on kaks atribuudid.  
  
- **Tüüp:** tihendamise kodekiga, mis võib olla **GZIP**, **Deflate** või **BZIP2**.  
- **Tase:** tihendamise-suhte, mis võib olla **optimaalne** või **kiireim**. 
    - **Kiireim:** Tihendamise toiming tuleb täita nii kiiresti kui võimalik, isegi juhul, kui fail on tihendatud optimaalselt. 
    - **Optimaalne**: tihendamise toiming peaks olema optimaalselt tihendatud, isegi kui toiming võtab rohkem aega. 
    
    Lisateavet leiate teemast [Tihendamise tase](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) . 

Oletame, et ülaltoodud valimi andmekomplekti kasutatakse Kopeeri tegevuse väljundina, Kopeeri tegevuse tihendab väljundi andmed koos GZIP kodekiga suhe optimaalse kasutamise ja kirjutage siis tihendatud andmete faili, pagecounts.csv.gz Azure'i bloobimälu sisse.   

Kui määrate atribuudi tihendamise mõne Sisestuskeel andmekomplekti JSON, tulemas saate lugeda tihendatud andmete lähtekoha ja määrate atribuudi mõne väljundi andmekomplekti JSON, kopeerige tegevuse saate kirjutada tihendatud andmete sihtkohta. Siin on mõned valimi stsenaariumid. 

- Lugege GZIP tihendatud andmete mõne Azure'i bloobimälu lahti see ja kirjutage tulemi andmed Azure SQL-andmebaasi. Saate määratleda Sisestuskeel Azure'i bloobimälu andmekomplekti koos tihendamise JSON atribuudi sel juhul. 
- Lihttekstifail kohapealse failisüsteemi kaudu andmeid lugeda, tihendada, kasutades GZip vorming ja kirjutamise tihendatud andmete maht on Azure'i bloobimälu. Määratlege soovitud väljund Azure'i bloobimälu andmekomplekt on tihendamise JSON atribuut sel juhul.  
- GZIP tihendatud andmeid lugeda mõne Azure'i bloobimälu, lahti see, kasutades BZIP2 tihendamine ja kirjutamise tulemi andmed on Azure'i bloobimälu. Saate määratleda Sisestuskeel Azure'i bloobimälu andmekomplekti seadistatud GZIP ja väljundi andmekomplekti tihendamise tüüp väärtuseks BZIP2 sel juhul tihendamise tüübiga.   
