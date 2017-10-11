Usługa Azure Load Balancer to moduł równoważenia obciążenia w warstwie 4 (TCP, UDP). Moduł równoważenia obciążenia zapewnia wysoką dostępność, dystrybuując ruch przychodzący w wystąpieniach usług o dobrej kondycji w usługach w chmurze lub na maszynach wirtualnych w zestawie modułu równoważenia obciążenia. Usługa Azure Load Balancer może także prezentować te usługi na wielu portach i/lub wielu adresach IP.

Moduł równoważenia obciążenia można skonfigurować do następujących celów:

* Równoważenie obciążenia przychodzącego ruchu internetowego na maszynach wirtualnych (VM). W tym scenariuszu moduł równoważenia obciążenia oznacza [dostępny z Internetu moduł równoważenia obciążenia](../articles/load-balancer/load-balancer-internet-overview.md).
* Równoważenie obciążenia między maszynami wirtualnymi w sieci wirtualnej (VNet), między maszynami wirtualnymi w usługach w chmurze lub między lokalnymi komputerami i maszynami wirtualnymi w sieci wirtualnej obejmującej wiele lokalizacji. W tym scenariuszu moduł równoważenia obciążenia oznacza [wewnętrzny moduł równoważenia obciążenia (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Przekazywanie ruchu zewnętrznego do konkretnego wystąpienia maszyny wirtualnej.
