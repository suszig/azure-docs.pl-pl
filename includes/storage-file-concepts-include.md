## Co to jest usługa Azure File Storage?
Usługa File Storage oferuje współużytkowany magazyn dla aplikacji używających standardowego protokołu SMB 2.1 lub SMB 3.0. Usługa Microsoft Azure Virtual Machines i usługi w chmurze mogą udostępniać dane między składnikami aplikacji za pośrednictwem zainstalowanych udziałów i aplikacji lokalnych, które mogą uzyskiwać dostęp do danych plików w udziale za pomocą interfejsu API usługi File Storage.

Aplikacje uruchomione na maszynach wirtualnych lub w ramach usług w chmurze platformy Azure mogą zainstalować udziały usługi File Storage, aby uzyskać dostęp do danych plików tak samo jak aplikacja na komputerze instalująca typowy udział SMB. Dowolna liczba maszyn wirtualnych lub ról platformy Azure może następnie równocześnie zainstalować udział usługi File Storage i uzyskiwać do niego dostęp.

Ponieważ udział usługi File Storage to standardowy udział plików na platformie Azure przy użyciu protokołu SMB, aplikacje działające na platformie Azure mają dostęp do danych w udziale za pośrednictwem interfejsów API we/wy plików. Dzięki temu programiści mogą wykorzystać istniejący kod i własne umiejętności, aby zmigrować istniejące aplikacje. Specjaliści IT mogą użyć poleceń cmdlet programu PowerShell do tworzenia i instalowania udziałów magazynu plików oraz do zarządzania nimi w ramach administracji aplikacjami platformy Azure. W tym podręczniku przedstawiono oba przykłady.

Najczęstsze zastosowania usługi File Storage to:

* Migracja lokalnych aplikacji korzystających z udziałów plików w celu uruchomienia na maszynach wirtualnych lub w usługach w chmurze platformy Azure, bez kosztownego ponownego zapisu
* Przechowywanie udostępnionych ustawień aplikacji, na przykład w plikach konfiguracji
* Przechowywanie danych diagnostycznych, takich jak dzienniki, metryki i zrzuty awaryjne we współdzielonej lokalizacji 
* Przechowywanie narzędzi i programów potrzebnych do tworzenia maszyn wirtualnych lub usług i administrowania nimi w chmurze platformy Azure

## Pojęcia dotyczące usługi File Storage
Usługa File Storage zawiera następujące składniki:

![files-concepts][files-concepts]

* **Konto magazynu:** cały dostęp do usługi Azure Storage odbywa się przez konto magazynu. Aby uzyskać szczegółowe informacje na temat pojemności konta magazynu, zobacz temat [Cele dotyczące skalowalności i wydajności usługi Azure Storage](../articles/storage/storage-scalability-targets.md).
* **Udział:** udział usługi File Storage jest udziałem plików SMB na platformie Azure. 
  Wszystkie pliki i katalogi muszą być tworzone w udziale nadrzędnym. Konto może zawierać nieograniczoną liczbę udziałów, a udział może obejmować nieograniczoną liczbę plików. Maksymalna całkowita objętość udziału plików to 5 TB.
* **Katalog:** opcjonalna hierarchia katalogów. 
* **Plik:** plik w udziale. Plik może mieć maksymalnie 1 TB pojemności.
* **Format adresu URL:** adresy URL plików mają następujący format:   
  https://`<storage
  account>`.file.core.windows.net/`<share>`/`<directory/directories>`/`<file>`  
  
  Następującego przykładowego adresu URL można użyć do udostępnienia jednego z plików na powyższym diagramie:  
  `http://samples.file.core.windows.net/logs/CustomLogs/Log1.txt`

Szczegółowe informacje o nazewnictwie udziałów, katalogów i plików można znaleźć w temacie [Naming and Referencing Shares, Directories, Files, and Metadata](http://msdn.microsoft.com/library/azure/dn167011.aspx) (Nazywanie i odwoływanie się do udziałów, katalogów, plików i metadanych).

[files-concepts]: ./media/storage-file-concepts-include/files-concepts.png

<!--HONumber=Sep16_HO3-->


