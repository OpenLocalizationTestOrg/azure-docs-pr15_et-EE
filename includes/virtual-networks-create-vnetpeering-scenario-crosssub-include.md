## <a name="peering-across-subscriptions"></a>Silmitsemine tellimustes

Selle stsenaariumi loote silmitsemine kaks VNets kuuluvate erinevate tellimuste vahel.

![rist sub stsenaarium](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

VNet silmitsemine tugineb Rollipõhine juurdepääsu reguleerimine (RBAC) luba. Rist-tellimuste stsenaarium, peate esmalt piisavaid õigusi anda kasutajatele, kes loob silmitsemine link:

> [AZURE.NOTE] Kui sama kasutaja on nii tellimuste üle, siis saate vahele jätta allpool 1. etapp – 4.
