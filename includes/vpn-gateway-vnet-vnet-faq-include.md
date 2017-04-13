- Virtuaalne võrkude võib olla samas või mõnes muus Azure piirkondades (asukohad).

- Pilveteenus või tasakaalustamiseks lõpp-punkti koormus ei saa jaotada üle virtuaalse võrkude, isegi juhul, kui need on ühendatud koos.

- Ühenduse loomine mitme Azure virtuaalse võrgu koos ei vaja mis tahes kohapealse VPN lüüside juhul, kui on vaja ühenduvust asukohaga.

- VNet-VNet toetab ühendusjooni virtuaalse võrgustikke. See ei toeta virtuaalmasinates ühendusjooni ega cloud services pole virtuaalse võrgus.

- VNet-VNet nõuab Azure'i VPN lüüside RouteBased (varem nimetati dünaamiline marsruutimine) koos VPN-tüüpi. 

- Virtuaalne võrguühendus saab kasutada korraga mitme saidi VPN, kuni 10 (vaikimisi/Standard lüüsid) või 30 (suure jõudlusega lüüsid) VPN tunnelite muude virtuaalse võrguga ühenduse virtuaalse võrgu VPN lüüsi või kohapealse saidid.

- Aadress tühikute virtuaalse võrgu ja kohapealsete kohtvõrgu saitide peab ei kattuks. Kattuvate aadress tühikute põhjustab nurjumise VNet-VNet ühenduste loomine.

- Liigsete tunnelid virtuaalne võrkude paari vahele ei toetata.

- Kõigi VPN tunnelite virtuaalse võrgu läbilaskevõime Azure'i VPN-lüüsi ja sama VPN lüüsi sees SLA Azure ühiskasutusse anda.

- VNet-VNet liikluse läbib üle Microsoft Network, mitte Interneti-ühendus.

- VNet-VNet liiklus samas piirkonnas on tasuta mõlemast suunast; rist piirkond VNet-VNet sealt on liikluse eest väljaminev muu VNet andmete edastamine määr allika piirkondade alusel. Vaadake [lehel hinnad](https://azure.microsoft.com/pricing/details/vpn-gateway/) üksikasju.