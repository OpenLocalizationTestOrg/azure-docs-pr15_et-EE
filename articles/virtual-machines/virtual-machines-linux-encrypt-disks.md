<properties
   pageTitle="Krüpti ketast Linux VM | Microsoft Azure'i"
   description="Kuidas krüptida ketast Linux VM Azure CLI ja ressursihaldur juurutamise mudeli kasutamise kohta"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Krüpti ketast Linux VM Azure CLI kasutamise kohta
Täiustatud virtuaalse masina (VM) Turve ja nõuetele vastavus, saate virtuaalse ketast Azure krüptitud ülejäänud juures. Ketast on krüptitud cryptographic tutvustatakse, mida on turvatud on Azure klahvi võlvkelder. Saate määrata cryptographic klahve ja saate auditeerida nende kasutamine. Selles artiklis üksikasjad, kuidas krüptida virtuaalse ketast Linux VM Azure CLI ja ressursihaldur juurutamise mudeli kasutamise kohta.


## <a name="quick-commands"></a>Kiirülevaate käsud
Kui teil on vaja kiiresti täita tööülesanne, jaotis järgmisega alus käske krüptimiseks virtuaalse ketast oma VM. Üksikasjalikumat teavet ja iga toimingu kontekst leiate [siit alates](#overview-of-disk-encryption)dokumendi ülejäänud.

Teil on vaja [Uusim Azure CLI](../xplat-cli-install.md) installitud ja sisse loginud kasutades režiimi ressursihaldur järgmiselt:

```
azure config mode arm
```

Järgmistes näidetes asendamine oma väärtustega näide parameetrite nimed. Näide parameetrite nimed sisaldavad `myResourceGroup`, `myKeyVault`, ja `myVM`.

Esmalt lubada Azure klahvi Vault pakkuja Azure tellimuse ja ressursside rühma loomine. Järgmises näites luuakse ressursi rühma nime `myResourceGroup` klõpsake soovitud `WestUS` asukoht:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Saate luua ka Azure võtme Vault. Järgmises näites luuakse klahvi võlvkelder nimega `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Oma klahvi võlvkelder krüptimisvõtme loomine ja selle lubamine ketta krüptimine. Järgmises näites luuakse nimega klahvi `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Looge lõpp töötlemise autentimise ja vahetamise cryptographic klahvikombinatsioone klahvi hoidlast Azure Active Directory abil. Funktsiooni `--home-page` ja `--identifier-uris` ei pea olema tegelik marsruuditavaid aadress. Turvalisuse kõrgeima taseme kliendi saladusi tuleks kasutada asemel paroolid. Azure'i CLI praegu ei saa luua kliendi saladusi. Kliendi saladusi saab luua ainult Azure'i portaalis. Järgmises näites luuakse Azure Active Directory lõpp-punkti nimega `myAADApp` ja parooli, kasutab `myPassword`. Enda parooli määrata järgmiselt:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Märkus selle `applicationId` näidatud eelmise käsu väljund. Selle rakenduse ID kasutatakse järgmist:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Saate lisada mõne olemasoleva VM andmed kettale. Järgmises näites liidetakse andmed kettale nimega VM `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Vaadata oma võti hoidla ja teie loodud võtme üksikasjad. Peate klahvi Vault ID, URI ja võti URL Viimane toiming. Järgmises näites kommentaare üksikasjad klahvi võlvkelder nimega `myKeyVault` ja võti nimega `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Krüpti oma ketast järgmiselt, sisestades kogu oma parameetrite nimed:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Azure'i CLI ei paku Paljusõnaline vigade krüptimine käigus. Tõrkeotsingu kohta lisateabe saamiseks vaadake `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Nagu eelmisel käsk on paljude muutujate ja te ei pruugi saada palju andmed selle kohta, miks ebaõnnestub, oleks täielik käsu näide järgmiselt:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Lõpuks lugege läbi krüptimise oleku uuesti, et kinnitada, et teie virtuaalse ketast on nüüd krüptitud. Järgmises näites kontrollib nimega VM olekut `myVM` klõpsake soovitud `myResourceGroup` ressursirühm:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Ketta krüptimise ülevaade
Virtuaalne ketast Linux VMs kohta on krüptitud [dm-crypt](https://wikipedia.org/wiki/Dm-crypt)abil. On tasuta krüptimise virtuaalse ketast Azure. Cryptographic klahvid on talletatud Azure'i klahvi Vault abil tarkvara kaitse või saate importida või luua oma võtmed riistvara turvalisus moodulid (HSMs) sertifitseeritud FIPS 140-2 tase 2 standarditele. Saate määrata nende cryptographic klahvide ja saate auditeerida nende kasutamist. Nende cryptographic võtit krüptimiseks ja dekrüptida virtuaalse ketast, mis on seotud teie VM. Azure Active Directory lõpp võimaldab turvalist cryptographic klahve välja nagu VMs on sisse lülitatud ja välja.

Krüptimise VM protsess on järgmine:

1. Luua krüptimisvõtme on Azure klahvi Vault.
2. Konfigureerige krüptimisvõtme saaks kasutada krüptimiseks ketast.
3. Lugeda cryptographic võti hoidlast Azure'i klahvi, luua lõpp abil Azure Active Directory, kellel on vajalikud õigused.
4. Küsimus käsk krüptimiseks oma virtuaalse ketast, Azure Active Directory lõpp-punkti ja sobivat cryptographic võtit kasutada.
5. Azure Active Directory lõpp-punkti taotleb nõutav krüptimisvõtme Azure'i klahvi hoidlast.
6. Virtuaalne ketast on krüptitud esitatud krüptimisvõtme.


## <a name="supporting-services-and-encryption-process"></a>Toetavad teenused ja krüptimine
Ketta krüptimise tugineb järgmised täiendavad komponendid:

- **Azure'i klahvi Vault** - cryptographic klahvid kaitsmiseks kasutada ja kasutada kettapuhastusriista krüptimine/dekrüptimine protsessi saladusi. 
  - Kui see on olemas, saate kasutada olemasolev Azure'i klahvi Vault. Teil pole ketast krüptimise võti Vault suunata.
  - Eraldi haldus piirmäärad ja võtme nähtavuse, saate luua sihtotstarbeline klahvi Vault.
- **Azure Active Directory** - tegeleb vajalikud cryptographic võtmed ja autentimise jaoks nõutud toimingute turvaline vahetamine. 
  - Tavaliselt saate kasutada olemasolevaid Azure Active Directory eksemplar rakenduse pidamiseks. 
  - Rakendus on rohkem lõpp klahvi hoidla ja virtuaalse masina teenuste taotleda ja saada välja antud korral cryptographic võtmed. Teil on välja tegelik rakendus, mis ühendab Azure Active Directory.


## <a name="requirements-and-limitations"></a>Nõuded ja piirangud
Toetatud stsenaariumid ja ketta krüptimise nõuded.

- Järgmise Linux serveri SKU-de jaoks – Ubuntu, CentOS, SUSE ja SUSE Linux Enterprise Server (SLES) ja punane rolli Enterprise Linux.
- Kõik ressursse (nt klahvi Vault, salvestusruumi konto ja VM) peab olema sama Azure piirkond ja tellimus.
- Standardse A, D, DS, G ja GS sarja VMs.

Ketta krüptimise ei toeta praegu järgmistel juhtudel:

- Tavaline taseme VMs.
- Klassikaline juurutamise mudeli abil loodud VMs.
- Keelamine OS ketta krüptimise Linux VMs.
- Värskendamine on juba krüptitud Linux VM cryptographic klahvireas.


## <a name="create-the-azure-key-vault-and-keys"></a>Looge Azure'i klahvi hoidla ja võtmed
Täitke ülejäänud juhendi, peate [Uusima Azure'i CLI](../xplat-cli-install.md) installitud ja sisse loginud kasutades režiimi ressursihaldur järgmiselt:

```
azure config mode arm
```

Kogu käskude näited, asendage kõik näide parameetrid oma nimed, asukoha ja väärtused. Järgmistes näidetes kasutatakse on mess, `myResourceGroup`, `myKeyVault`, `myAADApp`jne.

Esimene samm on luua mõne Azure'i klahvi Vault salvestada oma cryptographic võtmed. Azure'i klahvi Vault saate talletada klahvid, saladusi või paroole, mis võimaldavad teil turvaliselt rakendada neid oma rakendused ja teenused. Virtuaalse ketta krüptimine, kasutage klahvi Vault krüptimisvõtme, mida kasutatakse krüptida või dekrüptida oma virtuaalse ketast talletamiseks. 

Azure'i klahvi Vault pakkujale Azure tellimuse lubamine ja seejärel ressursi rühma loomine. Järgmises näites luuakse ressursirühma nimega `myResourceGroup` klõpsake soovitud `WestUS` asukoht:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Azure'i klahvi Vault cryptographic võtmed ja seotud Arvuta ressursse, nt salvestusruumi ja ise VM sisaldavad peab asuma piirkonna. Järgmises näites luuakse Azure'i klahvi Vault nimega `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Saate salvestada cryptographic klahvide abil tarkvara või riistvara turvalisus mudeli (HSM) kaitse. Kasutades HSMI nõuab lisatasu võti Vault. On lisatasu lisatasu võti Vault asemel standard võti hoidla, mis talletab tarkvara kaitstud klahvid loomiseks. Lisatasu võti Vault loomiseks eelmises etapis lisada `--sku Premium` käsk. Järgmises näites kasutatakse tarkvara kaitstud klahvid, kuna me loodud standard võti Vault. 

Nii kaitse mudelid, peab Azure'i platvormi juurdepääsu taotlemiseks cryptographic klahvid, kui VM saapad dekrüptida virtuaalse ketast. Luua krüptovõtme jooksul oma Vault klahvi ja seejärel lubamine kasutamine virtuaalse ketta krüptimise abil. Järgmises näites luuakse nimega klahvi `myKey` ja selle ketta krüptimiseks lubab:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Azure Active Directory rakenduse loomine
Kui virtuaalse krüptitud või dekrüptida, kasutage lõpp autentimise ja vahetamise cryptographic klahvid võti hoidlast. Selle lõpp-punkti Azure Active Directory rakendus võimaldab Azure'i platvormi taotleda korral cryptographic võtmed nimel VM. Vaikimisi Azure Active Directory eksemplar on saadaval tellimuse, ehkki paljud ettevõtted on pühendunud Azure Active Directory kataloogide.

Kui loote rakenduse täisversioon Azure Active Directory, on `--home-page` ja `--identifier-uris` parameetrite järgmises näites ei pea olema tegelik marsruuditavaid aadress. Järgmises näites määratakse ka parooli vastavalt salajane asemel Azure portaali loomisel võtmed. Sel ajal, kui ei saa Azure'i CLI teha genereerimine võtmed. 

Azure Active Directory rakenduse loomine Järgmises näites luuakse rakenduse nimega `myAADApp` ja parooli, kasutab `myPassword`. Enda parooli määrata järgmiselt:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Märkige soovitud `applicationId` mis tagastatakse väljund eelmise käsu kaudu. Selle rakenduse ID kasutatakse ülejäänud juhised. Järgmisena Looge teenuse põhilist nime (SPN) nii, et rakendus on teie keskkonnas kättesaadavaks. Edukalt krüptida või dekrüptida virtuaalse, õiguste krüptimisvõtme, mis on talletatud klahvi Vault peab olema seatud Azure Active Directory rakendamiseks lugeda võtmed. 

SPN loomine ja seadke vajalikud õigused järgmiselt:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Lisage virtuaalse ketta ja krüptimise oleku vaatamine
Mõne virtuaalse ketta tegelikult krüptimiseks võimaldab kettal lisamiseks mõne olemasoleva VM. Lisage mõne olemasoleva VM 5Gb andmeid kettal järgmiselt:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Virtuaalne ketast pole praegu krüptitud. Teie VM krüptimise hetkeseisu ülevaatamine järgmiselt:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Virtuaalse krüptimine
Nüüd krüptimiseks virtuaalse ketast, saate koondada kõik eelmise komponendid:

1. Määrake Azure Active Directory rakenduste ja parool.
2. Määrake klahvi Vault oma krüptitud ketast metaandmed salvestada.
3. Määrake cryptographic klahvid tegelik krüptimine ja dekrüptimine jaoks.
4. Määrake, kas soovite krüptida OS ketas, andmete ketast või kõik.

Võimaldab vaadata üksikasjad oma Azure'i klahvi hoidla ja olete loonud, kui teil on vaja võtme Vault ID URI-d ja seejärel sisestage URL-i Viimane toiming võti:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Krüpti oma virtuaalse ketast, kasutades väljund on `azure keyvault show` ja `azure keyvault key show` käske järgmiselt:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Eelmise käsu on paljude muutujate järgmises näites on lõpule viidud käsk viide:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Azure'i CLI ei paku Paljusõnaline vigade krüptimine käigus. Tõrkeotsingu kohta lisateabe saamiseks vaadake `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` krüptitud VM.

Lõpuks abil krüptimise oleku uuesti, et kinnitada, et teie virtuaalse ketast on nüüd krüptitud.

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Täiendavate andmete ketast lisamine
Kui teil on krüptitud oma andmete ketast, saate hiljem lisada täiendavad virtuaalse ketast oma VM ja ka need krüptida. Kui käivitate selle `azure vm enable-disk-encryption` käsk, inkrementida jada versiooni abil soovitud `--sequence-version` parameeter. Parameeter järjestus versiooni saate korrata toiminguid sama VM.

Näiteks võimaldab lisada teise virtuaalse ketta oma VM järgmiselt:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Käivitage uuesti käsku Krüpti virtuaalse kellaaja lisamine on `--sequence-version` parameetri ja suurendades meie esimese Käivita väärtuse järgmiselt:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Järgmised sammud

- Azure'i klahvi Vault, sh cryptographic võtmed ja võlvid, kustutamine haldamise kohta lisateavet teemast [haldamine klahvi Vault kasutamine CLI](../key-vault/key-vault-manage-with-cli.md).
- Lisateavet ketta krüptimise, näiteks ettevalmistamine krüptitud kohandatud VM üleslaadimine Azure, leiate [Azure'i ketta krüptimise](../security/azure-security-disk-encryption.md).