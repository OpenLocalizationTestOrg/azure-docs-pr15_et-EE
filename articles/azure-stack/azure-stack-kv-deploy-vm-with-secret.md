<properties
    pageTitle="Azure'i virnas klahvi Vault talletatud parooliga VM juurutamine | Microsoft Azure'i"
    description="Saate teada, kuidas Azure'i virnas klahvi Vault talletatud parooliga VM juurutamine"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>Juurutada VM talletatud klahvi Vault parooli toomine

Kui peate juurutamisel parameetrina turvaline väärtust (nt parooli) edastada, saate talletada selle väärtuse salajase, mis on Azure virnas võtme võlvkelder ja viidata väärtuse muud Azure'i ressursihaldur mallid. Saate lisada ainult viide salajane malli nii salajane kunagi on avatud. Teil pole vaja käsitsi sisestage väärtus on salajane iga kord, kui juurutate ressursside. Saate määrata, millised kasutajad või teenuse põhisumma pääsevad salajane.

## <a name="reference-a-secret-with-static-id"></a>Viide salajase staatilise ID-ga

Viidatava salajane edastab väärtused malli parameetrid faili kaudu. Salajane viidatava sooritab ressursiidentifikaator võtme Vault ja salajane nimi. Selles näites võtme vault salajane peab juba olemas. Staatiline väärtus kasutamiseks selle ressursi ID-ga.

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]Parameeter, mis aktsepteerib salajane peaks olema on *securestring*.

## <a name="next-steps"></a>Järgmised sammud
[Klahv Vault koos valimi minirakenduse juurutamine](azure-stack-kv-sample-app.md)

[Klahv Vault sertifikaadiga VM juurutamine](azure-stack-kv-push-secret-into-vm.md)

