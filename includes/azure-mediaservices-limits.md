Ressurss|Vaikimisi limiit|Lubatud
---|---|---
Ühe tellimuse Azure Media Services (AMS) kontod||25
Varade AMS konto kohta||1 000 000<sup>1</sup>
Aheldatud tööülesannete kohta||30
Ülesande varad||50
Varade töökoha kohta||100
Töö AMS konto kohta ||50 000<sup>2</sup>
Kordumatu lokaatorid seotud vara korraga||5<sup>4</sup>
Reaalajas kanalite AMS konto kohta </p></td>|5</p></td>|N/A<sup>1</sup>
Programmide peatatud olekus kanali kohta </p></td>|50</p></td>|N/A<sup>1</sup>
Programmide käitamise olekus kanali kohta </p></td>|3</p></td>|3
Streaming lõpp-punktid töötab olekus AMS konto kohta</p></td>|2</p></td>|N/A<sup>1</sup>
Streaming streaming lõpp-punkti kohta </p></td>|10 </p></td>|N/A<sup>1</sup>
Kodeering ühiku AMS konto </p></td>|25</p></td>|N/A<sup>1</sup>
Salvestusruumi kontod | |1000<sup>5</sup>
Poliitika || 1 000 000<sup>6</sup>

<sup>1</sup> saate taotleda värskendada, avades tugi Piletite kvoodi limiidid. Luua veel AMS kontosid suurendada piirangud, selle asemel Küsi tugi.

<sup>2</sup> see arv sisaldab ootel, lõpule jõudnud, aktiivne ja loobutakse tööde haldamine. Ei sisalda kustutatud tööde haldamine. Saate kustutada vana tööde **IJob.Delete** või **kustutada** HTTP-päringu abil.

<sup>3</sup> tehes loendi töö üksustele, kuni 1000 tagastatakse taotluse euro kohta. Kui teil on vaja jälgida kõiki esitatud töid, saate [OData süsteemi Päringusuvandid](http://msdn.microsoft.com/library/gg309461.aspx)kirjeldatud ülemine/vahele.

<sup>4</sup> lokaatorid on koostatud haldamise kasutaja juurdepääsu reguleerimine. Eri juurdepääsuõigused üksikkasutajale, kasutage digitaalõiguste halduse (DRM) lahendusi.

<sup>5</sup> salvestusruumi kontod peab olema sama Azure tellimusest.

<sup>6</sup> on piirang 1 000 000 poliitika erinevate AMS poliitikaid (näiteks Locator poliitika või ContentKeyAuthorizationPolicy). Sama poliitika ID tuleks kasutada siis, kui te kasutate alati sama päeva / juurdepääsu õiguste / jne.
