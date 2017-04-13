Ressurss|Vaikimisi limiit
---|---
Arv tellimuse kohta salvestusruumi kontod|200<sup>1</sup>
TB salvestusruumi konto kohta|500 TB
Max arv bloobimälu ümbriste, plekid, failikettad, tabeleid, järjekorrad, üksuste või sõnumite salvestusruumi konto kohta|Ainult lubatud 500 TB salvestusruumi konto
Max suurus ühe bloobimälu container, tabelis või järjekord|500 TB
Max arv Blokeeri bloobimälu plokki või lisage bloobimälu|50.000
Max tekstiplokk Blokeeri bloobimälu suurust või lisa bloobimälu|4 MB
Max Blokeeri bloobimälu suurust või lisa bloobimälu|50.000 x 4 MB (umbes 195 GB) 
Max mahu lehe bloobimälu |1 TB
Tabeli üksuse max suurus|1 MB
Max arv tabeli üksuse atribuudid|252
Sõnumi järjekorda Max maht|64 KB
Faili ühiskasutus max suurus|5 TB
Failide ühiskasutamine faili max suurus|1 TB
Max arv failide Failide ühiskasutamine|Ainult lubatud 5 TB koguvõimsus faili ühiskasutusse andmine
Max 8 KB IOPS aktsia|1000
Max arv failide Failide ühiskasutamine|Ainult lubatud 5 TB koguvõimsus faili ühiskasutusse andmine
Max arv bloobimälu ümbriste, plekid, failikettad, tabeleid, järjekorrad, üksuste või sõnumite salvestusruumi konto kohta|Ainult lubatud 500 TB salvestusruumi konto
Max arv salvestatud juurdepääsupoliitikaid container, failikettal, tabel või järjekord|5
Kokku taotleda Rate (eeldades, et 1KB objekti suuruse) salvestusruumi konto kohta|Kuni 20 000 IOPS, üksuste sekundis või sõnumite sekundis
Target (sihtkoht) läbilaskevõime jaoks ühe bloobimälu|Kuni 60 MB teine või kuni 500 taotlusi sekundis euro kohta.
Target (sihtkoht) läbilaskevõime ühe järjekorra (1 KB sõnumid)|Kuni 2000 sõnumite sekundis
Target (sihtkoht) läbilaskevõime ühe tabeli sektsiooni (1 KB üksused)|Kuni 2000 üksuste sekundis
Target (sihtkoht) läbilaskevõime jaoks ühte faili ühiskasutusse andmine|Kuni 60 MB sekundis
Max sissepääsu<sup>2</sup> tk salvestusruumi konto (meile piirkonnad)|10 Gbit GRS/ZRS<sup>3</sup> lubamisel 20 Gbit LRS jaoks
Max sealt<sup>2</sup> tk salvestusruumi konto (meile piirkonnad)|20 Gbit kui RA-GRS GRS ZRS<sup>3</sup> lubatud, 30 Gbit LRS jaoks
Max sissepääsu<sup>2</sup> tk salvestusruumi konto (Euroopa ja Aasia regioonid)|5 Gbit kui GRS/ZRS<sup>3</sup> lubatud, 10 Gbit LRS jaoks
Max sealt<sup>2</sup> tk salvestusruumi konto (Euroopa ja Aasia regioonid)|10 Gbit RA-GRS GRS ZRS<sup>3</sup> lubamisel 15 Gbit LRS jaoks

<sup>1</sup> See hõlmab nii Standard- ja Premium salvestusruumi kontod. Kui vajate rohkem kui 200 salvestusruumi kontod, esitada taotluse [Azure tugiteenuste](https://azure.microsoft.com/support/faq/)kaudu. Azure Storage meeskonnatöö läbi teie ettevõtte puhul ja kiita kuni 250 salvestusruumi kontod. 

<sup>2</sup> *Sissepääsu* viitab saadetavate salvestusruumi konto kõigi andmete (taotluste). *Sealt* viitab kõigi andmete (vastuste) saadud salvestusruumi konto.  

<sup>3</sup> Azure'i salvestusruumi Tiražeerimissuvandid on järgmised.

- **RA-GRS**: lugemisõigus – geograafilise liigne salvestusruumi. Kui RA-GRS on lubatud, sealt eesmärgid teisene asukoha jaoks on identsed esmane asukoht.
- **GRS**: geograafilise liigne salvestusruumi. 
- **ZRS**: Zone liigsete salvestusruumi. Saadaval ainult Blokeeri plekid. 
- **LRS**: kohalikult liigsete salvestusruumi. 

