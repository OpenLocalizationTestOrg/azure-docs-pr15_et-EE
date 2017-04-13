## <a name="dynamic-service-plan"></a>Dünaamiliste teenusleping

Dünaamiline teenuse kavas, määratakse funktsioon rakenduste funktsioon rakenduse eksemplari. Kui vaja mitu eksemplari lisatakse dünaamiliselt.
Juhtudel võib ühendada mitu ressursside parimal viisil saadaval Azure taristu tegemist üle. Lisaks teie funktsioonid käivitatakse paralleelselt minimeerimine taotlused kokku aega. Iga funktsiooni täitmisaeg liita sekundites, ja sisaldavad funktsioon rakenduse abil kokku liita. Koos maksumus juhitud eksemplari, tema mälu suurus ja kokku täitmisaeg arvuga mõõta gigabait sekundites. See on suurepärane võimalus, kui teie vajadustele Arvuta on katkendlik või teie kellaaegade töö on tavaliselt lühikese see võimaldab teil ainult maksta Arvuta ressursid, kui need on tegelikult kasutamine.   

### <a name="memory-tier"></a>Mälu taseme

Olenevalt teie funktsioon peab saate valida mälu kuvava neid rakenduses funktsioon (funktsioonide container).
Mälu suvandid erineda **128 MB 1536 MB**. Valitud mälu suurus vastab selle töötamise määrata kõik funktsioonid, mis on osa funktsioon rakenduse jaoks vajalik. Kui teie kood nõuab rohkem mälu, kui valitud suurus, on funktsioon rakenduse eksemplari sulgeda saadaoleva mäluga puuduvad.

### <a name="scaling"></a>Mastaapimine

Funktsioonide Azure'i platvormi hindab liikluse vajadustele, vastavalt konfigureeritud päästikute, et otsustada, millal mastaapimiseks üles või alla. Granulaarsus skaleerimist on funktsioon rakendus. Ülespoole sel juhul tähendab, et lisada mitu eksemplari funktsioon rakendus. Vastupidiselt liikluse läheb alla, funktsioon rakenduse eksemplari on keelatud-lõpuks skaleerimist nullini, kui ükski töötab.  

### <a name="resource-consumption-and-billing"></a>Ressursside tarbimine ja arveldamine

Klõpsake dünaamiline režiimi ressursi eraldus on valmis standardse rakenduse teenusleping teisiti, seetõttu on ka erinev, võimaldades mudeli "pay-kasutada" tarbimine mudel. Ainult ajaks, millal kood käivitatakse esitatakse tarbimine funktsioon rakenduse kohta.  
See on arvutatud mälu suuruse (GB) korrutatakse kogusumma täitmisaeg (sekundites) kõik funktsioonid, mis töötab funktsioon rakenduse sees. Ühiku tarbimine saab **GB-s (gigabait sekundites)**.