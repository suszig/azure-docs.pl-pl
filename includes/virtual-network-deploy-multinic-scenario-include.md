## <a name="scenario"></a>Scenariusz
Ten dokument przeprowadzi wdrożenia korzystającego z wielu kart sieciowych w maszynach wirtualnych w danego scenariusza. W tym scenariuszu należy obciążenia IaaS dwuwarstwowej hostowana na platformie Azure. Każda warstwa została wdrożona w jego własnej podsieci w sieci wirtualnej (VNet). Warstwy frontonu składa się z kilku serwerów sieci web, zgrupowane w ustawić wysoką dostępność usługi równoważenia obciążenia. Warstwa zaplecza składa się z kilku serwerów baz danych. Te serwery baz danych zostanie wdrożony z dwiema kartami sieciowymi, jeden dla dostępu do bazy danych, innych do zarządzania. Scenariusz obejmuje także grup zabezpieczeń sieci (NSG), aby kontrolować, jaki ruch jest dozwolony w każdej podsieci i karty Sieciowej w ramach wdrożenia. Na poniższym rysunku przedstawiono podstawowej architektury tego scenariusza.  

![Scenariusz MultiNIC](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

