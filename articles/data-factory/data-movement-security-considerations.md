---
title: "Zagadnienia dotyczące zabezpieczeń w fabryce danych Azure | Dokumentacja firmy Microsoft"
description: "Zawiera opis podstawowych zabezpieczeń infrastrukturę, która umożliwia Zabezpieczanie danych usługi przenoszenia danych z fabryki danych Azure."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2018
ms.author: abnarain
ms.openlocfilehash: 898e6914a427b2e8864d97a7188eb718811ce263
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2018
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Fabryka danych Azure — zagadnienia dotyczące zabezpieczeń dla przepływu danych
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-data-movement-security-considerations.md)
> * [Wersja 2 — wersja zapoznawcza](data-movement-security-considerations.md)

W tym artykule opisano infrastrukturę podstawowych zabezpieczeń, która umożliwia Zabezpieczanie danych usługi przenoszenia danych z fabryki danych Azure. Zasoby dotyczące zarządzania fabryki danych Azure są tworzone w infrastrukturze zabezpieczeń platformy Azure i użyj wszystkich możliwych zabezpieczenia oferowanych na platformie Azure.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [zagadnienia dotyczące zabezpieczeń przepływu danych dla fabryki danych w wersji 1](v1/data-factory-data-movement-security-considerations.md).

W ramach rozwiązania fabryki danych jest tworzony co najmniej jeden [potok](concepts-pipelines-activities.md) danych. Potoki to logiczne grupy działań, które wspólnie wykonują zadanie. Potoki te znajdują się w regionie, w której został utworzony z fabryką danych. 

Mimo że fabryki danych jest dostępna tylko w **wschodnie stany USA**, **wschodnie stany USA 2**, i **Europa Zachodnia** regionów (wersja zapoznawcza version 2), usługa przenoszenia danych jest dostępne [globalny w wielu regionach](concepts-integration-runtime.md#azure-ir). Jeśli usługa przenoszenia danych nie została jeszcze wdrożona do tego regionu, usługi fabryka danych zapewnia, że dane nie opuści obszaru geograficznego / regionu, chyba że jawnie poinstruować usługę, aby użyć alternatywnej regionu. 

Fabryka danych Azure, sam nie przechowuje żadnych danych, z wyjątkiem poświadczeń połączonej usługi dla magazynów danych chmury, które są szyfrowane za pomocą certyfikatów. Usługa umożliwia tworzenie opartych na danych przepływów pracy do aranżowania przenoszenia danych między [obsługiwanymi magazynami danych](copy-activity-overview.md#supported-data-stores-and-formats) oraz przetwarzania danych przy użyciu [usług obliczeniowych](compute-linked-services.md) w innych regionach lub w środowisku lokalnym. Można też monitorować i zarządzać przepływów pracy za pomocą monitora Azure i zestawy SDK.

Przenoszenie danych przy użyciu fabryki danych Azure została **certyfikowane** dla:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA) 
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018)
-   [CSA STAR](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)

Jeśli interesuje Cię zgodności platformy Azure i jak Azure zabezpiecza własnej infrastruktury, odwiedź stronę [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx). 

W tym artykule firma Microsoft analizuje zagadnienia dotyczące zabezpieczeń w następujących scenariuszach przepływu danych dwóch: 

- **Scenariusz chmury**— w tym scenariuszu źródłowym i docelowym są publicznie dostępny za pośrednictwem Internetu. Obejmują one usługi magazynu w chmurze zarządzanych, takich jak usługi Azure Storage, Magazyn danych SQL Azure, baza danych SQL Azure, Azure Data Lake Store, Amazon S3, Amazon Redshift, usługi SaaS, takie jak Salesforce i protokoły sieci web, takich jak FTP i OData. Pełną listę obsługiwanych źródeł danych można znaleźć [tutaj](copy-activity-overview.md#supported-data-stores-and-formats).
- **Scenariusza hybrydowego**— w tym scenariuszu źródłowe lub docelowe znajduje się za zaporą lub wewnątrz sieci firmowej lokalnymi lub dane magazynu jest w sieci prywatnej / wirtualnych sieci (w większości przypadków źródła) i nie jest dostępny publicznie. W tym scenariuszu dzielą się również hostowanych na maszynach wirtualnych serwerów bazy danych.

## <a name="cloud-scenarios"></a>Scenariusze chmury
###<a name="securing-data-store-credentials"></a>Zabezpieczanie poświadczeń magazynu danych
- Przechowywanie zaszyfrowane poświadczenia w magazynie zarządzanych fabryki danych Azure.

   Fabryka danych Azure chroni poświadczeń magazynu danych przez **szyfrowania** je za pomocą **certyfikaty zarządzany przez firmę Microsoft**. Te certyfikaty są obracane co **dwuletni** (w tym migracja poświadczeń i odnawiania certyfikatów). Te zaszyfrowane poświadczenia są bezpiecznie przechowywane w **magazyn Azure zarządzanych przez usługi zarządzania fabryki danych Azure**. Aby uzyskać więcej informacji na temat zabezpieczeń usługi Azure Storage można znaleźć [Omówienie zabezpieczeń magazynu Azure](../security/security-storage-overview.md).
- Przechowywanie poświadczeń w usłudze Azure Key Vault 

   Możesz teraz przechowywane poświadczenia magazynu danych w [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/), następnie umożliwił fabryki danych Azure, aby go uzyskać, podczas wykonywania działania. Aby uzyskać więcej informacji, zobacz [magazynu poświadczeń w usłudze Azure Key Vault](store-credentials-in-key-vault.md).

### <a name="data-encryption-in-transit"></a>Szyfrowanie danych podczas przesyłania
W przypadku magazynu danych w chmurze obsługuje protokół HTTPS lub TLS, wszystkie dane transferu między usługi przenoszenia danych z fabryki danych i magazynu danych w chmurze są za pośrednictwem bezpiecznego kanału HTTPS lub TLS.

> [!NOTE]
> Wszystkie połączenia **bazy danych SQL Azure** i **magazyn danych SQL Azure** zawsze wymagają szyfrowania (SSL/TLS), podczas gdy dane są przesyłane, do i z bazy danych. Podczas tworzenia potoku przy użyciu formatu JSON, Dodaj **szyfrowania** właściwości i wartości **true** w **ciąg połączenia**. Aby uzyskać **usługi Azure Storage**, można użyć **HTTPS** w parametrach połączenia.

### <a name="data-encryption-at-rest"></a>Szyfrowanie danych w spoczynku
Niektóre dane są przechowywane Obsługa szyfrowania danych magazynowanych. Zalecamy, aby włączyć mechanizm szyfrowania danych dla tych magazynów danych. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Przezroczysty danych szyfrowania (funkcji TDE) w usłudze Azure SQL Data Warehouse pomaga w ochronie przed zagrożeniem złośliwych działań, wykonując w czasie rzeczywistym szyfrowania i odszyfrowywania danych przechowywanych. To zachowanie jest niewidoczny dla klienta. Aby uzyskać więcej informacji, zobacz [Zabezpieczanie bazy danych w usłudze SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Baza danych SQL Azure obsługuje również przezroczystego szyfrowania danych (funkcji TDE), która pomaga w ochronie przed zagrożeniem złośliwych działań, wykonując w czasie rzeczywistym szyfrowania i odszyfrowywania danych bez konieczności wprowadzania zmian w aplikacji. To zachowanie jest niewidoczny dla klienta. Aby uzyskać więcej informacji, zobacz [przezroczystego szyfrowania danych z bazy danych SQL Azure](/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Usługa Azure Data Lake store zapewnia również szyfrowanie danych przechowywanych w ramach konta. Po włączeniu usługi Data Lake store szyfruje dane przed wprowadzeniem trwałych i automatycznie odszyfrowuje przed pobierania, ustawiania przezroczystości do uzyskiwania dostępu do danych klienta. Aby uzyskać więcej informacji, zobacz [zabezpieczeń w usłudze Azure Data Lake Store](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Magazyn obiektów Blob platformy Azure i magazynu tabel platformy Azure
Usługa Azure storage magazynu obiektów Blob i tabel Azure obsługuje magazynu usługi szyfrowania (SSE), który szyfruje dane przed wprowadzeniem trwałych do magazynu i automatycznie odszyfrowuje przed pobierania. Aby uzyskać więcej informacji, zobacz [szyfrowanie usługi Magazyn Azure dla danych magazynowanych](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 obsługuje szyfrowanie danych przechowywanych klienta i serwera. Aby uzyskać więcej informacji, zobacz [ochrona danych przy użyciu szyfrowania](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift obsługuje szyfrowanie klastra dla przechowywanych danych. Aby uzyskać więcej informacji, zobacz [szyfrowania bazy danych Redshift Amazon](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). 

#### <a name="salesforce"></a>SalesForce
SalesForce obsługuje szyfrowanie platformy osłony, który umożliwia szyfrowanie wszystkich plików, załączników i pola niestandardowe. Aby uzyskać więcej informacji, zobacz [opis przepływ uwierzytelniania OAuth serwera sieci Web](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-self-hosted-integration-runtime"></a>Scenariuszy hybrydowych (przy użyciu siebie integracji środowiska wykonawczego)
Scenariusze hybrydowe wymaga środowiska uruchomieniowego integracji siebie do zainstalowania w sieci lokalnej lub w sieci wirtualnej (Azure) lub prywatnej chmury wirtualne (Amazon). Hostowanie Samoobsługowe integrację środowiska uruchomieniowego musi mieć możliwość dostępu magazynów danych lokalnych. Aby uzyskać więcej informacji dotyczących środowiska uruchomieniowego integracji hostowania samoobsługowego, zobacz [środowiska uruchomieniowego integracji hosta samodzielnego](create-self-hosted-integration-runtime.md). 

![kanały środowiska uruchomieniowego integracji hostowanie Samoobsługowe](media/data-movement-security-considerations/data-management-gateway-channels.png)

**Kanał poleceń** umożliwia komunikację między usługi przenoszenia danych z fabryki danych i środowiska uruchomieniowego integracji siebie. Komunikat zawiera informacje związane z działaniem. Kanał danych jest używany do przenoszenia danych między lokalnych magazynów danych i magazyny danych w chmurze.    

### <a name="on-premises-data-store-credentials"></a>Dane lokalne przechowywanie poświadczeń
Poświadczenia dla sieci lokalnych magazynów danych są zawsze szyfrowane i przechowywane. Być może być albo przechowywane lokalnie na komputerze środowiska uruchomieniowego integracji hostowania samoobsługowego, lub w fabryce danych Azure zarządzanego magazynu (podobnie jak magazynu poświadczeń w chmurze). 

1. Istnieje możliwość **przechowywania poświadczeń lokalnie**. Szyfrowanie i przechowywanie poświadczeń lokalnie na siebie integrację środowiska uruchomieniowego, należy wykonać czynności opisane w [szyfrowania poświadczeń na siebie integrację środowiska uruchomieniowego](encrypt-credentials-self-hosted-integration-runtime.md). Wszystkie łączniki obsługuje tę opcję. Przy użyciu środowiska uruchomieniowego integracji siebie [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) do szyfrowania poufnych danych / poświadczeń informacji. 

   Użyj **AzureRmDataFactoryV2LinkedServiceEncryptedCredential nowy** polecenia cmdlet do szyfrowania poświadczeń dostępu do połączonej usługi / szyfrowania poufnych informacji w połączonej usłudze. Można użyć JSON zwrócił (z **EncryptedCredential** element **connectionString**) można utworzyć połączonej usługi przez **Set-AzureRmDataFactoryV2LinkedSevrice**polecenia cmdlet.  

2. Jeśli nie używasz **AzureRmDataFactoryV2LinkedServiceEncryptedCredential nowy** polecenia cmdlet jako opisanych w kroku powyżej, a zamiast korzystać bezpośrednio **AzureRmDataFactoryV2LinkedSevrice zestaw** polecenie cmdlet z połączenie ciągów / poświadczenia wbudowanego w formacie JSON, a następnie zostanie połączona usługa **zaszyfrowane i będą przechowywane w usłudze fabryka danych Azure zarządzanego magazynu**. Poufne informacje nadal są szyfrowane przy certyfikatów i certyfikaty te są zarządzane przez firmę Microsoft.



#### <a name="ports-used-during-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Porty używane podczas szyfrowania połączonej usługi na siebie integrację środowiska uruchomieniowego
Domyślnie program PowerShell korzysta z portu **8050** na komputerze z programem obsługi integracji hostowanie Samoobsługowe dla bezpiecznej komunikacji. W razie potrzeby można zmienić tego portu.  

![Port HTTPS dla bramy](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Szyfrowanie podczas przesyłania
Są wszystkie operacje transferu danych za pośrednictwem bezpiecznego kanału **HTTPS** i **TLS za pośrednictwem protokołu TCP** przed atakami man-in--middle podczas komunikowania się z usługami Azure.

Można również użyć [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) lub [Express Route](../expressroute/expressroute-introduction.md) dodatkowo zabezpieczyć kanał komunikacyjny między siecią lokalną a Azure.

Sieć wirtualna jest logiczną reprezentacja sieci w chmurze. Można połączyć sieć lokalną do sieci wirtualnej platformy Azure (VNet) przy konfigurowania sieci VPN IPSec (site-to-site) lub usługi Express Route (prywatnej komunikacji równorzędnej)     

W poniższej tabeli przedstawiono sieci i zalecenia dotyczące konfiguracji środowiska uruchomieniowego integracji siebie oparte na różnych kombinacji źródłowe i docelowe lokalizacje hybrydowego przenoszenia danych.

| Element źródłowy      | Element docelowy                              | Konfiguracja sieci                    | Ustawienia środowiska uruchomieniowego integracji                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Lokalnie | Maszyny wirtualne i usługi w chmurze wdrożony w sieci wirtualnych | IPSec VPN (punkt lokacja lub lokacja lokacja) | Hostowanie Samoobsługowe integrację środowiska uruchomieniowego może być zainstalowana lokalnie lub na platformie Azure wirtualnej maszyny (VM) w sieci wirtualnej |
| Lokalnie | Maszyny wirtualne i usługi w chmurze wdrożony w sieci wirtualnych | ExpressRoute (prywatnej komunikacji równorzędnej)           | Hostowanie Samoobsługowe integrację środowiska uruchomieniowego może być zainstalowana lokalnie lub na maszynie Wirtualnej platformy Azure w sieci wirtualnej |
| Lokalnie | Usług Azure, których publiczny punkt końcowy | ExpressRoute (publicznej komunikacji równorzędnej)            | Hostowanie Samoobsługowe integrację środowiska uruchomieniowego musi być zainstalowany na lokalnym |

Na poniższych ilustracjach przedstawiono użycie środowiska uruchomieniowego integracji siebie do przenoszenia danych między lokalną bazą danych i usług Azure przy użyciu expressroute i sieci VPN IPSec (z sieci wirtualnej):

**Usługi Express Route:**

![Użyj bramy Expressroute](media/data-movement-security-considerations/express-route-for-gateway.png) 

**Protokół IPSec sieci VPN:**

![Sieć VPN IPSec z bramy](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-self-hosted-integration-runtime"></a>Konfiguracje zapór i listę dozwolonych podobnej adresu IP (hostowanie Samoobsługowe integracji środowiska wykonawczego)

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Wymagania dotyczące zapory dla sieci o lokalnym/prywatnym  
W przedsiębiorstwie **firmowej zapory** działa na routerze centralnej w organizacji. A **zapory systemu Windows** działa jako demon na komputerze lokalnym, na którym zainstalowano środowiska uruchomieniowego integracji siebie. 

W poniższej tabeli przedstawiono **port wyjściowy** i wymagania dotyczące domeny dla **firmowej zapory**.

| Nazwy domen                  | Porty wyjściowe | Opis                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443, 80        | Wymagane przez siebie integrację środowiska uruchomieniowego do nawiązania połączenia usługi przenoszenia danych z fabryki danych |
| `*.core.windows.net`          | 443            | Używane przez siebie integrację środowiska uruchomieniowego do łączenia się konta magazynu Azure, korzystając z [przemieszczane kopiowania](copy-activity-performance.md#staged-copy) funkcji. |
| `*.frontend.clouddatahub.net` | 443            | Wymagane przez siebie integrację środowiska uruchomieniowego do nawiązania połączenia usługi fabryka danych Azure. |
| `*.database.windows.net`      | 1433           | (OPCJONALNIE) wymagane, jeśli folderem docelowym jest baza danych Azure SQL / Azure SQL Data Warehouse. Funkcja kopiowania przemieszczanego skopiować dane do usługi Azure SQL bazy danych/usługi Azure SQL Data Warehouse, bez konieczności otwierania portu 1433. |
| `*.azuredatalakestore.net`    | 443            | (OPCJONALNIE) potrzebna do folderu docelowego jest Azure Data Lake store |

> [!NOTE] 
> Może być konieczne zarządzanie portami / listę dozwolonych podobnej domen w firmowej zapory na poziomie zgodnie z wymaganiami źródeł odpowiednich danych. Ta tabela używa tylko bazy danych SQL Azure, Magazyn danych SQL Azure, Azure Data Lake Store jako przykłady.   

W poniższej tabeli przedstawiono **port wejściowy** wymagania dotyczące **zapory systemu Windows**.

| Porty wejściowe | Opis                              |
| ------------- | ---------------------------------------- |
| 8050 (TCP)    | Wymagany przez polecenie cmdlet programu PowerShell szyfrowania zgodnie z opisem w [szyfrowania poświadczeń na siebie integrację środowiska uruchomieniowego](encrypt-credentials-self-hosted-integration-runtime.md)/ poświadczeń Menedżera aplikacji w celu bezpiecznego ustawienia poświadczeń dla lokalnych magazynów danych na hostowanie Samoobsługowe Integracja środowiska wykonawczego. |

![Wymagania dotyczące portów bramy](media\data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurationswhitelisting-in-data-store"></a>Konfiguracje IP/niedozwolonych w magazynie danych
Niektóre sklepy danych w chmurze również wymagać niedozwolonych adres IP komputera, dostępu do nich. Upewnij się, że adres IP maszyny środowiska uruchomieniowego integracji hostowania samoobsługowego jest białej / odpowiednio skonfigurowane w zaporze.

Następujące magazyny danych chmury wymagają niedozwolonych adres IP komputera środowiska uruchomieniowego integracji siebie. Niektóre z tych magazynów danych, domyślnie nie mogą wymagać niedozwolonych adresów IP. 

- [Azure SQL Database](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Często zadawane pytania

**Pytanie:** można udostępniać środowiska uruchomieniowego integracji hostowanie Samoobsługowe dla fabryki danych?
**Odpowiedź:** nie obsługujemy tej funkcji jeszcze. Aktywnie pracujemy nad go.

**Pytanie:** jakie są wymagania dotyczące portów siebie integrację środowiska uruchomieniowego do pracy?
**Odpowiedź:** siebie integrację środowiska uruchomieniowego umożliwia nawiązanie połączenia oparte na protokole HTTP można otworzyć internet. **Wychodzącego porty 443 i 80** muszą być otwarte dla siebie integrację środowiska uruchomieniowego, to połączenie. Otwórz **ruchu przychodzącego dla portu 8050** tylko na poziomie komputera (nie na poziomie firmowej zapory) dla aplikacji Menedżer poświadczeń. Jeśli baza danych SQL Azure lub usługi Azure SQL Data Warehouse jest używany jako źródło / docelowego, następnie trzeba otworzyć **1433** również port. Aby uzyskać więcej informacji, zobacz [konfiguracji i adresy IP listę dozwolonych podobnej zapory](#firewall-configurations-and-whitelisting-ip-address-of gateway) sekcji. 


## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać informacje o wydajności działania kopiowania, zobacz [skopiuj wydajności działania i dostrajania przewodnik](copy-activity-performance.md).

 
