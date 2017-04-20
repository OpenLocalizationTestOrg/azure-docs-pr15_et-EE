Azure'i koormusetasakaalustus on kiht-4 (TCP, UDP) load koormusetasakaalustusteenuse. Koormusetasakaalustus pakub kõrge kättesaadavus jagades liiklust terve teenus juhul pilveteenused või virtuaalarvutid koormuse koormusetasakaalustusteenuse kogumist. Azure'i koormusetasakaalustus võib esitada ka mitu porti, mitu IP-aadressi või teenuslepingut.

Saate konfigureerida koormusetasakaalustuse, et:

* Laadida tasakaalu Interneti liiklust virtuaalarvuti (VM). Nimetatakse sel koormusetasakaalustus [Interneti poole suunatud koormusetasakaalustus](../articles/load-balancer/load-balancer-internet-overview.md).
* Koormuse tasakaalu liikluse vahel VM virtuaalse võrgu (VNet), pilveteenuste VMs või asutusesisese arvutid ja VMs on virtuaalse võrgu vahel. Nimetatakse sel koormusetasakaalustus [sisemine koormusetasakaalustus (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Edasi välise liiklust VM eksemplariga.
