# Omówienie
## [Sieci wirtualne](virtual-networks-overview.md)
## [Routing](virtual-networks-udr-overview.md)
## [Wirtualne sieci równorzędne](virtual-network-peering-overview.md)
## [Punkty końcowe usługi sieci wirtualnej](virtual-network-service-endpoints-overview.md)
## [Sieć wirtualna dla usług platformy Azure](virtual-network-for-azure-services.md)
## [Bezpieczeństwo](security-overview.md)
## [Ciągłość działalności biznesowej:](virtual-network-disaster-recovery-guidance.md)
## [Adresowanie IP](virtual-network-ip-addresses-overview-arm.md)
## [Ochrona przed atakami DDOS](ddos-protection-overview.md)
## [Często zadawane pytania](virtual-networks-faq.md)
## Wdrożenie klasyczne
### [Adresowanie IP](virtual-network-ip-addresses-overview-classic.md)
### [Listy kontroli dostępu](virtual-networks-acl.md)

# Rozpoczęcie pracy
## [Tworzenie pierwszej sieci wirtualnej](virtual-network-get-started-vnet-subnet.md)

# Instrukcje
## Planowanie i projektowanie
### [Sieci wirtualne](virtual-network-vnet-plan-design-arm.md)
### [Sieciowe grupy zabezpieczeń](virtual-networks-nsg.md)

## Wdrażanie
### [Sieci wirtualne](virtual-networks-create-vnet-arm-pportal.md)
#### [Azure PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [Interfejs wiersza polecenia platformy Azure 2.0](virtual-networks-create-vnet-arm-cli.md)
#### [Interfejs wiersza polecenia platformy Azure 1.0](virtual-networks-create-vnet-cli-nodejs.md)
#### [Szablon](virtual-networks-create-vnet-arm-template-click.md)

### Grupy zabezpieczeń sieci
#### [Witryna Azure Portal](virtual-networks-create-nsg-arm-pportal.md)
#### [Azure PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [Interfejs wiersza polecenia platformy Azure 2.0](virtual-networks-create-nsg-arm-cli.md)
#### [Interfejs wiersza polecenia platformy Azure 1.0](virtual-networks-create-nsg-cli-nodejs.md)
#### [Szablon](virtual-networks-create-nsg-arm-template.md)
#### [Grupy zabezpieczeń aplikacji](create-network-security-group-preview.md)
#### Wdrożenie klasyczne
##### [Azure PowerShell](virtual-networks-create-nsg-classic-ps.md)
##### [Interfejs wiersza polecenia platformy Azure 1.0](virtual-networks-create-nsg-classic-cli.md)

### Trasy definiowane przez użytkownika
#### [Witryna Azure Portal](create-user-defined-route-portal.md)
#### [Azure PowerShell](virtual-network-create-udr-arm-ps.md)
#### [Interfejs wiersza polecenia platformy Azure 2.0](virtual-network-create-udr-arm-cli.md)
#### [Interfejs wiersza polecenia platformy Azure 1.0](virtual-network-create-udr-arm-cli-nodejs.md)
#### [Szablon](virtual-network-create-udr-arm-template.md)
#### Wdrożenie klasyczne
##### [Azure PowerShell](virtual-network-create-udr-classic-ps.md)
##### [Interfejs wiersza polecenia platformy Azure](virtual-network-create-udr-classic-cli.md)

### Wirtualne sieci równorzędne
#### [Same deployment model - same subscription (Ten sam model wdrażania — ta sama subskrypcja)](virtual-network-create-peering.md)
#### [Same deployment model - different subscriptions (Ten sam model wdrażania — różne subskrypcje)](create-peering-different-subscriptions.md)
#### [Different deployment models - same subscription (Różne modele wdrażania — ta sama subskrypcja)](create-peering-different-deployment-models.md)
#### [Different deployment models - different subscriptions (Różne modele wdrażania — różne subskrypcje)](create-peering-different-deployment-models-subscriptions.md)

### [Punkty końcowe usługi sieci wirtualnej](virtual-network-service-endpoints-configure.md)

### Publiczny adres IP — strefa dostępności
#### [Witryna Azure Portal](create-public-ip-availability-zone-portal.md)
#### [Interfejs wiersza polecenia platformy Azure](create-public-ip-availability-zone-cli.md)
#### [PowerShell](create-public-ip-availability-zone-powershell.md)

### Maszyny wirtualne
#### Tworzenie maszyny wirtualnej ze statycznym publicznym adresem IP
##### [Witryna Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [Azure PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [Interfejs wiersza polecenia platformy Azure 2.0](virtual-network-deploy-static-pip-arm-cli.md)
##### [Interfejs wiersza polecenia platformy Azure 1.0](virtual-network-deploy-static-pip-cli-nodejs.md)
##### [Szablon](virtual-network-deploy-static-pip-arm-template.md)
##### Wdrożenie klasyczne
###### [Azure PowerShell](virtual-networks-reserved-public-ip.md)

#### Tworzenie maszyny wirtualnej ze statycznym prywatnym adresem IP
##### [Witryna Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [Azure PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [Interfejs wiersza polecenia platformy Azure](virtual-networks-static-private-ip-arm-cli.md)
##### Wdrożenie klasyczne
###### [Witryna Azure Portal](virtual-networks-static-private-ip-classic-pportal.md)
###### [Azure PowerShell](virtual-networks-static-private-ip-classic-ps.md)
###### [Interfejs wiersza polecenia platformy Azure](virtual-networks-static-private-ip-classic-cli.md)

#### Tworzenie maszyny wirtualnej z wieloma interfejsami sieciowymi
##### [Azure PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Interfejs wiersza polecenia platformy Azure 2.0](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Interfejs wiersza polecenia platformy Azure 1.0](../virtual-machines/linux/multiple-nics-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Szablon](virtual-network-deploy-multinic-arm-template.md)

##### Wdrożenie klasyczne
###### [Azure PowerShell](virtual-network-deploy-multinic-classic-ps.md)
###### [Interfejs wiersza polecenia platformy Azure](virtual-network-deploy-multinic-classic-cli.md)

#### Tworzenie maszyny wirtualnej z wieloma adresami IP
##### [Witryna Azure Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [Azure PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [Interfejs wiersza polecenia platformy Azure 2.0](virtual-network-multiple-ip-addresses-cli.md)
##### [Interfejs wiersza polecenia platformy Azure 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md)
##### [Szablon](virtual-network-multiple-ip-addresses-template.md)

#### [Tworzenie maszyny wirtualnej z wydajniejszą siecią](virtual-network-create-vm-accelerated-networking.md)

### Scenariusze łączności
#### [Z sieci wirtualnej do sieci wirtualnej](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Z sieci wirtualnej (Resource Manager) do sieci wirtualnej (klasycznej)](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Z sieci wirtualnej do sieci lokalnej (VPN)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Z sieci wirtualnej do sieci lokalnej (ExpressRoute)](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Wysoko dostępna hybrydowa architektura sieci](../guidance/guidance-hybrid-network-expressroute-vpn-failover.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

### Scenariusze zabezpieczeń
#### [Zabezpieczanie sieci przy użyciu urządzeń wirtualnych](virtual-network-scenario-udr-gw-nva.md)
#### [Strefa DMZ między platformą Azure i Internetem](../guidance/guidance-iaas-ra-secure-vnet-dmz.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Zabezpieczenia usługi w chmurze i sieci](../best-practices-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Tworzenie strefy DMZ z sieciowymi grupami zabezpieczeń](virtual-networks-dmz-nsg.md)
##### [Tworzenie strefy DMZ z sieciowymi grupami zabezpieczeń (wersja klasyczna)](virtual-networks-dmz-nsg-asm.md)
##### [Tworzenie strefy DMZ z zaporą i sieciowymi grupami zabezpieczeń (wersja klasyczna)](virtual-networks-dmz-nsg-fw-asm.md)
##### [Strefa DMZ z zaporą, routingiem zdefiniowanym przez użytkownika i sieciowymi grupami zabezpieczeń (wersja klasyczna)](virtual-networks-dmz-nsg-fw-udr-asm.md)

##### [Przykładowa aplikacja](virtual-networks-sample-app.md)

### Wdrożenie klasyczne
#### [Sieć wirtualna](create-virtual-network-classic.md)
##### [Witryna Azure Portal](virtual-networks-create-vnet-classic-pportal.md)
##### [Azure PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md)
##### [Interfejs wiersza polecenia platformy Azure](virtual-networks-create-vnet-classic-cli.md)
#### [Określanie ustawień usługi DNS w pliku konfiguracji sieci wirtualnej](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
#### [Określanie ustawień usługi DNS w pliku konfiguracji usługi](virtual-networks-specifying-dns-settings-in-a-service-configuration-file.md)

## Konfigurowanie
### Maszyny wirtualne
#### [Dodawanie lub usuwanie interfejsów sieciowych](virtual-network-network-interface-vm.md)
#### [Rozpoznawanie nazw dla maszyn wirtualnych i usług w chmurze](virtual-networks-name-resolution-for-vms-and-role-instances.md)
#### [Używanie dynamicznej usługi DNS do rejestrowania nazw hostów na własnym serwerze DNS](virtual-networks-name-resolution-ddns.md)
#### [Optymalizowanie przepływności sieci](virtual-network-optimize-network-bandwidth.md)
#### [Wyświetlanie i modyfikowanie nazw hostów](virtual-networks-viewing-and-modifying-hostnames.md)
#### Wdrożenie klasyczne
##### Statyczne adresy IP
###### [PowerShell](virtual-networks-reserved-private-ip.md)
###### [Interfejs wiersza polecenia](virtual-networks-static-private-ip-cli-nodejs.md)
##### [Publiczny adres IP na poziomie wystąpienia](virtual-networks-instance-level-public-ip.md)

### Wdrożenie klasyczne
#### Listy kontroli dostępu
##### [Witryna Azure Portal](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Azure PowerShell](virtual-networks-acl-powershell.md)

## Zarządzanie
### [Sieci wirtualne](virtual-network-manage-network.md)
#### [Podsieci](virtual-network-manage-subnet.md)
#### [Komunikacja równorzędna](virtual-network-manage-peering.md)
#### Wdrożenie klasyczne
##### [Plik konfiguracji sieci](virtual-networks-using-network-configuration-file.md)
##### [Migrowanie z grupy koligacji do regionu](virtual-networks-migrate-to-regional-vnet.md)
### Grupy zabezpieczeń sieci
#### [Witryna Azure Portal](virtual-network-manage-nsg-arm-portal.md)
#### [Azure PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [Interfejs wiersza polecenia platformy Azure 2.0](virtual-network-manage-nsg-arm-cli.md)
#### [Interfejs wiersza polecenia platformy Azure 1.0](virtual-network-manage-nsg-cli-nodejs.md)

#### [Dzienniki](virtual-network-nsg-manage-log.md)
### Interfejsy sieciowe
#### [Tworzenie, zmienianie lub usuwanie interfejsów sieciowych](virtual-network-network-interface.md)
#### [Dodawanie, zmienianie lub usuwanie adresów IP](virtual-network-network-interface-addresses.md)
### Maszyny wirtualne
#### [Przenoszenie maszyny wirtualnej do innej podsieci](virtual-networks-move-vm-role-to-subnet.md)
### [Publiczne adresy IP](virtual-network-public-ip-address.md)
### Ochrona przed atakami DDOS
#### [Witryna Azure Portal](ddos-protection-manage-portal.md)
#### [Azure PowerShell](ddos-protection-manage-ps.md)

## Rozwiązywanie problemów
### Grupy zabezpieczeń sieci
#### [Witryna Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
#### [Azure PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Trasy
#### [Witryna Azure Portal](virtual-network-routes-troubleshoot-portal.md)
#### [Azure PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### [Testowanie przepływności](virtual-network-bandwidth-testing.md)
### [Cannot delete virtual networks (Nie można usunąć sieci wirtualnych)](virtual-network-troubleshoot-cannot-delete-vnet.md)
### [Problemy z łącznością między maszynami wirtualnymi](virtual-network-troubleshoot-connectivity-problem-between-vms.md)

# Dokumentacja
## [Przykłady kodu](https://azure.microsoft.com/en-us/resources/samples/?service=virtual-network)
## [Azure PowerShell (Resource Manager)](/powershell/module/azurerm.network)
## [Azure PowerShell (wersja klasyczna)](/powershell/module/azure/)
## [Interfejs wiersza polecenia platformy Azure](/cli/azure/network)
## [Java](/java/api/)
## [REST (Resource Manager)](https://msdn.microsoft.com/library/mt163658.aspx)
## [REST (klasyczny)](https://msdn.microsoft.com/library/jj157182.aspx)


# Powiązane
## [Virtual Machines](/azure/virtual-machines/)
## [Application Gateway](/azure/application-gateway/)
## [System DNS platformy Azure](/azure/dns/)
## [Traffic Manager](/azure/traffic-manager/)
## [Load Balancer](/azure/load-balancer/)
## [VPN Gateway](/azure/vpn-gateway/)
## [ExpressRoute](/azure/expressroute/)

# Zasoby
## [Harmonogram działania dla platformy Azure](https://azure.microsoft.com/roadmap/?category=networking)
## [Blog na temat sieci](http://azure.microsoft.com/blog/topics/networking)
## [Forum sieci](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Cennik](https://azure.microsoft.com/pricing/details/virtual-network)
## [Kalkulator cen](https://azure.microsoft.com/pricing/calculator/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
## [Dostawca zasobów sieciowych](resource-groups-networking.md)
