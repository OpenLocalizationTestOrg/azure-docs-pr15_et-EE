## <a name="load-balancer-differences"></a>Laadi koormusetasakaalustusteenuse erinevused

Seal on erinevaid võimalusi levitada kasutades Microsoft Azure'i võrguliiklust. Need suvandid töötavad teisiti, võttes eri funktsiooni seadmine ja toetavad erinevaid stsenaariumeid lähemalt. Nad saavad iga kasutada eraldi või neid.

- **Azure'i laadi koormusetasakaalustusteenuse** töötab transpordikihi (virnas OSI võrgu viide Layer 4). Võrgu taseme jaotuse liikluse pakub mitmes eksemplaris sama Azure andmekeskuse töötavad rakenduse.

- **Lüüsi rakendus** töötab kihid (Layer 7 virnas OSI võrgu viide). See toimib vastupidi-puhverserveri teenuse kliendi ühenduse lõpetamine ja edastamine taotluste tagaandmebaas lõpp-punktid.

- DNS-i tasemel toimib **Liikluse haldur** .  DNS-i vastuste kasutab globaalselt hajutatud lõpp-punktid lõppkasutaja liikluse suunamiseks. Klientide ühenduse siis otse nende lõpp-punktid.

Järgmises tabelis on kokku võetud funktsioone, mida iga teenus pakub.

| Teenus | Azure'i laadi koormusetasakaalustusteenuse | Rakenduse lüüsi | Liikluse haldur |
|---|---|---|---|
|Tehnoloogia| Transport tase (kiht 4) | Rakenduse tase (kiht 7) | DNS-i tase |
| Rakendus Protokollid toetatud | Mis tahes | HTTP ja HTTPS |  Mis tahes (HTTP lõpp on vajalik lõpp-punkti jälgimiseks) |
| Lõpp-punktid | Azure'i VMs ja pilveteenustega rolli aknad | Mis tahes Azure'i sisemine IP-aadress või avaliku Interneti IP-aadress | Azure'i VMs, pilveteenused, Azure'i veebirakenduste ja väliste lõpp-punktid |
| VNet tugi | Saab kasutada nii Interneti suunatud ja sisemise (Vnet) rakendused | Saab kasutada nii Interneti suunatud ja sisemise (Vnet) rakendused |    Toetab ainult Interneti-ühendusega rakenduste |
Lõpp-punkti jälgimine | Toetatud sondid kaudu | Toetatud sondid kaudu | Toetatud HTTP-või HTTPS toomine kaudu | 

Azure'i laadi koormusetasakaalustusteenuse ja rakenduse lüüsi marsruutimiseks võrgu liikluse lõpp-punktid, kuid neil on erinevate kasutamise stsenaariumid, mille liiklus käsitlema. Järgmine tabel aitab kaks koormus soolise erinevused.

| Tüüp | Azure'i laadi koormusetasakaalustusteenuse | Rakenduse lüüsi |
|---|---|---|
| Protokollid | UDP/TCP | HTTP / HTTPS |
| IP broneerimiseks | Toetatud | Pole toetatud | 
| Koormust tasakaalustavad režiim | 5-tuple(source IP, source port, destination IP, destination port, protocol type) | Round jaan<br>URL-i põhjal marsruutimine | 
| Laadi tasakaalustamiseks režiimi (lähte-IP / sticky seansid) |  2-kordse (allika IP ja sihtkoha IP), 3-kordse (allika IP, sihtkoha IP ja port). Saate mastaapimiseks üles või alla virtuaalmasinates arvu alusel | Küpsis vastavalt osaleja<br>URL-i põhjal marsruutimist |
| Seisund sondid | Vaikimisi: juures intervall - 15 sekundit. Välja pööre: 2 pidev ebaõnnestumist. Kasutaja määratletud toetab sondid | Jõude juures intervall 30 sekundit. Võtta pärast 5 järjestikust reaalajas liiklust tõrked või ühe juures nurjumise jõude režiimis. Kasutaja määratletud toetab sondid | 
| SSL-i mahalaadimine | Pole toetatud | Toetatud | 
  