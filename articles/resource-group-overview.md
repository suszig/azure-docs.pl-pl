<properties
   pageTitle="Omówienie usługi Azure Resource Manager | Microsoft Azure"
   description="Opis wdrażania zasobów na platformie Azure, kontrolowania dostępu do tych zasobów oraz zarządzania nimi za pomocą usługi Azure Resource Manager."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/27/2016"
   ms.author="tomfitz"/>

# Omówienie usługi Azure Resource Manager

Infrastruktura aplikacji zwykle obejmuje wiele składników — może to być maszyna wirtualna, konto magazynu i sieć wirtualna albo aplikacja sieci Web, baza danych, serwer bazy danych i usługi zewnętrzne. Te składniki nie są widoczne jako osobne jednostki, tylko jako powiązane i zależne od siebie nawzajem części jednej całości. Dlatego najlepiej wdrażać i monitorować je oraz zarządzać nimi grupowo. Usługa Azure Resource Manager umożliwia pracę z zasobami tworzącymi rozwiązanie w formie grupy. Wszystkie zasoby danego rozwiązania można wdrożyć, zaktualizować lub usunąć w ramach jednej skoordynowanej operacji. Wdrażanie wykonuje się przy użyciu szablonu, którego można następnie używać w różnych środowiskach (testowanie, etap przejściowy i produkcja). Usługa Resource Manager zapewnia funkcje zabezpieczeń, inspekcji i tagowania ułatwiające zarządzanie zasobami po wdrożeniu. 

## Zalety korzystania z usługi Resource Manager

Usługa Resource Manager zapewnia kilka korzyści:

- Możliwość grupowego wdrożenia i monitorowania wszystkich zasobów w ramach rozwiązania oraz zarządzania nimi (zamiast obsługiwania zasobów pojedynczo).
- Możliwość wielokrotnego wdrażania rozwiązania w całym cyklu programistycznym z gwarancją spójnego stanu zasobów po każdym wdrożeniu.
- Możliwość definiowania wdrożenia za pomocą szablonów deklaratywnych.
- Możliwość definiowania zależności między zasobami, aby wdrażać je w odpowiedniej kolejności.
- Możliwość stosowania kontroli dostępu do wszystkich usług w grupie zasobów dzięki natywnej integracji funkcji kontroli dostępu na podstawie ról z platformą zarządzania.
- Możliwość dodawania tagów do zasobów w celu logicznego uporządkowania wszystkich zasobów w ramach subskrypcji.
- Możliwość uproszczenia rozliczeń w organizacji przez wyświetlanie zestawień kosztów całej grupy lub tylko kosztów zasobów oznaczonych tym samym tagiem.  

Usługa Resource Manager udostępnia nową metodę wdrażania rozwiązań i zarządzania nimi. Jeśli znasz wcześniejszy model wdrażania i chcesz dowiedzieć się więcej o zmianach, zobacz artykuł [Understanding Resource Manager deployment and classic deployment](resource-manager-deployment-model.md) (Opis wdrażania za pomocą usługi Resource Manager oraz wdrażania klasycznego).

## Wskazówki

Poniższe sugestie pomogą Ci w pełni wykorzystać możliwości usługi Resource Manager w pracy z rozwiązaniami.

1. Definiuj i wdrażaj infrastrukturę za pomocą składni deklaratywnej w szablonach usługi Resource Manager, a nie za pomocą poleceń imperatywnych.
2. Zdefiniuj w szablonie wszystkie etapy wdrażania i konfiguracji. W konfiguracji rozwiązania nie powinno być żadnych etapów ręcznych.
3. Korzystaj z poleceń imperatywnych do zarządzania zasobami, np. do uruchamiania i zatrzymywania aplikacji lub maszyny.
4. Rozmieść zasoby z tym samym cyklem życia w grupie zasobów. We wszystkich pozostałych operacjach związanych z organizacją zasobów używaj tagów.

## Grupy zasobów

Grupa zasobów to kontener, który zawiera powiązane zasoby dla aplikacji. Może ona obejmować wszystkie zasoby danej aplikacji lub tylko te, które są logicznie pogrupowane. Zasoby mogą być przydzielane do grup na podstawie priorytetów organizacji.

Definiując grupę zasobów, należy wziąć pod uwagę pewne ważne czynniki:

1. Wszystkie zasoby w grupie powinny mieć taki sam cykl życia. Będą one wdrażane, aktualizowane i usuwane razem. Jeśli jakiś zasób, na przykład serwer bazy danych, ma mieć inny cykl wdrażania, powinien zostać umieszczony w innej grupie zasobów.
2. Każdy zasób może znajdować się tylko w jednej grupie zasobów.
3. Zasoby w grupie można dodawać i usuwać w dowolnym momencie.
4. Zasoby można przenosić między poszczególnymi grupami. Aby uzyskać więcej informacji, zobacz [Move resources to new resource group or subscription](resource-group-move-resources.md) (Przenoszenie zasobów do nowej grupy lub subskrypcji).
4. Grupa zasobów może zawierać zasoby, które znajdują się w różnych regionach.
5. Grupa zasobów może służyć do określania zakresu kontroli dostępu na potrzeby działań administracyjnych.
6. Zasób może być połączony z zasobem w innej grupie zasobów, gdy oba zasoby muszą ze sobą współdziałać, ale nie mają tego samego cyklu życia (np. w przypadku wielu aplikacji łączących się z bazą danych). Aby uzyskać więcej informacji, zobacz [Linking resources in Azure Resource Manager](resource-group-link-resources.md) (Łączenie zasobów w usłudze Azure Resource Manager).

## Dostawcy zasobów

Dostawca zasobów to usługa dostarczająca zasoby, które można wdrażać i którymi można zarządzać za pomocą usługi Resource Manager. Każdy dostawca zasobów udostępnia operacje interfejsu API REST do pracy z zasobami. Na przykład w celu wdrożenia Magazynu kluczy Azure na potrzeby przechowywania kluczy i kluczy tajnych należy użyć dostawcy zasobów **Microsoft.KeyVault**. Ten dostawca zasobów udostępnia typ zasobu o nazwie **magazyny** do utworzenia magazynu kluczy oraz typ **magazyny/klucze tajne** do utworzenia klucza tajnego w magazynie kluczy. Aby dowiedzieć się więcej o danym dostawcy zasobów, możesz zapoznać się z jego opisem w operacjach interfejsu API REST, np.: [Key Vault REST API operations](https://msdn.microsoft.com/library/azure/dn903609.aspx) (Operacje interfejsu API REST Magazynu kluczy).

Aby móc wdrażać infrastrukturę i zarządzać nią, musisz poznać szczegóły dostawców zasobów, na przykład udostępniane przez nich typy zasobów, numery wersji operacji interfejsu API REST, obsługiwane operacje oraz schemat używany podczas ustawiania wartości typu zasobu do utworzenia. Aby dowiedzieć się więcej na temat obsługiwanych dostawców zasobów, zobacz [Resource Manager providers, regions, API versions and schemas](resource-manager-supported-services.md) (Dostawcy, regiony, wersje interfejsów API oraz schematy usługi Resource Manager).

## Wdrażanie na podstawie szablonu

Usługa Resource Manager umożliwia utworzenie prostego szablonu (w formacie JSON) do definiowania wdrażania i konfiguracji aplikacji. Jest on nazywany szablonem usługi Resource Manager i pozwala na deklaratywne definiowanie wdrożeń. Dzięki szablonowi można wielokrotnie wdrażać aplikację w całym jej cyklu życia z gwarancją spójnego stanu zasobów po każdym wdrożeniu.

W szablonie należy zdefiniować infrastrukturę aplikacji, sposób konfigurowania infrastruktury oraz sposób publikowania kodu aplikacji w tej infrastrukturze. Nie trzeba się martwić o kolejność wdrażania, ponieważ usługa Azure Resource Manager analizuje zależności i sprawdza, czy zasoby są tworzone we właściwej kolejności. Aby uzyskać więcej informacji, zobacz [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md) (Definiowanie zależności w szablonach usługi Azure Resource Manager).

W przypadku tworzenia rozwiązania z witryny Marketplace rozwiązanie automatycznie zawiera szablon wdrożenia. Nie trzeba tworzyć szablonu od początku — można zacząć od szablonu istniejącego rozwiązania i dostosować go do konkretnych potrzeb. Aby uzyskać szablon dla istniejącej grupy zasobów, można wyeksportować bieżący stan grupy do szablonu lub skorzystać z szablonu, którego użyto do określonego wdrożenia. Przeglądając wyeksportowany szablon, można poznać jego składnię. Aby dowiedzieć się więcej o pracy z wyeksportowanymi szablonami , zobacz [Eksportowanie szablonu usługi Azure Resource Manager z istniejących zasobów](resource-manager-export-template.md).

Nie trzeba definiować całej infrastruktury w jednym szablonie. Często dobrym rozwiązaniem jest podział wymagań dotyczących wdrożenia na szablony przeznaczone do określonego celu. Szablonów tych można bez problemu używać wielokrotnie w różnych rozwiązaniach. Aby wdrożyć dane rozwiązanie, należy utworzyć szablon wzorcowy połączony ze wszystkimi wymaganymi szablonami. Aby uzyskać więcej informacji, zobacz [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md) (Używanie szablonów połączonych w usłudze Azure Resource Manager).

Szablonu można także używać w celu aktualizacji infrastruktury. Można na przykład dodać nowy zasób do aplikacji lub dodać reguły konfiguracji dla już wdrożonych zasobów. Jeśli szablon służy do utworzenia nowego zasobu, ale ten zasób już istnieje, usługa Azure Resource Manager przeprowadzi aktualizację, zamiast tworzyć nowy zasób. Usługa Azure Resource Manager zaktualizuje istniejący zasób do stanu określonego dla nowego zasobu. Można także polecić usłudze Resource Manager usunięcie wszystkich zasobów, które nie są zdefiniowane w szablonie. Aby poznać różne opcje wdrażania, zobacz [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md) (Wdrażanie aplikacji za pomocą szablonu usługi Azure Resource Manager). 

Szablon może mieć parametry umożliwiające dostosowywanie i zapewnienie elastyczności wdrożenia. Można na przykład podać wartości parametrów dostosowujące wdrożenie do danego środowiska testowego. Określając parametry, można użyć tego samego szablonu wdrożenia do wszystkich środowisk aplikacji.

Usługa Resource Manager zapewnia rozszerzenia na potrzeby sytuacji, gdy potrzebne są dodatkowe operacje, które nie są uwzględnione w konfiguracji (np. zainstalowanie konkretnego oprogramowania). Jeśli używasz już usługi do zarządzania konfiguracją, takiej jak DSC, Chef lub Puppet, dzięki rozszerzeniom możesz z nią dalej bez przeszkód pracować.

Ponadto szablon staje się częścią kodu źródłowego aplikacji. Można go zaewidencjonować w repozytorium kodu źródłowego i aktualizować w miarę rozwijania aplikacji. Do edycji szablonu można używać programu Visual Studio.

Aby uzyskać więcej informacji na temat definiowania szablonu, zobacz [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md) (Tworzenie szablonów usługi Azure Resource Manager).

Aby uzyskać szczegółowe instrukcje dotyczące tworzenia szablonu, zobacz [Przewodnik po szablonie usługi Resource Manager](resource-manager-template-walkthrough.md).

Aby uzyskać wskazówki dotyczące wdrażania rozwiązania w różnych środowiskach, zobacz [Development and test environments in Microsoft Azure](solution-dev-test-environments.md) (Środowiska projektowe i testowe na platformie Microsoft Azure). 

## Tagi

Usługa Resource Manager udostępnia funkcję tagowania umożliwiającą dzielenie zasobów na kategorie zgodnie z wymaganiami zarządzania lub rozliczeń. Tagi są przydatne w przypadku złożonych kolekcji grup zasobów i zasobów, które trzeba uporządkować wizualnie w możliwie logicznej formie. Można na przykład oznaczyć tagami zasoby, które pełnią podobną rolę w organizacji lub należą do tego samego działu. Bez użycia tagów użytkownicy w organizacji mogą tworzyć wiele zasobów, które będą później bardzo trudne do znalezienia i zarządzania. Na przykład może być konieczne usunięcie wszystkich zasobów z określonego projektu — ale jeśli te zasoby nie zostały oznaczone tagami tego projektu, trzeba będzie je znaleźć ręcznie. Tagowanie może być istotnym sposobem na zredukowanie niepotrzebnych kosztów w ramach subskrypcji. 

Zasoby mogą być oznaczone tym samym tagiem, nawet jeśli nie znajdują się w tej samej grupie zasobów. Można utworzyć własną taksonomię tagów, aby mieć pewność, że wszyscy użytkownicy w organizacji używają tych samych tagów. Dzięki temu uniknie się pomyłek wynikających z użycia podobnych tagów (na przykład „wydział” zamiast „dział”).

Aby uzyskać więcej informacji na temat tagów, zobacz [Using tags to organize your Azure resources](resource-group-using-tags.md) (Porządkowanie zasobów na platformie Azure za pomocą tagów). Można także utworzyć [dostosowane zasady](#manage-resources-with-customized-policies) wymagające dodawania tagów do zasobów podczas wdrażania.

## Kontrola dostępu

Usługa Resource Manager pozwala kontrolować, kto może wykonywać określone czynności w organizacji. Zapewnia ona natywną integrację usługi OAuth i funkcji kontroli dostępu na podstawie ról z platformą zarządzania, umożliwiając stosowanie kontroli dostępu do wszystkich usług w grupie zasobów. Wstępnie zdefiniowane role dotyczące platform i zasobów można przypisywać użytkownikom, a następnie stosować je do subskrypcji, grup zasobów lub pojedynczych zasobów w celu zapewnienia kontroli dostępu. Można na przykład skorzystać ze wstępnie zdefiniowanej roli „Współautor bazy danych SQL” umożliwiającej użytkownikom zarządzanie bazami danych, ale nie serwerami baz danych ani zasadami zabezpieczeń. Wystarczy dodać użytkowników w organizacji, którzy potrzebują tego poziomu dostępu, do roli „Współautor bazy danych SQL” i zastosować rolę do subskrypcji, grupy zasobów lub zasobu.

Usługa Resource Manager automatycznie rejestruje czynności użytkownika na potrzeby inspekcji. Aby uzyskać informacje dotyczące pracy z dziennikami inspekcji, zobacz [Audit operations with Resource Manager](resource-group-audit.md) (Operacje inspekcji w usłudze Resource Manager).

Aby uzyskać więcej informacji na temat kontroli dostępu na podstawie ról, zobacz temat [Azure Role-Based Access Control](./active-directory/role-based-access-control-configure.md) (Kontrola dostępu na podstawie ról na platformie Azure). Listę ról wbudowanych i dozwolonych w ramach nich czynności można znaleźć w artykule [RBAC: Built in Roles](./active-directory/role-based-access-built-in-roles.md) (Kontrola dostępu na podstawie ról: role wbudowane). Role wbudowane obejmują m.in. role ogólne (Właściciel, Czytelnik czy Współautor) oraz role związane z konkretnymi usługami (Współautor·maszyny·wirtualnej, Współautor·usługi Virtual Network czy Menedżer·zabezpieczeń·SQL).

Można również jawnie zablokować dostęp do kluczowych zasobów, aby uniemożliwić użytkownikom ich usuwanie i modyfikowanie. Aby uzyskać więcej informacji, zobacz [Lock resources with Azure Resource Manager](resource-group-lock-resources.md) (Blokowanie zasobów w usłudze Azure Resource Manager).

Najlepsze rozwiązania można znaleźć w artykule [Security considerations for Azure Resource Manager](best-practices-resource-manager-security.md) (Zagadnienia dotyczące zabezpieczeń w usłudze Azure Resource Manager).

## Zarządzanie zasobami za pomocą zasad niestandardowych

Usługa Resource Manager umożliwia tworzenie zasad niestandardowych na potrzeby zarządzania zasobami. Można tworzyć zasady różnych typów, np. wymuszające konwencję nazewnictwa zasobów, ograniczające typy i wystąpienia wdrażanych zasobów, ograniczające regiony dla danego typu zasobu czy wprowadzające wymaganie wartości tagów w zasobach w celu uporządkowania rozliczeń według działów. Tworzenie zasad umożliwia obniżenie kosztów i zachowanie spójności w ramach subskrypcji. Aby uzyskać więcej informacji, zobacz [Use Policy to manage resources and control access](resource-manager-policy.md) (Zarządzanie zasobami i kontrola dostępu przy użyciu zasad).

## Spójna warstwa zarządzania

Usługa Resource Manager umożliwia wykonywanie w pełni zgodnych operacji za pomocą programu Azure PowerShell, interfejsu wiersza polecenia platformy Azure dla systemów Mac, Linux i Windows, Portalu Azure i interfejsu API REST. Można korzystać z interfejsu, który najlepiej sprawdza się w danej sytuacji, oraz szybko zmieniać interfejsy bez obaw o zgodność. W przypadku czynności wykonywanych poza portalem wyświetlane są nawet powiadomienia w portalu.

Aby uzyskać informacje dotyczące programu PowerShell, zobacz [Using Azure PowerShell with Resource Manager](powershell-azure-resource-manager.md) (Używanie programu Azure PowerShell z usługą Resource Manager) i [Azure Resource Manager Cmdlets](https://msdn.microsoft.com/library/azure/dn757692.aspx) (Polecenia cmdlet usługi Azure Resource Manager).

Aby uzyskać informacje dotyczące interfejsu wiersza polecenia platformy Azure, zobacz [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md) (Używanie interfejsu wiersza polecenia platformy Azure na komputerach Mac i komputerach z systemem Linux oraz Windows z usługą Azure Resource Manager).

Aby uzyskać informacje dotyczące interfejsu API REST, zobacz [Azure Resource Manager REST API Reference](https://msdn.microsoft.com/library/azure/dn790568.aspx) (Dokumentacja interfejsu API REST usługi Azure Resource Manager). Aby wyświetlić operacje REST dla wdrożonych zasobów, zobacz [Use Azure Resource Explorer to view and modify resources](resource-manager-resource-explorer.md) (Wyświetlanie i modyfikowanie zasobów za pomocą Eksploratora zasobów Azure).

Aby uzyskać informacje dotyczące korzystania z portalu, zobacz [Korzystanie z Portalu Azure do zarządzania zasobami Azure](./azure-portal/resource-group-portal.md).

Usługa Azure Resource Manager obsługuje mechanizm współużytkowania zasobów między źródłami (cross-origin resource sharing — CORS). Mechanizm ten umożliwia wywołanie interfejsu API REST usługi Resource Manager lub interfejsu API REST usługi platformy Azure z poziomu aplikacji sieci Web znajdującej się w innej domenie. Bez obsługi mechanizmu CORS przeglądarki internetowe blokowałyby aplikacji w jednej domenie dostęp do zasobów w innej domenie. Usługa Resource Manager obsługuje mechanizm CORS dla wszystkich żądań z prawidłowymi poświadczeniami uwierzytelniania.

## Następne kroki

- Artykuł [Eksportowanie szablonu usługi Azure Resource Manager z istniejących zasobów](resource-manager-export-template.md) zawiera proste instrukcje dotyczące pracy z szablonami.
- Bardziej szczegółowe instrukcje dotyczące tworzenia szablonu zawiera artykuł [Przewodnik po szablonie usługi Resource Manager](resource-manager-template-walkthrough.md).
- Aby poznać funkcje, których można użyć w szablonie, zobacz [Template functions](resource-group-template-functions.md) (Funkcje szablonu).
- Aby uzyskać informacje dotyczące korzystania z programu Visual Studio z usługą Resource Manager, zobacz [Tworzenie i wdrażanie grup zasobów platformy Azure za pomocą programu Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

Oto film z omówieniem tego zagadnienia:

[AZURE.VIDEO azure-resource-manager-overview]



<!--HONumber=Jun16_HO2-->


