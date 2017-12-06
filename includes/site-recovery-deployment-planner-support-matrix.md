# <a name="support-matrix"></a>Macierz obsługi

| | **Z programu VMware do platformy Azure** |**Z funkcji Hyper-V do platformy Azure**|**Z platformy Azure do platformy Azure**|**Funkcji Hyper-V do lokacji dodatkowej**|**VMware do lokacji dodatkowej**
--|--|--|--|--|--
Obsługiwane scenariusze |Tak|Tak|Nie|Tak*|Nie
Obsługiwana wersja | vCenter 6.5, 6.0 lub 5.5| Windows Server 2016, w systemie Windows Server 2012 R2 | Nie dotyczy |Windows Server 2016, w systemie Windows Server 2012 R2|Nie dotyczy
Obsługiwana konfiguracja|vCenter, ESXi| Klaster funkcji Hyper-V, host funkcji Hyper-V|Nie dotyczy|Klaster funkcji Hyper-V, host funkcji Hyper-V|Nie dotyczy|
Liczba serwerów, które mogą być profilowane na uruchomione wystąpienie planowania wdrożenia odzyskiwania lokacji Azure |Jednego (maszyn wirtualnych należących do jednego serwera vCenter lub jeden serwer ESXi można sprofilować jednocześnie)|Wiele (maszyny wirtualne wielu hostów lub klastrów hostów może być profilu w czasie)| Nie dotyczy |Wiele (maszyny wirtualne wielu hostów lub klastrów hostów może być profilu w czasie)| Nie dotyczy

* Narzędzie jest przeznaczone głównie dla funkcji Hyper-V do scenariusza odzyskiwania po awarii systemu Azure. Dla funkcji Hyper-V do odzyskiwania po awarii lokacji dodatkowej może służyć tylko zrozumienie źródła po stronie zalecenia, takich jak wymagane przepustowości, miejsca wymaganego wolnego miejsca na każdym z serwerów źródłowych funkcji Hyper-V i replikacji początkowej, przetwarzanie wsadowe liczb i partii definicje.  Ignoruj Azure zalecenia i kosztów z raportu. Ponadto operacji Get przepływności nie ma zastosowania dla funkcji Hyper-V do scenariusza odzyskiwania po awarii lokacji dodatkowej.
