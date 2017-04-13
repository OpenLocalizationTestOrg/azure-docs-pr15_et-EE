Järgmises tabelis on loetletud PolicyBased ja RouteBased VPN lüüside nõuded. Selles tabelis kehtib ressursihaldur ja klassikaline juurutamise mudelid. Klassikaline mudel PolicyBased VPN lüüsid on sama, mis staatiline lüüside ja lüüside marsruutimiseks-põhine on samad, mis dünaamiline lüüsid.


|   | **PolicyBased Basic VPN-lüüsi** | **RouteBased Basic VPN-lüüsi** | **RouteBased Standard VPN-lüüsi**   | **RouteBased suure jõudlusega VPN-lüüsi** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Saitide ühenduvuse (S2S)**  | PolicyBased VPN-i konfigureerimine        | RouteBased VPN-i konfigureerimine  | RouteBased VPN-i konfigureerimine     | RouteBased VPN-i konfigureerimine    |
| **SharePointi saidil Ühenduvus (P2S**)      | Pole toetatud   | Toetatud (saate kõrvuti S2S)  | Toetatud (saate kõrvuti S2S)  | Toetatud (saate kõrvuti S2S) |
| **Autentimismeetodi määramine**                 |    Eelnevalt jagatud võti  | Eelnevalt jagatud võti S2S Ühenduvus serdid P2S Ühenduvus | Eelnevalt jagatud võti S2S Ühenduvus serdid P2S Ühenduvus | Eelnevalt jagatud võti S2S Ühenduvus serdid P2S Ühenduvus |
| **Suurim lubatud arv S2S ühendused**       | 1                              | 10                                                                    | 10                                | 30                               |
| **Suurim lubatud arv P2S ühendused**       | Pole toetatud                  | 128                                                                   | 128                               | 128                              |
|**Aktiivse marsruutimise tugi (BGP)**           | Pole toetatud                  | Pole toetatud                                                         | Toetatud                     | Toetatud                   |
 
