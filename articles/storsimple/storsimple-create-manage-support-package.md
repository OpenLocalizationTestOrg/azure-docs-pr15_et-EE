<properties
   pageTitle="StorSimple tugi paketi loomine | Microsoft Azure'i"
   description="Saate teada, kuidas luua, dekrüptida ja redigeerida StorSimple seadme tugi pakett."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />


# <a name="create-and-manage-a-storsimple-support-package"></a>Saate luua ja hallata paketi StorSimple tugi

## <a name="overview"></a>Ülevaade

Paketi StorSimple tugi on lihtne kasutada mis kogub kõik oluline logid abistamiseks Microsoft Support StorSimple seadme seotud probleemide tõrkeotsing. Kogutud logid on krüptitud ja tihendatud.

Õppeteema sisaldab üksikasjalikke juhiseid, luua ja hallata tugiteenuste pakett.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Loomine ja Azure klassikaline portaalis tugiteenuste paketi üleslaadimine

Saate luua ja üles laadida tugiteenuste pakett Microsoft Support saidi **hooldusvaates teenuse Azure klassikaline portaali** kaudu.

> [AZURE.NOTE] Üles nõuab pääsukoodi tugi. Teie tugiteeninduse peaks andma see teile meili teel.

On krüptitud ja tihendatud tugiteenuste pakett (CAB-faili) on loodud ja üles laadida saidile tugiteenuste. Tugiteeninduse seejärel saate tuua selle paketi probleemi tõrkeotsinguks saidilt tugi.

Klassikaline portaalis tugiteenuste paketi loomiseks tehke järgmist.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Azure'i klassikaline portaalis tugi paketi loomiseks

1. Valige **seadmed** > **hooldustööd**.

2. Valige jaotises **toetavad paketi** **loomine ja laadi tugi pakett**.

3. Dialoogiboksis **loomine ja üleslaadimise tugiteenuste pakett** tehke järgmist.

    ![Tugiteenuste paketi loomine](./media/storsimple-create-manage-support-package/IC740923.png)

    - Sisestage tekstiväljale **Tugi parool** parool. Oma Microsofti tugiteenuste töötaja peaks seda pääsukoodi saada teile e-posti.

    - Märkige ruut anda nõusoleku üleslaadimiseks tugiteenuste pakett Microsoft Support saidile.

    - Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-create-manage-support-package/IC740895.png).


## <a name="manually-create-a-support-package"></a>Tugiteenuste paketi loomine käsitsi

Mõnel juhul peate käsitsi luua tugiteenuste pakett Windows PowerShelli kaudu StorSimple. Näiteks:

- Kui teil on vaja oma logifailid enne Microsoft Support ühiskasutusse andmise tundliku teabe eemaldamine.

- Kui teil on raskusi ühenduvusprobleemide tõttu paketi üleslaadimine.

Saate teie käsitsi loodud tugiteenuste pakett Microsoft Support e-posti kaudu jagada. Tehke StorSimple Windows PowerShelli tugi paketi loomiseks tehke järgmist.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Windows PowerShelli tugi paketi loomiseks StorSimple

1. Windows PowerShelli seansi käivitamine kaugarvutis StorSimple seadme ühenduse loomiseks kasutatava administraatorina, sisestage järgmine käsk:

    `Start PowerShell`

2. Windows PowerShelli seanss, ühendada oma seadme SSAdmin konsooli.

    - Sisestage käsuviibale.

        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`

    1. Tippige avanevas dialoogiboksis oma seadme administraatori parooli. Vaikimisi parool on:

        `Password1`

        ![Dialoogiboks PowerShelli mandaadi](./media/storsimple-create-manage-support-package/IC740962.png)

    2. Klõpsake nuppu **OK**.
    1. Sisestage käsuviibale.

        `Enter-PSSession $MS`

3. Seansi, mis avab, sisestage asjakohane käsk.

    - Võrgu aktsiad, mis on kaitstud parooliga, sisestage:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        Palutakse parooli, ühiskasutusega võrgukaust ja mõne krüptimine parooli tee (kuna paketi tugi on krüptitud). Tugiteenuste pakett luuakse siis määratud kausta.

    - Mis on kaitstud parooliga aktsiate, pole vaja selle `-Credential` parameeter. Sisestage järgmine:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        Tugiteenuste pakett on loodud nii domeenikontrollerid määratud ühiskasutusega võrgukaust. See on krüptitud, tihendatud faili, mille saate saata Microsoft Support tõrkeotsingu. Lisateavet leiate teemast [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md).


### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>Cmdlet-käsk Ekspordi-HcsSupportPackagePõrandaliist parameetrid
Saate järgmiste parameetrite cmdlet-käsu ekspordi-HcsSupportPackagePõrandaliist.

| Parameetri            | Nõutav/valikuline | Kirjeldus                                                                                                                                                             |
|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-Path`                 | Nõutav          | Kasutage asukoht, kus tugiteenuste pakett paigutatakse võrgu ühiskausta.                                                                 |
| `-EncryptionPassphrase` | Nõutav          | Kasutage parooli abil krüptida tugiteenuste pakett.                                                                                                        |
| `-Credential`           | Valikuline.          | Kasutage Accessi mandaat ühiskasutusega võrgukaust esitama.                                                                                        |
| `-Force`                | Valikuline.          | Kasutage krüptimine parooli kinnitus sammu vahele jätta.                                                                                                                |
| `-PackageTag`           | Valikuline.          | Määrake kausta *tee* , kus paigutatakse tugi paketi abil. Vaikimisi on [seadme nimi]-[praeguse kuupäeva ja time:yyyy-MM-dd-HH-mm-ss].       |
| `-Scope`                | Valikuline.          | Määrake **kobar** (vaikeväärtus) toe paketi loomiseks nii kontrollerid. Kui soovite luua paketi ainult praegune kontrolleril, määrake **selle domeenikontrolleri**. |


## <a name="edit-a-support-package"></a>Tugiteenuste pakett redigeerimine

Kui olete loodud tugi paketi, peate tundliku teabe eemaldamiseks pakett redigeerimine. See võib hõlmata helitugevuse nimed, seadme IP-aadressid ja logifailide varukoopia nimed.

> [AZURE.IMPORTANT] Saab redigeerida ainult tugiteenuste paketi, mis loodi Windows PowerShelli kaudu StorSimple. Azure'i klassikaline portaalis StorSimple halduri teenuse loodud paketi ei saa redigeerida.

Tugi paketi üleslaadimine saidil Microsoft Support enne redigeerimiseks esmalt dekrüptida tugi paketi, faile redigeerida ja seejärel uuesti krüptimine. Tehke järgmist.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Windows PowerShelli tugi paketi StorSimple redigeerimiseks

1. Luua tugi paketi kirjeldatud varasemate versioonide [tugi Windows PowerShelli StorSimple paketi loomiseks](#to-create-a-support-package-in-windows-powershell-for-storsimple).

2. Kohalik klientarvutis [skripti alla laadida](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) .

3. Windows PowerShelli mooduli importida. Määrake kohalik skripti allalaaditud kausta tee. Importimiseks mooduli, sisestage:

    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. Kõik failid on *.aes* faile, mis on tihendatud ja krüptitud. Lahti ja dekrüptida faile, sisestage:

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    Pange tähele, et tegelik faililaiendid kuvatakse nüüd kõik failid.

    ![Tugiteenuste pakett redigeerimine](./media/storsimple-create-manage-support-package/IC750706.png)

5. Kui teilt küsitakse krüptimine parooli, sisestage parool, kui tugiteenuste pakett on loodud.

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:EncryptionPassphrase: ****

6. Liikuge kausta, mis sisaldab logifailid. Kuna logifailide on nüüd lahti ja dekrüptida, on need algse faililaiendid. Nende failide eemaldamiseks kliendikohased teabe, nt helitugevuse nimed ja seadme IP-aadressid, muutmine ja salvestage failid.

7. Sulgege failid koos gzip tihendada ja need AES 256 krüptida. See on kiiruse ja turvalisuse üle võtta võrgus. Tihendamine ja krüptida faile, sisestage järgmine:

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![Tugiteenuste pakett redigeerimine](./media/storsimple-create-manage-support-package/IC750707.png)

8. Küsimisel sisestage paketi muudetud tugi on krüptimine parooli.

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. Kirjutage uus parool, nii et saate seda jagada Microsoft Support nõudmisel.


### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Näide: Tugi paketi ühiskasutatav Paroolkaitsega failide redigeerimine

Järgmises näites kujutatakse dekrüptida, redigeerimine ja uuesti Krüpti tugiteenuste pakett.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [toe pakettide ja seadme logid oma seadme juurutuse tõrkeotsingu](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting)kasutamine.

- Siit saate teada, [hallata StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
