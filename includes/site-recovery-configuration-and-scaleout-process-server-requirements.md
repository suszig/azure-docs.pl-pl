| **Sprzęt** | |
| --- |---|
| Liczba rdzeni procesora CPU| 8 |
| Pamięć RAM| 12 GB|
| Liczba dysków | 3 <br><br> - Dysk systemu operacyjnego<br> - Dysk pamięci podręcznej serwera przetwarzania<br> - Dysk przechowywania (na potrzeby powrotu po awarii)|
| Wolne miejsce na dysku (pamięć podręczna serwera przetwarzania) | 600 GB
| Wolne miejsce na dysku (dysk przechowywania) | 600 GB|
| **Oprogramowanie** | |
| Wersja systemu operacyjnego | Windows Server 2012 R2 |
| Ustawienia regionalne systemu operacyjnego | Angielski (en-us)|
| Wersja programu VMware vSphere PowerCLI | [PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0 "PowerCLI 6.0")|
| Role systemu Windows Server | Nie należy włączać następujących ról: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V |
| **Sieć** | |
| Typ karty interfejsu sieciowego | VMXNET3 |
| Typ adresu IP | Statyczny |
| Dostęp do Internetu | Serwer powinien być w stanie uzyskać dostęp do następujących adresów URL, bezpośrednio lub za pośrednictwem serwera proxy: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (nie jest to wymagane w przypadku serwerów przetwarzania używanych do skalowania w poziomie) <br> - time.nist.gov <br> - time.windows.com |
| Porty | 443 (organizowanie kanału sterowania)<br>9443 (transport danych)|


<!--HONumber=Feb17_HO3-->


