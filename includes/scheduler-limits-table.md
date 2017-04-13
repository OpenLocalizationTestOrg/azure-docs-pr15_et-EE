Järgmises tabelis kirjeldatakse iga põhi piirmäära, piirangud, vaikesätted ja Azure ajasti pidurdab.

|Ressurss|Piiratud kirjeldus|
|---|---|
|**Töö suurus**|Töö maksimaalne maht on 16K. Kui panna või paik töö, mis on suurem kui need piirangud, tagastatakse 400 vigane päring olekukoodi.|
|**Taotluse URL-i suurus**|URL-i taotluse maksimaalne maht on 2048 märki.|
|**Aggregate päise suurus**|Suurim lubatud liitväärtuse päise maht on 4096 tähemärki.|
|**Päise loendamine**|Päise maksimaalne arv on 50 päised.|
|**Sisu maht**|Suurim lubatud sisu maht on 8192 märki.|
|**Korduvus ajavahemiku**|Suurim lubatud Korduvus ajavahemiku on 18 kuud.|
|**Aeg alguskellaaeg**|"Aeg alguskellaaeg" maksimumväärtus on 18 kuud.|
|**Töö ajalugu**|Suurim lubatud vastuse keha talletatud varasem töökogemus on 2048 baiti.|
|**Frequency**|Vaikimisi max sagedus kvoodi on 1 tund tasuta töö kogumi ja 1 minuti standardne töö kogumi. Max sagedus on konfigureeritav töö kogumise olema väiksem kui maksimumväärtus. Kõik tööd on töö kogumis on piiratud töö saidikogumi väärtus. Kui proovite luua tööd sagedamini kui lubatud sagedus töö kogumise seejärel taotlus ei õnnestu 409 konflikti olekukoodi.|
|**Tööde haldamine**|Vaikimisi max töö piirmäär on 5 töökohta tasuta töö kogumi ja 50 töökohta standardne töö kogumi. Töö maksimumarv on konfigureeritav töö saidikogumi kohta. Kõik tööd on töö kogumis on piiratud töö saidikogumi väärtus. Kui proovite luua rohkemate kui ülempiir töökohtade kvoodi, siis taotluse nurjub 409 konflikti olekukoodi.|
|**Ajaloo säilitamine**|Töö ajalugu alles, kuni 2 kuud või kuni viimase 1000 täitmised.|
|**Lõpule viidud ja tehtud töö säilitus.**|Täidetud ja tehtud töö säilitatakse 60 päeva jooksul.|
|**Ajalõpp**|On staatiline (mitte konfigureeritav) taotluse ajalõpp 60 sekundi HTTP toimingute jaoks. Rohkem esitatava toimingute puhul järgige HTTP asünkroonne Protokollid; näiteks tagastada on 202 kohe, kuid jätkata tööd taustal.|
