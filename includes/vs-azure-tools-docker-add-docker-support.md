1. Visual Studio **Lahenduste Explorer**, paremklõpsake projekti ja valige **Lisa > keskmise suurusega tugi** kontekstimenüüst.

    ![Keskmise suurusega tugi kontekstimenüü lisamine](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Keskmise suurusega toe lisamine on ASP.net-i Core web project tulemuseks mitu keskmise suurusega seotud failide lisamist projekti, sealhulgas keskmise suurusega koostamine faile, juurutamise Windows PowerShelli skriptide ja keskmise suurusega atribuudi failide lisamine. 

    ![Keskmise suurusega faile lisada projekti](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE][Keskmise suurusega Windows Beta](https://beta.docker.com)kasutamisel avage Properties\Docker.props, eemaldage vaikeväärtus ja Visual Studio väärtuseks muudatuste jõustumiseks taaskäivitage.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
