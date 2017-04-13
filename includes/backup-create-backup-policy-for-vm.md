## <a name="defining-a-backup-policy"></a>Varukoopia poliitika määratlemine

Varukoopia poliitika määratleb kui andmete hetktõmmiste on võetud ja kaua neid pilte säilitatakse maatriks. Poliitika varundamiseks VM määratlemisel võite käivitada detailse *kord päevas*. Kui loote uue poliitika, rakendatakse see vault. Varukoopia poliitika kasutajaliidese näeb välja umbes järgmine:

![Varukoopia poliitika](./media/backup-create-policy-for-vms/backup-policy.png)

Poliitika loomiseks tehke järgmist.

1. Sisestage nimi **poliitika nimi**.

2. Andmete hetktõmmiste võib võtta päeva-, nädala- või intervalliga. **Varundamise sagedus** rippmenüü abil saate valida, kas andmete hetktõmmiste võetakse iga päev või nädala.

    - Kui valite igapäevane intervall, esiletõstetud juhtelemendi abil saate valida päeva hetktõmmis jaoks aega. Tunni muutmiseks tühistage valik tunni ja valige uus tund.

    ![Igapäevase varukoopia poliitika](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>

    - Kui valite nädala intervall, esiletõstetud juhtelementide abil saate valida soovitud toimumise nädalapäevad ja kellaaeg hetktõmmise tegemiseks. Valige menüüs päeva, üks või mitu päeva. Valige menüüs tund, üks tund. Tunni muutmiseks tühistage valik valitud tunni ja valige uus tund.

    ![Nädala varukoopia poliitika](./media/backup-create-policy-for-vms/backup-policy-weekly.png)

3. Vaikimisi on valitud kõik **Säilitamise erinevaid** võimalusi. Tühjendage ruut säilituspoliitika hulka piirata, mida soovite kasutada. Määrake interval(s) kasutada.

    Kuu ja kord aastas säilituspoliitika vahemike abil saate määrata vastavalt nädala või päeva lisandus tõmmised.

    >[AZURE.NOTE] Kui VM kaitsmisele Varundustöö töötab kord päevas. Kui varukoopia töötab aeg on sama iga vahemiku säilitamine.

4. Pärast kõigi suvandite poliitika seadmisele ülaosas tera, klõpsake nuppu **Salvesta**.

    Uue poliitika rakendatakse kohe vault.
