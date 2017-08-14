
* Konwersja wymaga ponownego uruchomienia maszyny wirtualnej, więc należy zaplanować migrację maszyn wirtualnych podczas istniejącego okna obsługi. 

* Konwersja jest nieodwracalna. 

* Należy przetestować konwersję. Przeprowadź migrację testowej maszyny wirtualnej, zanim przeprowadzisz migrację w środowisku produkcyjnym.

* Podczas konwersji cofany jest przydział maszyny wirtualnej. Maszyna wirtualna otrzyma nowy adres IP przy jej ponownym uruchomieniu po konwersji. W razie potrzeby do maszyny wirtualnej można [przypisać statyczny adres IP](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md).

* Oryginalne wirtualne dyski twarde i konto magazynu używane przez maszynę wirtualną przed konwersją nie są usuwane. Nadal będą za nie naliczane opłaty. Aby uniknąć naliczania opłat za te artefakty, usuń oryginalne obiekty blob dysków VHD po upewnieniu się, że konwersja została zakończona.
