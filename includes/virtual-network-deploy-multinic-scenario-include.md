## <a name="scenario"></a>Stsenaarium

Selle dokumendi juhendab juurutus, mis kasutab mitu NICs VMs konkreetse stsenaariumi. Selle stsenaariumi korral peate kahepoolse mitmetasandilise IaaS töökoormus majutatud Azure. Iga taseme juurutatakse oma alamvõrgu virtuaalse võrku (VNet). Esiosa taseme koosneb mitu web serverid, rühmitatud laadi koormusetasakaalustusteenuse, kõrge-saadavus seadmine. Tagasi end taseme koosneb mitu andmebaasi serverid. Need andmebaasi serverid juurutatakse koos kahe NICs iga, üks andmebaasi juurdepääsu, teine haldus. Stsenaarium ka juurutamise võrgu turberühmad (NSGs) kontrollida liikluse on lubatud iga alamvõrku, asukoht ja. Alloleval joonisel lihtsa arhitektuur selle stsenaariumi.  

![MultiNIC stsenaarium](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

