Saate luua mitme teenuste tellimus, igat ette valmistatud teatud taseme, piiratud ainult iga taseme lubatud teenuste arvu juures. Näiteks võite luua kuni 12 teenuste tavaline astme ja teise 12 teenuseid S1 taseme ühe tellimuse sees. Astme kohta leiate lisateavet teemast [valimine soovitud SKU-ga või taseme Azure'i otsingu jaoks](../articles/search/search-sku-tier.md).

Soovi korral saate tõsta lubatud teenuste piirangud. Kui teil on vaja veel ühe tellimuse teenused, võtke ühendust Azure toega.

Ressurss|Tasuta|Tavaline|S1|S2|S3 |S3 HD <sup>1</sup>
---|---|---|---|----|---|----
Suurim lubatud teenuste |1 |12 |12  |6 |6 |6 
Skaala SU <sup>2</sup>|N/A <sup>3</sup>|3 SU <sup>4</sup> |36 SU|36 SU|36 SU|12 SU 3 SU <sup>5</sup>

<sup>1</sup> S3 HD ei toeta [indexers](../articles/search/search-indexer-overview.md) sel ajal. 

<sup>2</sup> Otsi üksused (SU) on tasustatav ühiku teenus, mis on määratud *sektsioon*või *koopia* . Salvestusruumi, indekseerimine ja toimingute jaoks on vaja nii ressursid. Kehtivad kombinatsioonid jäävad jaotises ülempiirid kohta leiate lisateavet teemast [Ressursi tasemeid päringu- ja töökoormus](../articles/search/search-capacity-planning.md). 

<sup>3</sup> tasuta põhineb kasutavad mitu tellijad ühiskasutusega ressurssidele. Selle taseme on jaoks soovitud üksikud abonendi sihtotstarbeline ressursse. Seetõttu skaala on märgitud pole rakendatav.

<sup>4</sup> basic on üks määratud sektsioon. Selle taseme veebisaidil kasutatakse täiendavate SUs jaotamiseks rohkem koopiad suurendatud päringu töökoormus.

<sup>5</sup> S3 HD on lubatud kombinatsioonid erinevate eraldatud struktuuri. Koopiate, võib olla kuni 12. Sektsioonid, on maksimaalne 3.




