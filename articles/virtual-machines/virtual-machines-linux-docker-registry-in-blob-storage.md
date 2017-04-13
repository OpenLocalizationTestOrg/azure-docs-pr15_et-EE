<properties 
  pageTitle="Oma privaatne keskmise suurusega registri Azure juurutamine | Microsoft Azure'i"
  description="Kirjeldab, kuidas saate kasutada keskmise suurusega registri majutada oma container piltide Azure'i bloobimälu teenuse."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="timlt"
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="09/27/2016" 
  ms.author="ahmetb" />

# <a name="deploying-your-own-private-docker-registry-on-azure"></a>Oma privaatne keskmise suurusega registri Azure juurutamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



Dokumendis kirjeldatakse, mis on keskmise suurusega privaatne registri on ja näitab, kuidas saate juurutada keskmise suurusega registri 2.0 container pilt keskmise suurusega privaatne registri Microsoft Azure Azure'i bloobimälu abil.

Selle dokumendi eeldab:

1. Saate teada, kuidas kasutada keskmise suurusega ja keskmise suurusega pilte talletamiseks. (Teil pole? [Lisateavet keskmise suurusega](https://www.docker.com))
2. Teil on serveris, kus on installitud keskmise suurusega mootor. (Teil pole? [Teha kiiresti Azure.](https://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## <a name="what-is-a-private-docker-registry"></a>Mis on privaatne keskmise suurusega registri?

Selleks, et saata konteinerite rakenduste pilveteenusesse, koostada keskmise suurusega container pilt ja salvestada selle kuskil nii, et seda saab kasutada endale ja teistele. 

Kuigi container pildi loomise ja saatmise see pilveteenuses on lihtne, on probleemiks salvestada loodud pildi usaldusväärselt. Seetõttu keskmise suurusega pakub tsentraliseeritud teenust nimega [Keskmise suurusega keskuse] [ docker-hub] talletamiseks pilve pilte ja võimaldab teil luua ümbriste nende piltide kasutamise igal ajal.

Kuigi [Keskmise suurusega jaoturi] [ docker-hub] on tasuline salvestamiseks oma isiklike rakenduse ümbris pilte, keskmise suurusega mõttes arendajate vajadustele ja pakub on avatud lähtekoodi tööriistakomplekt salvestada oma pilte oma isiklike keskmise suurusega registri tulemüüriga või kohapealse ilma on pihta avaliku Interneti.
Kuna Azure'i bloobimälu on lihtne turvaline, saate selle kiiresti luua ja kasutada privaatne keskmise suurusega registri Azure, mille saate ise määrata.

## <a name="why-should-you-host-a-docker-registry-on-azure"></a>Miks majutate keskmise suurusega registri Azure?

Majutada oma keskmise suurusega registri eksemplari Microsoft Azure ja Azure'i bloobimälu oma piltide talletamine, võib olla järgmised eelised.

**Turvalisus:** Teie keskmise suurusega pilte ei jäta Azure andmekeskuste, nii, et nad ületavad avaliku Interneti, kui kasutasite keskmise suurusega jaoturi.
  
**Jõudluse:** Sama andmekeskuse või piirkonnas rakenduste salvestatakse teie keskmise suurusega pilte. See tähendab, et pildid tõmmatakse kiirem ja usaldusväärselt võrreldes keskmise suurusega jaoturi.

**Töökindluse:** Microsoft Azure'i bloobimälu abil saate kasutada palju salvestusruumi atribuutide nagu kõrge kättesaadavus, koondamise premium salvestusruumi (SSD) jne.

## <a name="configuring-docker-registry-to-use-azure-blob-storage"></a>Keskmise suurusega registri kasutada Azure'i bloobimälu konfigureerimine

(See on soovitatav [keskmise suurusega registri 2.0 dokumentatsiooni][registri-dokumendid] enne jätkamist lugege.)

Saate [konfigureerida] [ registry-config] oma keskmise suurusega registri kahel viisil.
Sa saad:

1. Kasutage mõnda `config.yml` faili. Sel juhul peate looma eraldi keskmise suurusega pildi peale `registry` pilt.
2. Alistada kaudu keskkonna muutujate konfiguratsiooni vaikefail: saab asju teinud ilma loomine ja haldamine eraldi keskmise suurusega pildi.

Selles teemas järgib lihtsuse suvand 2, keskkonna muutujate abil.

Selleks, et käivitada keskmise suurusega registri astme:

* kasutab Azure salvestusruumi konto piltide talletamine
* kuulab virtuaalse masina porti 5000
* pole konfigureeritud autentimine (pole soovitatav, vt allpool märkust)

Käivitage järgmine käsk keskmise suurusega oma bash terminalis (asendamine `<storage-account>` ja `<storage-key>` mandaadiga):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Kui käsk väljub, näete majutada oma isiklike keskmise suurusega registri eksemplari käivitades ümbris on `docker ps` oma hosti käsk:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Keskmise suurusega registri turvalisus konfigureerimine ei kuulu selles dokumendis ja oma registrit saab kättesaadav kõigile ilma autentimine vaikimisi, kui avate pordi registri pordi virtuaalse masina lõpp-punkti üles või laadite koormusetasakaalustusteenuse ülaltoodud juurutamise käsu kasutamisel.
>
> Lugege [Konfigureerimise keskmise suurusega registri] [ registry-config] dokumentatsiooni saate teada, kuidas registri eksemplar ja teie pilte.

## <a name="next-steps"></a>Järgmised sammud

Kui olete oma registri luua, on aeg minna seda veel kasutada. Alustage keskmise suurusega [registri-dokumendid]. 

[docker-hub]: https://hub.docker.com/
[registry]: https://github.com/docker/distribution
[registri-dokumendid]: http://docs.docker.com/registry/
[registry-config]: http://docs.docker.com/registry/configuration/
 
