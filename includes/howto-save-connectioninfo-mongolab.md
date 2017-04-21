See on võimalik MongoLab URI oma koodi kleepimiseks, soovitame seadistamine haldamise hõlbustamiseks keskkonnas. Sellisel viisil kui URI muutub, saate värskendada seda Azure portaali kaudu ilma koodi.


1. Azure'i portaalis valige **Web Apps**.
1. Klõpsake loendis veebirakenduste veebirakenduse nime.  
![WebAppEntry][entry-website]  
Kuvab veebirakenduse armatuurlaud.

1. Klõpsake menüüribal nuppu **Konfigureeri** .  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Liikuge kerides jaotiseni ühendusstringi.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. Sisestage **nimi**, MONGOLAB_URI.
1. **Väärtus**, kleepige me saadud eelmises jaotises ühendusstring.
1. Valige loendist **Tüüp** drop-down (vaikimisi **SQLAzure**) asemel **kohandatud** .
1. Klõpsake tööriistaribal nuppu **Salvesta** .  
![SaveWebApp][button-website-save]

**Märkus:** Azure'i lisab selle **CUSTOMCONNSTR\_ ** eesliide muutuja, miks koodi viited kohal **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png
