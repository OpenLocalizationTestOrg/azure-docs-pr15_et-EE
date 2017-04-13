<properties
   pageTitle="Azure'i teenus container juhtida kasutajaliides web | Microsoft Azure'i"
   description="Ümbriste juurutada teenuse Azure'i teenus kobar maraton veebi Kasutajaliidese kaudu."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Keskmise suurusega, ümbriste, mikro-teenuste Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/19/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-web-ui"></a>Container halduse UI veebi kaudu

Näiteks Põhiliselt/OS pakub juurutamine ja skaleerimist rühmitatud töökoormus, samal ajal niisutussüsteemide aluseks oleva riistvara keskkonnas. AV/OS peal on framework, haldab plaanimis- ja täitmise Arvuta töökoormus.

Raamistiku on saadaval paljude populaarsete töökoormus, kirjeldab seda dokumenti saate luua ja mastaapimiseks container saama maraton. Enne töö kaudu nendes näidetes, peate AV/OS klaster, mis on konfigureeritud Azure'i teenus. Peate olema selle arvutikobaras remote connectivity. Nendeks kohta lisateabe saamiseks lugege järgmisi artikleid:

- [Azure'i teenus on kobar juurutamine](container-service-deployment.md)
- [Ühenduse loomine mõne Azure'i teenus kobar](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>Näiteks Põhiliselt/OS UI uurimine

Liikuge Secure Shell (SSH) tunneliga asukoht, kus http://localhost/. See laadib AV/OS web UI ja kuvatud Teave kobar, nt kasutatud ressursid, aktiivse ja käivitatud teenuste kohta.

![NÄITEKS PÕHILISELT/OS KASUTAJALIIDES](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Tutvumine maraton kasutajaliides

Maraton UI vaatamiseks liikuge http://localhost/Marathon. Selle kuval saate alustada uut container või mõnda muusse rakendusse Azure'i Container teenuse AV/OS klaster. Näete tarkvarakomplektiga ümbriste ja rakendused.  

![Maraton kasutajaliides](media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Keskmise suurusega vormindatud container juurutamine

Juurutage uus ümbris maraton abil, nuppu **Teenuserakenduse loomine** ja sisestage järgmine teave vormile:

Väli           | Väärtus
----------------|-----------
ID              | Nginx
Pilt           | Nginx
Võrgu         | Ületada
Host Port       | 80
Protocol (protokoll)        | TCP

![Uue rakenduse Kasutajaliidese - üldine](media/dcos/dcos4.png)

![Uue rakenduse UI--keskmise suurusega Container](media/dcos/dcos5.png)

![Uue rakenduse Kasutajaliidese - pordid ja teenuse tuvastamine](media/dcos/dcos6.png)

Kui soovite vastendada staatiliselt container pordi pordiga agent, peate kasutama JSON režiim. Selleks lülitumine uue rakenduse viisardi **JSON** tumblernupu abil. Sisestage jaotises järgmist funktsiooni `portMappings` rakenduse määratluse osas. Selles näites seob ümbris pordi 80 pordi 80 AV/OS agendi. Pärast selle muudatuse saate vahetada viisardi välja JSON režiim.

```none
"hostPort": 80,
```

![Uue rakenduse UI--pordi 80 näide](media/dcos/dcos13.png)

Näiteks Põhiliselt/OS kobar juurutatakse era- ja agentide kogumiga. Kobar saama kasutada rakendusi Interneti kaudu, tuleb teil juurutada rakendused avalikku esindajale. Selleks valige vahekaart **Valikuline** viisardi Uus rakendus ja sisestage **slave_public** **Aktsepteeritud ressursi rollid**.

![Uue rakenduse UI--avalik agent seadmine](media/dcos/dcos14.png)

Tagasi avalehel maraton, näete ümbris juurutamise olek.

![Maraton põhilehe UI--container juurutamise olek](media/dcos/dcos7.png)

Kui aktiveerite AV/OS web UI (http://localhost/), näete, et AV/OS kobar töötab tööülesande (sel juhul keskmise suurusega vormindatud container).

![Näiteks Põhiliselt/OS web UI--töötavate klaster ülesanne](media/dcos/dcos8.png)

Saate vaadata ka kobar sõlm, mis töötab tööülesanne.

![Tööülesande kobar sõlm AV/OS web UI--](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Teie ümbriste skaala

Saate maraton UI mastaapimiseks ümbris eksemplari arvu. Selleks, liikuge lehele, **maraton** , valige ümbris, mida soovite skaala ja **skaala** nuppu. Dialoogiboksis **Skaala rakenduse** Sisestage soovitud container eksemplaride arv ja valige **Rakendus, skaala**.

![Maraton UI--skaala rakenduse dialoogiboks](media/dcos/dcos10.png)

Kui skaala toiming on lõpule jõudnud, kuvatakse sama tööülesande AV/OS agentide laiali mitmes eksemplaris.

![Näiteks Põhiliselt/OS web UI armatuurlaua--tööülesande ümbruses üle agentide](media/dcos/dcos11.png)

![Näiteks Põhiliselt/OS web UI--sõlmed](media/dcos/dcos12.png)

## <a name="next-steps"></a>Järgmised sammud

- [Näiteks Põhiliselt/OS ja maraton API töötamine](container-service-mesos-marathon-rest.md)

Sügav sukelduda koos Mesos teenuses Azure Container

> [AZURE'I. Azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos VIDEO]]
