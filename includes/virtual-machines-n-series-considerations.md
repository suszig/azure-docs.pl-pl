## <a name="deployment-considerations"></a>Zagadnienia dotyczące wdrażania

* Dostępność N serii maszyn wirtualnych, zobacz [produkty, które są dostępne w regionie](https://azure.microsoft.com/en-us/regions/services/).

* N-series maszyny wirtualne mogą być wdrażane tylko w modelu wdrażania usługi Resource Manager.

* Podczas tworzenia maszyny Wirtualnej z N serii za pomocą portalu Azure na **podstawy** bloku, wybierz opcję **typu dysku maszyny Wirtualnej** z **HDD**. Aby wybrać dostępny rozmiar N serii na **rozmiar** bloku, kliknij przycisk **Wyświetl wszystkie**.

* NC i maszyn wirtualnych z wirtualizacją sieci nie obsługują dysków maszyny Wirtualnej, które bazują na magazynu Azure Premium.

* Jeśli chcesz wdrożyć więcej niż kilka N serii maszyn wirtualnych, należy wziąć pod uwagę z subskrypcji lub inne opcje zakupu. Jeśli używasz [bezpłatnego konta platformy Azure](https://azure.microsoft.com/free/), możesz użyć ograniczonej liczby rdzeni obliczeniowych platformy Azure.

* Konieczne może być Zwiększ limit przydziału rdzeni (na region) w Twojej subskrypcji platformy Azure i zwiększyć oddzielne limit przydziału rdzeni NC, NCv2, ND lub wirtualizacją sieci. Aby zażądać zwiększenia limitu przydziału [otwarcia żądania pomocy technicznej online klienta](../articles/azure-supportability/how-to-create-azure-support-request.md) bez dodatkowych opłat. Domyślne limity może się różnić w zależności od kategorii subskrypcji.

* Jeden obraz maszyny Wirtualnej, można wdrożyć na maszynach wirtualnych serii N jest [maszyny wirtualnej platformy Azure danych nauki](../articles/machine-learning/data-science-virtual-machine/overview.md). Maszyna wirtualna nauki danych preinstaluje i konfiguruje wielu popularnych danych nauki i narzędzie bezpośrednich. On również preinstaluje wersji sterowników procesora GPU tesla — NVIDIA.





