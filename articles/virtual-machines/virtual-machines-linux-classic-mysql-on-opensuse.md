<properties
    pageTitle="MySQL-i installimine on OpenSUSE VM | Microsoft Azure'i"
    description="Vaadake, kuidas installida seadmesse OpenSUSE Linuxi VMirtual Azure MySQL-i."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Töötab OpenSUSE Linuxi Azure MySQL-i installimine

[MySQL-i] [ MySQL] on populaarne, avatud allika SQL-andmebaasi. Selle õpetuse näidatakse, kuidas luua OpenSUSE Linux töötab, siis MySQL-i installimine.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


<br>


## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Töötab OpenSUSE Linuxi loomine

[AZURE.INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Installimine ja käivitamine MySQL-i virtual arvutisse

[AZURE.INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Järgmised sammud
MySQL-i kohta leiate üksikasjalikumat teavet teemast [MySQL-i dokumentatsiooni][MySQLDocs].

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com

