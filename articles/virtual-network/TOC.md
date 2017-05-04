# Omówienie
## [Sieci wirtualne](virtual-networks-overview.md)
## [Trasy zdefiniowane przez użytkownika i przesyłanie dalej IP](virtual-networks-udr-overview.md)
## [Wirtualne sieci równorzędne](virtual-network-peering-overview.md)
## [Ciągłość działalności biznesowej:](virtual-network-disaster-recovery-guidance.md)
## [Często zadawane pytania](virtual-networks-faq.md)
## Adresowanie IP
### [Resource Manager](virtual-network-ip-addresses-overview-arm.md)
### [Wdrożenie klasyczne](virtual-network-ip-addresses-overview-classic.md)

# Wprowadzenie
## [Tworzenie pierwszej sieci wirtualnej](virtual-network-get-started-vnet-subnet.md)

# Instrukcje
## Planowanie i projektowanie
### [Sieci wirtualne](virtual-network-vnet-plan-design-arm.md)
### [Sieciowe grupy zabezpieczeń](virtual-networks-nsg.md)

## Wdrożenie
### Sieci wirtualne
#### [Portal](virtual-networks-create-vnet-arm-pportal.md)
#### [Program PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [Interfejs wiersza polecenia](virtual-networks-create-vnet-arm-cli.md)
#### [Szablon](virtual-networks-create-vnet-arm-template-click.md)
#### [Portal (klasyczny)](virtual-networks-create-vnet-classic-pportal.md)
#### [PowerShell (klasyczny)](virtual-networks-create-vnet-classic-netcfg-ps.md)
#### [Interfejs wiersza polecenia (klasyczny)](virtual-networks-create-vnet-classic-cli.md)

### Grupy zabezpieczeń sieci
#### [Portal](virtual-networks-create-nsg-arm-pportal.md)
#### [Program PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [Interfejs wiersza polecenia](virtual-networks-create-nsg-arm-cli.md)
#### [Szablon](virtual-networks-create-nsg-arm-template.md)
#### [PowerShell (klasyczny)](virtual-networks-create-nsg-classic-ps.md)
#### [Interfejs wiersza polecenia (klasyczny)](virtual-networks-create-nsg-classic-cli.md)

### Trasy definiowane przez użytkownika
#### [Program PowerShell](virtual-network-create-udr-arm-ps.md)
#### [Interfejs wiersza polecenia](virtual-network-create-udr-arm-cli.md)
#### [Szablon](virtual-network-create-udr-arm-template.md)
#### [PowerShell (klasyczny)](virtual-network-create-udr-classic-ps.md)
#### [Interfejs wiersza polecenia (klasyczny)](virtual-network-create-udr-classic-cli.md)

### Wirtualne sieci równorzędne
#### [Portal](virtual-networks-create-vnetpeering-arm-portal.md)
#### [Program PowerShell](virtual-networks-create-vnetpeering-arm-ps.md)
#### [Szablon](virtual-networks-create-vnetpeering-arm-template-click.md)

### [Interfejsy sieciowe](virtual-network-network-interface.md)

### [Publiczne adresy IP](virtual-network-public-ip-address.md)

### Maszyny wirtualne

#### Statyczne publiczne adresy IP
##### [Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [Program PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [Interfejs wiersza polecenia](virtual-network-deploy-static-pip-arm-cli.md)
##### [Szablon](virtual-network-deploy-static-pip-arm-template.md)
##### [PowerShell (klasyczny)](virtual-networks-reserved-public-ip.md)

#### Statyczne prywatne adresy IP
##### [Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [Program PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [Interfejs wiersza polecenia](virtual-networks-static-private-ip-arm-cli.md)
##### [Portal (klasyczny)](virtual-networks-static-private-ip-classic-pportal.md)
##### [PowerShell (klasyczny)](virtual-networks-static-private-ip-classic-ps.md)
##### [Interfejs wiersza polecenia (klasyczny)](virtual-networks-static-private-ip-classic-cli.md)

#### Wiele interfejsów sieciowych
##### [Program PowerShell](virtual-network-deploy-multinic-arm-ps.md)
##### [Interfejs wiersza polecenia](virtual-network-deploy-multinic-arm-cli.md)
##### [Szablon](virtual-network-deploy-multinic-arm-template.md)
##### [PowerShell (klasyczny)](virtual-network-deploy-multinic-classic-ps.md)
##### [Interfejs wiersza polecenia (klasyczny)](virtual-network-deploy-multinic-classic-cli.md)

#### Wiele adresów IP
##### [Azure Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [Interfejs wiersza polecenia](virtual-network-multiple-ip-addresses-cli.md)
##### [Szablon](virtual-network-multiple-ip-addresses-template.md)

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

## Konfigurowanie
### Wydajniejsze sieci dla maszyn wirtualnych
#### [Azure Portal](virtual-network-accelerated-networking-portal.md)
#### [Program PowerShell](virtual-network-accelerated-networking-powershell.md)
### [Optymalizowanie przepływności sieci maszyny wirtualnej](virtual-network-optimize-network-bandwidth.md)
### Listy kontroli dostępu
#### [Portal klasyczny](virtual-networks-acl.md)
#### [PowerShell](virtual-networks-acl-powershell.md)
### [Name resolution for VMs and cloud services](virtual-networks-name-resolution-for-vms-and-role-instances.md) (Rozpoznawanie nazw dla maszyn wirtualnych i usług w chmurze)

## Zarządzanie
### Grupy zabezpieczeń sieci
#### [Portal](virtual-network-manage-nsg-arm-portal.md)
#### [Program PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [Interfejs wiersza polecenia](virtual-network-manage-nsg-arm-cli.md)
#### [Dzienniki](virtual-network-nsg-manage-log.md)
### Maszyny wirtualne
#### [Wyświetlanie i modyfikowanie nazw hostów](virtual-networks-viewing-and-modifying-hostnames.md)
#### [Przenoszenie maszyny wirtualnej do innej podsieci](virtual-networks-move-vm-role-to-subnet.md)

## Rozwiązywanie problemów
### Grupy zabezpieczeń sieci
#### [Portal](virtual-network-nsg-troubleshoot-portal.md)
#### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Trasy
#### [Portal](virtual-network-routes-troubleshoot-portal.md)
#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### [Testowanie przepływności](virtual-network-bandwidth-testing.md)

# Dokumentacja
## [PowerShell (Resource Manager)](/powershell/module/azurerm.network)
## [PowerShell (klasyczny)](/powershell/module/azure/?view=azuresmps-3.7.0)
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
## [Blog na temat sieci](http://azure.microsoft.com/blog/topics/networking)
## [Forum sieci](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Cennik](https://azure.microsoft.com/pricing/details/virtual-network)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
