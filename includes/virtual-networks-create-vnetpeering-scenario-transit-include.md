## <a name="service-chaining-transit-through-peered-vnet"></a>Tworzenie łańcucha usług — przesył przy użyciu skomunikowanej równorzędnie sieci wirtualnej
Chociaż korzystanie z tras systemowych automatycznie umożliwia ruch sieciowy we wdrożeniu, istnieją przypadki, w których użytkownik chce sterować przekazywaniem pakietów przez urządzenia wirtualne.
W tym scenariuszu mamy do czynienia z dwiema sieciami wirtualnymi w subskrypcji, HubVNet i VNet1, co zostało przedstawione na poniższym diagramie. Wirtualne urządzenie sieciowe (NVA) zostanie wdrożone w sieci wirtualnej HubVNet. Po ustanowieniu komunikacji równorzędnej między sieciami wirtualnymi HubVNet i VNet1 można skonfigurować trasy definiowane przez użytkownika i określić następny przeskok do urządzenia NVA w sieci wirtualnej HubVNet.

![Przesył NVA](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [!NOTE]
> Dla uproszczenia przyjęto założenie, że wszystkie przykładowe sieci wirtualne znajdują się w tej samej subskrypcji. Ale wszystko będzie też działać dla scenariusza komunikacji między subskrypcjami.
> 
> 

Kluczową właściwością pozwalającą włączyć routing przesyłania jest parametr „Zezwalaj na ruch przekazywany”. Dzięki temu możliwe jest akceptowanie i wysyłanie ruchu z i do urządzenia NVA w skomunikowanej równorzędnie sieci wirtualnej.  



<!--HONumber=Nov16_HO2-->


