


## Korzyści z integracji zasobów obliczeniowych, sieci i usługi Storage w ramach modelu wdrażania przy użyciu usługi Azure Resource Manager

Model wdrażania przy użyciu usługi Azure Resource Manager umożliwia łatwe korzystanie z szablonów wstępnie zbudowanych aplikacji oraz tworzenie szablonów aplikacji w celu wdrażania zasobów obliczeniowych, sieci i usługi Storage na platformie Azure i zarządzania nimi. W tej części omówimy korzyści związane z wdrażaniem zasobów z wykorzystaniem modelu wdrażania przy użyciu usługi Azure Resource Manager.

-   Uproszczenie złożoności — tworzenie i integracja złożonych aplikacji, które mogą obejmować całą gamę zasobów platformy Azure (np. witryny sieci Web, bazy danych SQL, maszyny wirtualne lub sieci wirtualne), oraz współpraca w ramach tych aplikacji z poziomu pliku szablonu z możliwością udostępniania
-   Elastyczność polegająca na możliwości stosowania powtarzalnych wdrożeń w zakresie programowania, metodyki DevOps i działań administratorów systemu w przypadku korzystania z tego samego pliku szablonu
-   Głęboka integracja rozszerzeń maszyny wirtualnej (skrypty niestandardowe, DSC, Chef, Puppet itp.) z usługą Azure Resource Manager w pliku szablonu umożliwia łatwą aranżację konfiguracji ustawień w maszynie wirtualnej
-   Definiowanie tagów zasobów obliczeniowych, sieci i usługi Storage oraz propagacja rozliczeń tych tagów
-   Proste i precyzyjne zarządzanie dostępem do zasobów organizacji w ramach kontroli dostępu na podstawie ról na platformie Azure (RBAC)
-   Uproszczone uaktualnianie/aktualizowanie sekcji przez modyfikowanie oryginalnego szablonu i jego ponowne wdrożenie


## Korzyści z interfejsów API zasobów obliczeniowych, sieci i usługi Storage w usłudze Azure Resource Manager

Poza korzyściami wymienionymi powyżej nowo udostępnione interfejsy API zapewniają także istotne usprawnienia wydajności:

-   Umożliwienie masowych i równoległych wdrożeń maszyn wirtualnych
-   Obsługa 3 domen błędów w zestawach dostępności
-   Ulepszone rozszerzenie skryptu niestandardowego umożliwiające określanie skryptów z dowolnego publicznie dostępnego niestandardowego adresu URL
- Integracja maszyn wirtualnych z usługą Azure Key Vault zapewniająca niezwykle bezpieczne przechowywanie i prywatne wdrażanie kluczy tajnych ze [zgodnych ze standardem FIPS](http://wikipedia.org/wiki/FIPS_140-2)[sprzętowych modułów zabezpieczeń](http://wikipedia.org/wiki/Hardware_security_module)
-   Interfejsy API stanowiące podstawowe bloki konstrukcyjne obsługi sieci, które umożliwiają klientom tworzenie złożonych aplikacji obejmujących interfejsy sieciowe, moduły równoważenia obciążenia oraz sieci wirtualne
-   Interfejsy sieciowe jako nowy obiekt umożliwiający stosowanie skomplikowanej konfiguracji sieci i jej ponowne użycie w odniesieniu do maszyn wirtualnych
-   Moduły równoważenia obciążenia jako najwyższej jakości zasób umożliwiający przypisywanie adresów IP
-   Szczegółowe interfejsy API sieci wirtualnej pozwalające na uproszczenie zarządzania poszczególnymi sieciami wirtualnymi

## Różnice koncepcyjne związane z wprowadzeniem nowych interfejsów API

W tej sekcji opisano niektóre z najważniejszych różnic koncepcyjnych między dostępnymi obecnie interfejsami API opartymi na języku XML oraz opartymi na formacie JSON interfejsami API dla zasobów obliczeniowych, sieci i usługi Storage, które są dostępne za pośrednictwem usługi Azure Resource Manager.

 Element | Usługa Azure Service Management (oparta na języku XML)    | Dostawcy zasobów obliczeniowych, sieci i usługi Storage (opartych na formacie JSON)
 ---|---|---
| Usługa w chmurze dla maszyn wirtualnych |  Usługa w chmurze stanowiła kontener do przechowywania maszyn wirtualnych wymagających dostępności na platformie oraz równoważenia obciążenia. | Usługa w chmurze nie jest już obiektem wymaganym do utworzenia maszyny wirtualnej przy użyciu nowego modelu. |
| Zestawy dostępności | Dostępność dla platformy była wskazywana przez skonfigurowanie na maszynach wirtualnych takiego samego parametru „AvailabilitySetName”. Maksymalna liczba domen błędów wynosiła 2. | Zestaw dostępności to zasób udostępniany przez dostawcę Microsoft.Compute. Maszyny wirtualne wymagające wysokiej dostępności muszą należeć do zestawu dostępności. Maksymalna liczba domen błędów wynosi obecnie 3. |
| Grupy koligacji | Grupy koligacji były wymagane do tworzenia sieci wirtualnych. Przestały być wymagane wraz z wprowadzeniem regionalnych sieci wirtualnych. |Dla uproszczenia pojęcie grup koligacji nie jest stosowane w interfejsach API udostępnianych za pośrednictwem usługi Azure Resource Manager. |
| Równoważenie obciążenia    | Utworzenie usługi w chmurze zapewnia niejawny moduł równoważenie obciążenia dla wdrożonych maszyn wirtualnych. | Usługa Load Balancer stanowi zasób udostępniany przez dostawcę Microsoft.Network. Podstawowy interfejs sieciowy maszyn wirtualnych wymagający równoważenia obciążenia powinien odwoływać się do modułu równoważenia obciążenia. Moduły równoważenia obciążenia mogą być wewnętrzne lub zewnętrzne. [Dowiedz się więcej.](../articles/resource-groups-networking.md) |
|Wirtualny adres IP | Po dodaniu maszyny wirtualnej do usługi w chmurze usługom w chmurze zostaje przypisany domyślny adres VIP (wirtualny adres IP). Wirtualny adres IP to adres skojarzony z niejawnym modułem równoważenia obciążenia.   | Publiczny adres IP stanowi zasób udostępniany przez dostawcę Microsoft.Network. Publiczny adres IP może być statyczny (zastrzeżony) lub dynamiczny. Dynamiczne publiczne adresy IP można przypisać do usługi Load Balancer. Publiczne adresy IP mogą być chronione przy użyciu grup zabezpieczeń. |
|Zastrzeżony adres IP|   Istnieje możliwość zastrzeżenia adresu IP na platformie Azure i skojarzenia go z usługą w chmurze w celu zapewnienia jego umocowania.   | Publiczny adres IP, który można utworzyć w trybie statycznym, oferuje te same możliwości, co zastrzeżony adres IP. Obecnie statyczne publiczne adresy IP można przypisać tylko do modułu równoważenia obciążenia. |
|Publiczny adres IP (PIP) dla maszyny wirtualnej | Publiczne adresy IP można również skojarzyć bezpośrednio z maszyną wirtualną. | Publiczny adres IP stanowi zasób udostępniany przez dostawcę Microsoft.Network. Publiczny adres IP może być statyczny (zastrzeżony) lub dynamiczny. Obecnie w celu uzyskania publicznego adresu IP dla maszyny wirtualnej do interfejsu sieciowego można jednak przypisywać tylko dynamiczne publiczne adresy IP. |
|Punkty końcowe| Wejściowe punkty końcowe wymagały skonfigurowania na maszynie wirtualnej w celu uzyskania otwartej łączności z określonymi portami. Jednym z często używanych trybów łączenia się z maszynami wirtualnymi jest ustawienie wejściowych punktów końcowych. | Można skonfigurować reguły ruchu przychodzącego translatora adresów sieciowych w usługach Load Balancer w celu uzyskania tej samej możliwości włączania punktów końcowych dla określonych portów na potrzeby połączeń z maszynami wirtualnymi. |
|Nazwa DNS| Usługa w chmurze otrzymuje niejawną, globalnie unikatową nazwę DNS. Na przykład: `mycoffeeshop.cloudapp.net`. | Nazwy DNS są opcjonalnymi parametrami, które można określić w zasobie publicznego adresu IP. W pełni kwalifikowana nazwa domeny (FQDN) będzie mieć następujący format: `<domainlabel>.<region>.cloudapp.azure.com`. |
|Interfejsy sieciowe | Podstawowy i pomocniczy interfejs sieciowy oraz ich właściwości zostały zdefiniowane jako konfiguracja sieci maszyny wirtualnej. | Interfejs sieciowy stanowi zasób udostępniany przez dostawcę Microsoft.Network. Cykl życia interfejsu sieciowego nie jest powiązany z maszyną wirtualną. |

## Wprowadzenie do szablonów platformy Azure dla maszyn wirtualnych

Pracę z szablonami platformy Azure można rozpocząć, korzystając z różnych oferowanych przez nas narzędzi do projektowania i wdrażania w ramach platformy.

### Azure Portal

W portalu Azure nadal dostępna będzie opcja wdrażania maszyn wirtualnych przy jednoczesnym użyciu klasycznego modelu wdrażania oraz modelu wdrażania maszyn wirtualnych z użyciem usługi Resource Manager. Portal Azure będzie również umożliwiać wdrażanie szablonów niestandardowych.

### Azure PowerShell

Program Azure PowerShell będzie obsługiwać dwa tryby wdrażania — **AzureServiceManagement** i **AzureResourceManager**.  Tryb AzureResourceManager zawiera teraz również polecenia cmdlet do zarządzania maszynami wirtualnymi, sieciami wirtualnymi oraz kontami usługi Storage. Więcej informacji na ten temat można znaleźć [tutaj](../articles/powershell-azure-resource-manager.md).

### Interfejs wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure (Azure CLI) oferuje dwa tryby wdrażania — **AzureServiceManagement** i **AzureResourceManager**. Tryb AzureResourceManager obejmuje teraz także polecenia do zarządzania maszynami wirtualnymi, sieciami wirtualnymi i kontami usługi Storage. Więcej informacji na ten temat można znaleźć [tutaj](../articles/xplat-cli-azure-resource-manager.md).

### Visual Studio

Najnowsza wersja zestawu SDK platformy Azure dla programu Visual Studio pozwala tworzyć i wdrażać maszyny wirtualne oraz złożone aplikacje bezpośrednio w programie Visual Studio. Program Visual Studio oferuje możliwość wdrażania przy użyciu wstępnie utworzonej listy szablonów oraz rozpoczynania pracy z użyciem pustego szablonu.

### Interfejsy API REST

Szczegółową dokumentację interfejsu API REST dla dostawców zasobów obliczeniowych, sieci i usługi Storage można znaleźć [tutaj](https://msdn.microsoft.com/library/azure/dn790568.aspx).

## Często zadawane pytania

**Czy można utworzyć maszynę wirtualną przy użyciu nowej usługi Azure Resource Manager w celu wdrożenia w sieci wirtualnej lub na koncie usługi Storage, które zostały utworzone z użyciem interfejsów API usługi Azure Service Management?**

Takie rozwiązanie nie jest obecnie obsługiwane. Nie można zrealizować wdrożenia, korzystając z nowych interfejsów API usługi Azure Resource Manager w celu wdrożenia maszyny wirtualnej w sieci wirtualnej utworzonej przy użyciu interfejsów API usługi Service Management.

**Czy można utworzyć maszynę wirtualną, korzystając z nowych interfejsów API usługi Azure Resource Manager z obrazu użytkownika, który został utworzony z użyciem interfejsów API usługi Azure Service Management?**

Takie rozwiązanie nie jest obecnie obsługiwane. Można jednak skopiować pliki VHD z konta usługi Storage utworzonego przy użyciu interfejsów API usługi Service Management oraz skopiować je do nowego konta, które zostało utworzone z użyciem nowych interfejsów API usługi Azure Resource Manager.

**Jak wpłynie to na przydziały obowiązujące dla mojej subskrypcji?**

Przydziały dla maszyn wirtualnych, sieci wirtualnych i kont usługi Storage utworzonych przy użyciu nowych interfejsów API usługi Azure Resource Manager obowiązują niezależnie od bieżących przydziałów. Każda subskrypcja otrzymuje nowe przydziały na tworzenie zasobów przy użyciu nowych interfejsów API. Więcej informacji o dodatkowych przydziałach można znaleźć [tutaj](../articles/azure-subscription-service-limits.md).

**Czy mogę nadal używać moich zautomatyzowanych skryptów do inicjowania obsługi maszyn wirtualnych, sieci wirtualnych, kont usługi Storage itp. przy użyciu nowych interfejsów API usługi Azure Resource Manager?**

Wszelkie utworzone elementy do automatyzacji oraz skrypty będą nadal działać w przypadku istniejących maszyn wirtualnych i sieci wirtualnych utworzonych w trybie Azure Service Management. Skrypty należy jednak zaktualizować w celu zastosowania nowego schematu tworzenia tych samych zasobów w nowym trybie Azure Resource Manager.

**Gdzie znajdę przykłady szablonów usługi Azure Resource Manager?**

Kompleksowy zestaw szablonów startowych można znaleźć na stronie [Azure Resource Manager QuickStart Templates](https://azure.microsoft.com/documentation/templates/) (Szablony szybkiego startu usługi Azure Resource Manager).



<!--HONumber=sep16_HO1-->


