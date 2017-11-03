## <a name="sample-scenario"></a>Przykładowy scenariusz
Aby lepiej zilustrować Zarządzanie grup NSG, w tym artykule wykorzystano scenariusz poniżej.

![Scenariusz sieci wirtualnych](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

W tym scenariuszu utworzysz grupy NSG dla każdej podsieci w **TestVNet** sieci wirtualnej, zgodnie z opisem poniżej: 

* **Grupa NSG frontonu**. Fronton będzie dotyczyć NSG *frontonu* podsieci i zawiera dwie reguły:    
  * **Reguła RDP**. Ta reguła zezwala na ruch RDP do *frontonu* podsieci.
  * **Reguła sieci Web**. Ta reguła umożliwia ruch HTTP do *frontonu* podsieci.
* **Grupa NSG zaplecza**. Wewnętrzna grupa NSG zostaną zastosowane do *zaplecza* podsieci i zawiera dwie reguły:    
  * **Reguła SQL**. Ta reguła zezwala na ruch SQL tylko z *frontonu* podsieci.
  * **Reguła sieci Web**. Ta reguła nie zezwala na wszystkie internet powiązany ruch z *zaplecza* podsieci.

Kombinacja tych zasad tworzenia scenariusza przypominającej DMZ, gdzie może odbierać ruchu przychodzącego ruchu SQL z podsieci frontonu podsieci wewnętrznej, a nie ma dostępu do Internetu, gdy podsieci frontonu komunikacji z Internetem i otrzymywać przychodzące żądania HTTP tylko.

Aby wdrożyć scenariusz opisany powyżej, wykonaj [to łącze](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), kliknij przycisk **wdrażanie na platformie Azure**, Zastąp domyślne wartości parametrów, jeśli to konieczne i postępuj zgodnie z instrukcjami w portalu. W poniższe instrukcje przykładowy szablon został użyty do wdrożenia nazwy grup zasobów **NSG zarządcy zasobów**. 

