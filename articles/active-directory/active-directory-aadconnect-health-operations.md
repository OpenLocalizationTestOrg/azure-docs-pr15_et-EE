<properties
    pageTitle="Azure'i AD-ühenduse seisund toimingud."
    description="Selles artiklis kirjeldatakse täiendavad toimingud, mida saab teha, kui olete juurutanud Azure'i AD-ühenduse seisund."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Azure'i AD-ühenduse seisund toimingud

Järgmine teema kirjeldab erinevaid toiminguid, mida saab teha Azure'i AD-ühenduse seisundit.

## <a name="enable-email-notifications"></a>Luba meiliteatised
Saate konfigureerida Azure'i AD-ühenduse seisund teenuse teatised, mis näitab, oma identiteedi taristu ei ole terve meiliteatiste saatmiseks. See juhtub teatise loomisel ning seda märgitud lahendatud. Järgige alltoodud juhiseid, et konfigureerida meiliteatised.

![Azure'i AD-ühenduse seisund e-posti teatis avastamine](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] Vaikimisi on keelatud meiliteatised.


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Lubamiseks Azure AD ühenduse seisundit meiliteatised

1. Avage teatiste tera teenusele, millega soovite saada e.
2. Toiminguriba nupp "Teavitussätete" klõpsake nuppu.
3. Lülitage parameetrit meiliteatise sees.
4. Märkige ruut konfigureerida kõik Üldadministraatorid saada meiliteatisi.
5. Kui soovite saada e-postiga teateid e-posti aadressid, saate need määrata väljal täiendavad meilisõnumi adressaadi. E-posti aadress loendist eemaldada, Paremklõpsake kirjet ja valige Kustuta.
6. Viimistlemine klõpsake muudatuste "Salvesta". Kõik muudatused võtab efektid alles pärast seda, kui valite "Salvesta".

## <a name="delete-a-server-or-service-instance"></a>Serveri või teenuse eksemplari kustutamine

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>Azure'i AD-ühenduse seisund teenuse server kustutamine
Mõnel juhul soovite jälgida serveri eemaldamine. Järgige alltoodud juhiseid, et eemaldada server Azure'i AD-ühenduse seisund teenuse.

Serveri kustutamisel arvestage järgmist:

- See toiming PEATAB koguda rohkem teavet, et server. Selle serveri eemaldatakse jälgimise teenus. Pärast selle toimingu te ei saa uusi teatisi, vaadata kasutus- või analytics andmete selles serveris.
- See toiming ei desinstallimine või seisundi Agent eemaldamine oma serveri. Kui on enne selle etapi läbimiseks seisund Agent desinstallitud, võidakse kuvada tõrketeade sündmuste seotud seisund Agent serveris.
- See toiming ei kustutata juba kogutud selles serveris andmeid. Ühe säilituspoliitika Microsoft Azure'i andmed kustutatakse andmeid.
- Pärast selle toimingu tegemiseks, kui soovite sama server uuesti jälgimise alustamiseks pead eemaldada ja uuesti installida seisundit agent selles serveris.


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>Kustutamiseks seisund teenuse Azure'i AD-ühenduse server

Azure'i AD-ühenduse seisundi jaoks AD FS-i ja Azure AD ühenduse (Sünkrooni):

1. Serveri nimi eemaldada klõpsates avada serveri tera keelest serveri loend.
2. Enne Server, klõpsake nuppu "Kustuta" Toiminguriba.
3. Kinnitage kustutamine server, tippides serverinime kinnitusdialoogis toimingu.
4. Klõpsake nuppu "Kustuta".

Azure'i AD-ühenduse AD DS seisund:

1. Avage domeenikontrollerid armatuurlaud.
2. Valige selle domeenikontrolleri eemaldada.
3. Klõpsake nuppu "Kustuta valitud" Toiminguriba.
4. Kinnitage serveri kustutamine toimingut.
5. Klõpsake nuppu "Kustuta".

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Azure'i AD-ühenduse seisund teenuse teenuse eksemplari kustutada

Mõnel juhul soovite eemaldada teenuse eksemplari. Järgige alltoodud juhiseid, et eemaldada Azure'i AD-ühenduse seisund teenuse teenuse eksemplari.

Teenuse eksemplari kustutamisel arvestage järgmist:

- See toiming eemaldab praeguse eksemplari jälgimise teenus.
- See toiming on ei desinstallimine või eemaldamine seisund Agent mis tahes serverid, mis olid jälgida teenuse eksemplar osana. Kui on enne selle etapi läbimiseks seisund Agent desinstallitud, võidakse kuvada tõrketeade sündmuste server, mis on seotud Agent seisundi kohta.
- Ühe säilituspoliitika Microsoft Azure'i andmed kustutatakse kõik andmed, see teenus eksemplarist.
- Pärast selle toimingu tegemiseks teenuse jälgimise alustamiseks soovi palun eemaldada ja uuesti installida seisund agent serverites, mis jälgib. Pärast selle toimingu tegemiseks, kui soovite sama server uuesti jälgimise alustamiseks pead eemaldada ja uuesti installida seisund agent selles serveris.


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>Azure'i AD-ühenduse seisund teenuse teenuse eksemplari kustutada

1. Avage teenus tera keelest teenuste loend, valides teenuse identifikaator (serveripargi nimi), mida soovite eemaldada.
2. Enne Server, klõpsake nuppu "Kustuta" Toiminguriba.
3. Teenuse nime kinnitamiseks tippige see väljale kinnitus. (nt: sts.contoso.com)
4. Klõpsake nuppu "Kustuta".
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Rollipõhise juurdepääsu haldamine juurdepääsu kontroll
### <a name="overview"></a>Ülevaade
Azure'i AD-ühenduse seisund [Rollipõhine juurdepääsu juhtimine](role-based-access-control-configure.md) võimaldab juurdepääsu teenuse seisund Azure'i AD-ühenduse kasutajate ja/või rühmade globaalne administraatorite väljaspool. See on saavutada rollide määramine ettenähtud kasutajate või rühmade ja võimaldab piirata globaalse kataloogi raames administraatorid.

#### <a name="roles"></a>Rollid
Azure'i AD ühenduse seisund toetab sisseehitatud järgmised rollid.

| Roll | Õigused |
| ----------- | ---------- |
| Omanik | Omanikud saavad ***juurdepääsu haldamine*** (nt kasutaja/rühma määra roll), ***vaadata kogu teavet*** (nt teatiste vaatamine) portaali ja ***muuta sätteid*** (nt meiliteatised) jooksul seisund Azure'i AD-ühenduse kaudu. <br>Vaikimisi Azure AD globaalse administraatorid määratakse selle rolli ja seda ei saa muuta.  |
|Kaasautor|  Osaliste saate ***vaadata kogu teavet*** (nt teatiste vaatamine) portaali ja ***muuta sätteid*** (nt meiliteatised) jooksul seisund Azure'i AD-ühenduse kaudu.|
|Lugeja| Lugejad saavad ***vaadata kogu teavet*** (nt teatiste vaatamine) asuvad Azure'i AD-ühenduse Health portaalist.|

Kõik muud rollid (nt "Kasutaja juurdepääsu administraatorid" või "DevTest Labs kasutajad"), isegi kui see on saadaval portaali kogemus, ei mõjuta juurdepääsu Azure'i AD-ühenduse seisund sees.

#### <a name="access-scope"></a>Accessi ulatus

Azure'i AD-ühendus toetab haldamine Accessi kahel viisil:

- ***Teenuse läbivalt kogu***: see on soovitatav tee enamik klientide ja määrab juurdepääsu läbivalt kogu teenuse (nt ADFS-i serveripargi) üle kõik rolli tüüpe jälgitakse Health Azure'i AD-ühenduse.

- ***Eksemplari***: mõnedel juhtudel peate eraldada Accessi aluseks rolli tüüpi või teenuse astme. Sel juhul saate hallata juurdepääsu teenuse astme.  

On lubatud, kui kasutajal on juurdepääs ühte kataloogi või eksemplari tasemel.


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Kuidas lubada kasutajate või rühmade juurdepääsu Azure'i AD-ühenduse seisund
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Juhiseid 1: Valige sobiv juurdepääsu ulatus
Kui soovite lubada kasutajate juurdepääsu *teenuse läbivalt kogu* tasemel asuvad Health Azure'i AD-ühenduse, avada peamised tera Azure'i AD-ühenduse seisund.<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>Samm 2: Lisada kasutajad, rühmad ja määrake rollid
1. Klõpsake jaotises Konfigureerige "Kasutajad" osa.<br>
![Azure'i AD-ühenduse seisundit RBAC peamised Blade](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Valige "Lisa"
3. Valige "Roll", näiteks "Omanik"<br>
![Azure'i AD-ühenduse seisund RBAC kasutaja lisamine](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Tippige soovitud nimi või sihtarvutites kasutaja või rühma. Saate valida ühe või mitme kasutajate või rühmade samal ajal. Klõpsake "Vali".
![Azure'i AD-ühenduse seisund RBAC valige kasutaja](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Valige "Ok".<br>

6. Kui soovitud Rollimäärang on lõpule jõudnud, kuvatakse kasutajate ja/või rühmade loendis.<br>
![Azure'i AD-ühenduse Health RBAC kasutaja nimekiri](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Need juhised võimaldab juurdepääsu vastavalt nende määratud rollid ja loetletud kasutajate.
>[AZURE.NOTE]
- Globaalse administraatorid on alati täielik juurdepääs kõik toimingud, kuid üldadministraator kontod ei ole ülaltoodud loendis olemas.
- Funktsiooni "Kasutajate kutsumine" asuvad Azure'i AD-ühenduse Health ei toetata.

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Samm 3: Jagada kasutajatele või rühmadele blade asukoht
1. Pärast õiguste määramine kasutaja pääseb juurde Azure'i AD-ühenduse seisund minnes [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth).
2. Üks kord enne, kasutaja saate kinnitada tera või erinevate osade armatuurlaud, klõpsates lihtsalt "Kinnita armatuurlaua"<br>
![Azure'i AD-ühenduse seisund RBAC PIN-koodi blade](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] Kasutaja "Lugeja" roll ei saa "Loo" saada Azure'i AD-ühenduse seisund laiend Azure'i turuplatsilt toimingu sooritamiseks. Selle kasutaja pääsevad tera minnes eespool link. Järgmise kasutuse, saate kasutaja kinnitada tera armatuurlaud.

### <a name="remove-users-andor-groups"></a>Kasutajate ja/või rühmade eemaldamine
Saate eemaldada kasutaja või rühma lisatud Azure'i AD-ühenduse seisundi rolli alusel juurdepääsu reguleerimine osa paremale klõpsake ja valige Eemalda.<br>
![Azure'i AD-ühenduse seisund RBAC Eemalda kasutaja](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Seotud lingid

* [Azure'i AD-ühenduse seisund](active-directory-aadconnect-health.md)
* [Azure'i AD-ühenduse seisund agenti installi](active-directory-aadconnect-health-agent-install.md)
* [Kasutades Azure AD AD FS-i seisundit kasutajaga](active-directory-aadconnect-health-adfs.md)
* [Azure'i AD-ühenduse seisundi kasutamise sünkroonimine](active-directory-aadconnect-health-sync.md)
* [Kasutades Azure AD kasutajaga AD DS seisund](active-directory-aadconnect-health-adds.md)
* [Azure'i AD-ühenduse seisund KKK](active-directory-aadconnect-health-faq.md)
* [Azure'i AD-ühenduse seisund versiooniajalugu](active-directory-aadconnect-health-version-history.md)
