## <a name="scenario"></a>Scenariusz
Aby lepiej zilustrować tworzenie Udr, ten dokument użyje scenariusz poniżej.

![OPIS ILUSTRACJI](./media/virtual-network-create-udr-scenario-include/figure1.png)

W tym scenariuszu utworzysz jeden przez dla *Front end podsieci* i przez inny dla *podsieci zakończenia Wstecz* , zgodnie z poniższym opisem: 

* **Frontonu przez**. Fronton będzie dotyczyć przez *frontonu* podsieci i może zawierać jedną trasę:    
  * **RouteToBackend**. Ta trasa wyśle cały ruch do podsieci zaplecza, aby **FW1** maszyny wirtualnej.
* **Wewnętrznej bazy danych przez**. Wewnętrzny przez zostaną zastosowane do *zaplecza* podsieci i może zawierać jedną trasę:    
  * **RouteToFrontend**. Ta trasa wyśle cały ruch do podsieci frontonu, aby **FW1** maszyny wirtualnej.

Kombinacja te trasy zapewni, że cały ruch kierowany z jednej podsieci do drugiej będą kierowane do **FW1** maszyny wirtualnej, która jest używana jako urządzenie wirtualne. Należy również włączyć funkcję przesyłania dalej IP dla tej maszyny Wirtualnej, aby upewnić się, że może ona odbierać ruchu kierowanego do innych maszyn wirtualnych.

