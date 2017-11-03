## <a name="scenario"></a>Scenariusz
Aby lepiej zilustrować tworzenie grup NSG, ten dokument użyje scenariusz poniżej.

![Scenariusz sieci wirtualnych](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

W tym scenariuszu utworzysz grupy NSG dla każdej podsieci w **TestVNet** sieci wirtualnej, zgodnie z opisem poniżej: 

* **Grupa NSG frontonu**. Fronton będzie dotyczyć NSG *frontonu* podsieci i zawiera dwie reguły:    
  * **Reguła RDP**. Ta reguła zezwala na ruch RDP do *frontonu* podsieci.
  * **Reguła sieci Web**. Ta reguła umożliwia ruch HTTP do *frontonu* podsieci.
* **Grupa NSG zaplecza**. Wewnętrzna grupa NSG zostaną zastosowane do *zaplecza* podsieci i zawiera dwie reguły:    
  * **Reguła SQL**. Ta reguła zezwala na ruch SQL tylko z *frontonu* podsieci.
  * **Reguła sieci Web**. Ta reguła nie zezwala na wszystkie internet powiązany ruch z *zaplecza* podsieci.

Kombinacja tych reguł tworzenie scenariusza przypominającej DMZ, gdzie podsieci zaplecza może odbierać ruchu przychodzącego dla serwera SQL z podsieci frontonu i nie ma dostępu do Internetu, gdy podsieci frontonu komunikacji z Internetem i otrzymywać przychodzące żądania HTTP tylko.

