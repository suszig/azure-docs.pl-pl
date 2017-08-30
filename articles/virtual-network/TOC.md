# Omówienie
## [Sieci wirtualne](virtual-networks-overview.md)
## [Trasy zdefiniowane przez użytkownika i przesyłanie dalej IP](virtual-networks-udr-overview.md)
## [Wirtualne sieci równorzędne](virtual-network-peering-overview.md)
## [Ciągłość działalności biznesowej:](virtual-network-disaster-recovery-guidance.md)
## [Często zadawane pytania](virtual-networks-faq.md)
## [Adresowanie IP](virtual-network-ip-addresses-overview-arm.md)
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
#### [PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [Interfejs wiersza polecenia](virtual-networks-create-vnet-arm-cli.md)
#### [Szablon](virtual-networks-create-vnet-arm-template-click.md)

### Grupy zabezpieczeń sieci
#### [Portal](virtual-networks-create-nsg-arm-pportal.md)
#### [Program PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [Interfejs wiersza polecenia](virtual-networks-create-nsg-arm-cli.md)
#### [Szablon](virtual-networks-create-nsg-arm-template.md)
#### Wdrożenie klasyczne
##### [PowerShell](virtual-networks-create-nsg-classic-ps.md)
##### [Interfejs wiersza polecenia](virtual-networks-create-nsg-classic-cli.md)

### Trasy definiowane przez użytkownika
#### [Program PowerShell](virtual-network-create-udr-arm-ps.md)
#### [Interfejs wiersza polecenia](virtual-network-create-udr-arm-cli.md)
#### [Szablon](virtual-network-create-udr-arm-template.md)
#### Wdrożenie klasyczne
##### [PowerShell](virtual-network-create-udr-classic-ps.md)
##### [Interfejs wiersza polecenia](virtual-network-create-udr-classic-cli.md)

### Wirtualne sieci równorzędne
#### [Same deployment model - same subscription (Ten sam model wdrażania — ta sama subskrypcja)](virtual-network-create-peering.md)
#### [Same deployment model - different subscriptions (Ten sam model wdrażania — różne subskrypcje)](create-peering-different-subscriptions.md)
#### [Different deployment models - same subscription (Różne modele wdrażania — ta sama subskrypcja)](create-peering-different-deployment-models.md)
#### [Different deployment models - different subscriptions (Różne modele wdrażania — różne subskrypcje)](create-peering-different-deployment-models-subscriptions.md)

### Maszyny wirtualne
#### Tworzenie maszyny wirtualnej ze statycznym publicznym adresem IP
##### [Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [Program PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [Interfejs wiersza polecenia](virtual-network-deploy-static-pip-arm-cli.md)
##### [Szablon](virtual-network-deploy-static-pip-arm-template.md)
##### Wdrożenie klasyczne
###### [PowerShell](virtual-networks-reserved-public-ip.md)

#### Tworzenie maszyny wirtualnej ze statycznym prywatnym adresem IP
##### [Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [Program PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [Interfejs wiersza polecenia](virtual-networks-static-private-ip-arm-cli.md)
##### Wdrożenie klasyczne
###### [Portal](virtual-networks-static-private-ip-classic-pportal.md)
###### [Program PowerShell](virtual-networks-static-private-ip-classic-ps.md)
###### [Interfejs wiersza polecenia](virtual-networks-static-private-ip-classic-cli.md)

#### Tworzenie maszyny wirtualnej z wieloma interfejsami sieciowymi
##### [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Interfejs wiersza polecenia](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### Wdrożenie klasyczne
###### [PowerShell](virtual-network-deploy-multinic-classic-ps.md)
###### [Interfejs wiersza polecenia](virtual-network-deploy-multinic-classic-cli.md)

#### Tworzenie maszyny wirtualnej z wieloma adresami IP
##### [Witryna Azure Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [Interfejs wiersza polecenia](virtual-network-multiple-ip-addresses-cli.md)
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
##### [Prosta strefa DMZ z sieciowymi grupami zabezpieczeń](virtual-networks-dmz-nsg-asm.md)
##### [Strefa DMZ z zaporą i sieciowymi grupami zabezpieczeń](virtual-networks-dmz-nsg-fw-asm.md)
##### [Strefa DMZ z zaporą, routingiem zdefiniowanym przez użytkownika i sieciowymi grupami zabezpieczeń](virtual-networks-dmz-nsg-fw-udr-asm.md)
##### [Przykładowa aplikacja](virtual-networks-sample-app.md)

### Wdrożenie klasyczne
#### [Sieć wirtualna](create-virtual-network-classic.md)
##### [Portal](virtual-networks-create-vnet-classic-pportal.md)
##### [Program PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md)
##### [Interfejs wiersza polecenia](virtual-networks-create-vnet-classic-cli.md)

## Konfigurowanie
### Maszyny wirtualne
#### [Dodawanie lub usuwanie interfejsów sieciowych](virtual-network-network-interface-vm.md)
#### [Rozpoznawanie nazw dla maszyn wirtualnych i usług w chmurze](virtual-networks-name-resolution-for-vms-and-role-instances.md)
#### [Optymalizowanie przepływności sieci](virtual-network-optimize-network-bandwidth.md)
#### [Wyświetlanie i modyfikowanie nazw hostów](virtual-networks-viewing-and-modifying-hostnames.md)
### Wdrożenie klasyczne
#### Listy kontroli dostępu
##### [Portal](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [PowerShell](virtual-networks-acl-powershell.md)

## Zarządzaj
### [Sieci wirtualne](virtual-network-manage-network.md)
#### [Podsieci](virtual-network-manage-subnet.md)
#### [Komunikacja równorzędna](virtual-network-manage-peering.md)
#### Wdrożenie klasyczne
##### [Plik konfiguracji sieci](virtual-networks-using-network-configuration-file.md)
##### [Migrowanie z grupy koligacji do regionu](virtual-networks-migrate-to-regional-vnet.md)
### Grupy zabezpieczeń sieci
#### [Portal](virtual-network-manage-nsg-arm-portal.md)
#### [Program PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [Interfejs wiersza polecenia](virtual-network-manage-nsg-arm-cli.md)
#### [Dzienniki](virtual-network-nsg-manage-log.md)
### Interfejsy sieciowe
#### [Tworzenie, zmienianie lub usuwanie interfejsów sieciowych](virtual-network-network-interface.md)
#### [Dodawanie, zmienianie lub usuwanie adresów IP](virtual-network-network-interface-addresses.md)
### Maszyny wirtualne
#### [Przenoszenie maszyny wirtualnej do innej podsieci](virtual-networks-move-vm-role-to-subnet.md)
### [Publiczne adresy IP](virtual-network-public-ip-address.md)

## Rozwiązywanie problemów
### Grupy zabezpieczeń sieci
#### [Portal](virtual-network-nsg-troubleshoot-portal.md)
#### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Trasy
#### [Portal](virtual-network-routes-troubleshoot-portal.md)
#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### [Testowanie przepływności](virtual-network-bandwidth-testing.md)
### [Cannot delete virtual networks (Nie można usunąć sieci wirtualnych)](virtual-network-troubleshoot-cannot-delete-vnet.md)
### [Problemy z łącznością między maszynami wirtualnymi](virtual-network-troubleshoot-connectivity-problem-between-vms.md)

# Dokumentacja
## [Przykłady kodu](https://azure.microsoft.com/en-us/resources/samples/?service=virtual-network)
## [PowerShell (Resource Manager)](/powershell/module/azurerm.network)
## [PowerShell (klasyczny)](/powershell/module/azure/)
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
