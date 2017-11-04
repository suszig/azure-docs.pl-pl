Maszyna wirtualna platformy Azure obsługuje dołączanie wielu dysków danych. W celu optymalizacji wydajności i uniknięcia potencjalnego ograniczania można ograniczyć liczbę intensywnie używanych dysków dołączonych do maszyny wirtualnej. Jeśli wszystkie dyski nie są intensywnie używane jednocześnie, konto magazynu może obsługiwać większą liczbę dysków.

* **Na dyskach platformy Azure zarządzanego:** limit liczby dysków zarządzane ma charakteru regionalnego i zależy od typu magazynu. Wartość domyślna, a także maksymalny limit jest 10 000 dla poszczególnych subskrypcji, na region i typ magazynu. Na przykład można utworzyć maksymalnie 10 000 standardowe zarządzane dysków i również 10 000 premium zarządzane dyski w subskrypcji i regionu. 

    Migawki zarządzane i obrazy są przeliczane względem limitu usługi Managed Disks.

* **Konto magazynu w warstwie Standardowa:** maksymalna całkowita liczba żądań dla konta magazynu w warstwie Standardowa to 20 000 operacji wejścia/wyjścia na sekundę (IOPS). Łączna liczba IOPS na wszystkich dyskach maszyny wirtualnej w koncie magazynu w warstwie Standardowa nie powinna przekroczyć tego limitu.
  
    Możesz orientacyjnie obliczyć liczbę intensywnie używanych dysków obsługiwanych przez jedno konto magazynu w warstwie Standardowa na podstawie limitu liczby żądań. Na przykład w przypadku maszyny wirtualnej w warstwie Podstawowa maksymalna liczba intensywnie używanych dysków to 66 (20 000/300 IOPS na dysk), a w przypadku maszyny wirtualnej w warstwie Standardowa — 40 (20 000/500 IOPS na dysk), jak pokazano w poniższej tabeli. 
* **Konta magazynu w warstwie Premium:** maksymalna całkowita przepływność konta magazynu w warstwie Premium to 50 Gb/s. Całkowita przepływność na wszystkich dyskach maszyny wirtualnej nie powinna przekroczyć tego limitu.

