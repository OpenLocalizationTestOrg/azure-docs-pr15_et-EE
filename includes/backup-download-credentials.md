## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Hoidla mandaat autentida Azure varukoopia teenuse abil

Kohapealses serveris (Windowsi kliendi või serveriga Windows Server või andmete kaitse Manager) peab olema kinnitatud varukoopiate hoidla enne selle saate varundada Azure. Autentimine on saavutada, kasutades "võlvkelder identimisteabe". Hoidla mandaat mõiste on sarnane "avaldada sätted" faili, mida kasutatakse Azure PowerShelli.

### <a name="what-is-the-vault-credential-file"></a>Mis on vault mandaati fail?

Vault identimisteabe fail on iga varukoopiate hoidla portaali loodud sert. Portaali seejärel lisatud avalik võti Access Control teenus (ACS). Serdi privaatvõti on kättesaadav kasutajale töövoo, mis on antud sisendina seadme registreerimine töövoo käigus. See autendib masina varundatud andmete saatmiseks on tuvastatud vault teenuses Azure varukoopia.

Vault mandaati kasutatakse ainult registreerimise töövoo käigus. See on kasutaja kohustusi tagamaks, et vault identimisteabe fail on rikutud pole. Kui see jääb kätte petturitest-kasutaja, saab vault identimisteabe faili vastu sama vault muud seadmed registreerida. Siiski nagu varundatud andmed on krüptitud parool, mis kuulub kliendi, ei saa varukoopia olemasolevaid andmeid kahjustada. Selle probleemi leevendada, hoidla mandaat on seatud 48hrs lõpeb. Saate alla laadida hoidla mandaat varukoopiate hoidla, mis tahes arv kordi – kuid ainult uusima vault mandaati fail on registreerimise töövoo käigus.

### <a name="download-the-vault-credential-file"></a>Vault mandaati faili alla laadida

Vault mandaati faili alla laadida turvalise kanali Azure portaali kaudu. Azure'i varunduse teenus on teadlikud sertifikaadi privaatvõti ja privaatvõti mitte hoitakse portaalis või teenuse. Järgmiste juhiste abil saate vault mandaati faili allalaadimiseks kohalikus arvutis.

1.  Logige sisse [haldusportaal](https://manage.windowsazure.com/)
2.  Klõpsake vasakpoolsel navigeerimispaanil **Taastamise teenused** ja valige varukoopiate hoidla, mille olete loonud. Klõpsake ikooni cloud varukoopiate hoidla Kiirkäivituse vaate avamiseks.

    ![Kiirvaade](./media/backup-download-credentials/quickview.png)

3.  Klõpsake lehel kiirjuhendi **allalaadimine hoidla mandaat**. Portaali loob vault mandaati faili, mis tehakse allalaadimiseks saadaval.

    ![Laadi alla](./media/backup-download-credentials/downloadvc.png)

4.  Portaali loob vault mandaati kasutades kombinatsiooni vault nimi ja praeguse kuupäeva. Klõpsake nuppu **Salvesta** alla laadida hoidla mandaat kohaliku konto allalaaditavad failid kausta või menüüst Salvesta, et määrata asukoht hoidla mandaat, valige käsk Salvesta nimega.

### <a name="note"></a>Märkus
- Veenduge, et hoidla mandaat on salvestatud kohas, kuhu pääseb juurde teie arvutist. Kui see on salvestatud faili ühiskasutusse andmine/SMB, kontrollige juurdepääsu õigusi.
- Vault identimisteabe faili kasutatakse ainult registreerimise töövoo käigus.
- Vault identimisteabe faili aegub pärast 48hrs ja saab alla laadida portaali.
- Vaadake Azure varukoopia [FAQ](../articles/backup/backup-azure-backup-faq.md) küsimuste töövoo jaoks.
