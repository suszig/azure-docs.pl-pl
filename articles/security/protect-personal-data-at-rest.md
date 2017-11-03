---
title: Azure ochrona danych osobowych magazynowane z szyfrowaniem | Dokumentacja firmy Microsoft
description: "W tym artykule jest częścią serii, pomaga Azure umożliwia ochronę danych osobowych"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 2bb8370d23d9450fb8154f21c27817666fd7852c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a>Technologii szyfrowania Azure: ochrony danych osobowych w stanie spoczynku z szyfrowaniem

Ten artykuł pomaga w zrozumieniu i użytkowaniu technologii szyfrowania Azure do zabezpieczania danych w stanie spoczynku.

Szyfrowanie danych magazynowanych jest najlepszym rozwiązaniem do ochrony danych poufnych i osobistych i spełniać wymagania dotyczące zachowania poufności danych i zgodności.
Szyfrowanie magazynowanych zaprojektowano w celu uniemożliwić osobie atakującej dostęp do niezaszyfrowane dane przez zapewnienie im dane są szyfrowane, gdy na dysku.

## <a name="scenario"></a>Scenariusz 

Rejs duże przedsiębiorstwo, siedzibą w Stanach Zjednoczonych rozwija operacjach oferowanie trasy w DS i Bałtyckiego mórz oraz brytyjskich. Aby zapewnić obsługę tych działań, ustawił mniejszych rejs wiersze na podstawie we Włoszech, Niemczech, Dania i Zjednoczone Królestwo

Przez firmę Microsoft Azure do przechowywania danych firmowych w chmurze. Może to obejmować informacje klienta i/lub pracowników, takich jak:

- adresy
- Numery telefonów
- Numery identyfikacyjne podatku
- informacje o karcie kredytowej

Firmy muszą chronić poufności danych klienta i pracowników, podczas udostępniania danych dostępne dla tych działów, które go potrzebują. (na przykład lista płac i zastrzeżenia działów)

Wiersz rejs zachowuje również dużej bazy danych elementów członkowskich programu osób trzecich i lojalność zawierający informacje osobiste, aby śledzić relacje z bieżących i starszych klientów.

### <a name="problem-statement"></a>Opis problemu

Firma musi ochrony prywatności danych osobowych pracowników i klientów podczas przesyłania danych dostępne dla tych działów, które go potrzebują (na przykład lista płac i zastrzeżenia działów). Tej zapewnionej znajduje się poza centrum danych firmy pod kontrolą i nie jest pod kontrolą fizycznych firmy.

### <a name="company-goal"></a>Celem firmy

W ramach strategii zabezpieczeń obrony zabezpieczeń wielowarstwowy jest celem firmy, aby upewnić się, że wszystkie źródła danych, które zawierają dane osobowe użytkownika są szyfrowane, łącznie z tymi znajdującymi się w magazynie w chmurze. Jeśli nieupoważnionym osobom uzyskanie dostępu do danych osobowych, musi należeć do formularza, który będzie renderowany go nie można go odczytać. Zastosowanie szyfrowania powinna być łatwa lub przezroczyste — dla użytkowników i administratorów.

## <a name="solutions"></a>Rozwiązania

Usługi Azure zapewniają wiele narzędzia i technologie, aby pomóc w ochronie danych osobowych w stanie spoczynku szyfrując go.

### <a name="azure-key-vault"></a>W usłudze Azure Key Vault

[Usługa Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) zapewnia bezpieczny magazyn kluczy, używany do szyfrowania danych przechowywanych w usługach Azure i jest zalecanym klucza magazynu i zarządzania rozwiązaniem. Zarządzanie kluczami szyfrowania jest niezbędne do zabezpieczania przechowywanych danych.

#### <a name="how-do-i-use-azure-key-vault-to-protect-keys-that-encrypt-personal-data"></a>Jak używać usługi Azure Key Vault do ochrony kluczy szyfrowania danych osobowych?

Aby korzystać z usługi Azure Key Vault, konieczna jest subskrypcja z kontem platformy Azure. Należy również zainstalować programu Azure PowerShell. Kroki obejmują przy użyciu poleceń cmdlet programu PowerShell, można wykonać następujące czynności:

1. Nawiązywanie połączenia z subskrypcjami

2. Tworzenie magazynu kluczy

3. Dodawanie klucza lub klucza tajnego do magazynu kluczy

4. Rejestrowanie aplikacji używających magazynu kluczy z usługą Azure Active Directory

5. Zezwolić aplikacjom na używanie klucza lub klucza tajnego

Aby utworzyć magazyn kluczy, użyj polecenia cmdlet programu PowerShell New-AzureRmKeyVault. Przypisze nazwę magazynu, nazwa grupy zasobów i lokalizacji geograficznej. Podczas zarządzania kluczami za pośrednictwem innych poleceń cmdlet użyjesz nazwy magazynu. Aplikacje używające magazynu za pomocą interfejsu API REST użyje magazynu identyfikatora URI.

Usługa Azure Key Vault zapewniają klucza chronionego przez oprogramowanie dla Ciebie lub zaimportować istniejący klucz w. Plik PFX. Można również przechowywać klucze tajne (hasła) w magazynie.

Można również wygenerowanie klucza w lokalnym module HSM i przeniesienie go do modułów HSM w usłudze Key Vault bez opuszczania granic modułu HSM klucz.

Aby uzyskać szczegółowe instrukcje na temat używania usługi Azure Key Vault, postępuj zgodnie z instrukcjami [wprowadzenie do usługi Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)

Aby uzyskać listę poleceń cmdlet programu PowerShell używane z usługą Azure Key Vault, zobacz [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

### <a name="azure-disk-encryption-for-windows"></a>Szyfrowanie dysków Azure dla systemu Windows

[Azure szyfrowania dysku dla systemu Windows i maszyn wirtualnych systemu Linux IaaS](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) chroni dane osobowe magazynowane na maszynach wirtualnych Azure i integruje się z usługą Azure Key Vault. Używa szyfrowania dysków Azure [funkcji BitLocker](https://technet.microsoft.com/library/cc732774.aspx) w systemie Windows i [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) w systemie Linux można zaszyfrować zarówno systemu operacyjnego i dysków z danymi. Szyfrowanie dysków Azure jest obsługiwana na Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 i na klientach z systemem Windows 8 i Windows 10.

#### <a name="how-do-i-use-azure-disk-encryption-to-protect-personal-data"></a>Jak używać szyfrowania dysków Azure do ochrony danych osobowych?

Aby korzystać z szyfrowania dysków Azure, konieczna jest subskrypcja z kontem platformy Azure. Aby włączyć Azure dysku szyfrowanie dla systemu Windows i maszyn wirtualnych systemu Linux, wykonaj następujące czynności:

1. Aby włączyć szyfrowanie dysków i określenia konfiguracji szyfrowania, należy użyć szablon Menedżera zasobów szyfrowania dysków Azure, programu PowerShell lub interfejsu wiersza polecenia (CLI). 

2. Udziel dostępu do platformy Azure do odczytu z magazynu kluczy materiałów szyfrowania.

3. Podaj tożsamość aplikacji usługi Azure Active Directory (AAD) do zapisu materiału klucza szyfrowania magazynu kluczy.

Azure zaktualizować maszyny Wirtualnej oraz konfiguracji magazynu kluczy i skonfigurować zaszyfrowanych maszyny Wirtualnej.

Po skonfigurowaniu magazynu kluczy do obsługi szyfrowania dysków Azure można dodać klucza szyfrowania klucza (KEK), aby zwiększyć bezpieczeństwo i do obsługi kopii zapasowych maszyn wirtualnych zaszyfrowanych.

![](media/protect-personal-data-at-rest/create-key.png)

Szczegółowe instrukcje dotyczące scenariuszy wdrażania i możliwości użytkowników znajdują się w [Azure dysku szyfrowanie dla systemu Windows i maszyn wirtualnych systemu Linux IaaS.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)

### <a name="azure-storage-service-encryption"></a>Szyfrowanie usługi Azure Storage

[Azure magazynu usługi szyfrowania (SSE) dla danych magazynowanych](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) pomaga chronić i ochrony danych w celu spełnienia Twojej organizacji zobowiązań zabezpieczeń i zgodności. Magazynu Azure szyfruje dane przy użyciu szyfrowania AES 256-bitowego przed wprowadzeniem trwałych do magazynu i automatycznie odszyfrowuje je przed pobierania. Ta usługa jest dostępna dla obiektów blob Azure i plików.

#### <a name="how-do-i-use-storage-service-encryption-to-protect-personal-data"></a>Jak używać szyfrowanie usługi magazynu do ochrony danych osobowych?

Aby włączyć szyfrowanie usługi Magazyn, wykonaj następujące czynności:

1. Zaloguj się do portalu Azure.

2. Wybierz konto magazynu.

3. W obszarze Ustawienia w sekcji usługi obiektów Blob, wybierz szyfrowania.

4. W sekcji usługi plików wybierz opcję szyfrowania.

Kliknij przycisk Ustawienia szyfrowania, można włączyć lub wyłączyć szyfrowanie usługi magazynu.

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

Nowe dane będą szyfrowane. Dane w istniejących plików z tego konta magazynu pozostaną niezaszyfrowane.

Po włączeniu szyfrowania, skopiuj dane do konta magazynu przy użyciu jednej z następujących metod:

1. Kopiowanie obiektów blob lub pliki z [narzędzia wiersza polecenia AzCopy](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).

2. [Instalowanie udziału plików, za pomocą protokołu SMB](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) dzięki czemu można użyć narzędzia, takiego jak Robocopy do kopiowania plików.

3. Kopiowanie obiektów blob lub pliku danych do i z magazynu obiektów blob lub między magazynu kont przy użyciu narzędzia [biblioteki klienta magazynu, takich jak .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).

4.  Użyj [Eksploratora usługi Storage](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) przekazać obiekty BLOB na koncie magazynu z włączone szyfrowanie.

### <a name="transparent-data-encryption"></a>Przezroczyste szyfrowanie danych

Funkcji przezroczystego szyfrowania danych (TDE) jest funkcją w usługach SQL Azure za pomocą którego można zaszyfrować dane na poziomie bazy danych i serwera. Funkcji TDE jest teraz włączone domyślnie na wszystkie nowo utworzone bazy danych. Funkcji TDE wykonuje w czasie rzeczywistym we/wy szyfrowania i odszyfrowywania plików danych i dziennika.

#### <a name="how-do-i-use-tde-to-protect-personal-data"></a>Jak używać funkcji TDE do ochrony danych osobowych?

Za pomocą interfejsu API REST lub przy użyciu programu PowerShell, można skonfigurować funkcji TDE za pośrednictwem portalu Azure. Aby włączyć funkcji TDE na istniejącej bazy danych przy użyciu portalu Azure, wykonaj następujące czynności:

1. Odwiedź portal Azure w <https://portal.azure.com> i zaloguj się przy użyciu konta administratora platformy Azure lub współautora.

2. Na banerze po lewej stronie kliknij do PRZEGLĄDANIA, a następnie kliknij bazy danych SQL.

3. Z bazy danych SQL w okienku po lewej stronie kliknij bazy danych użytkowników.

4. W bloku bazy danych kliknij pozycję wszystkie ustawienia.

5. W bloku ustawienia kliknij części szyfrowania danych przezroczysty, aby otworzyć blok danych przezroczystego szyfrowania.

6. W bloku szyfrowania danych przycisk szyfrowania danych, aby przejść, a następnie kliknij przycisk Zapisz (w górnej części strony), aby zastosować ustawienia. Postęp przezroczystego szyfrowania danych zostanie zbliżone do stanu szyfrowania.

![Włączanie szyfrowania danych](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

Instrukcje dotyczące włączania funkcji TDE i odszyfrowywania chronionych funkcji TDE baz danych i innych informacji można znaleźć w artykule [przezroczystego szyfrowania danych z bazy danych SQL Azure.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)

## <a name="summary"></a>Podsumowanie

Firmy mogą wykonywać jego celem szyfrowania danych osobowych, przechowywane w chmurze Azure. Mogą one to robić przy użyciu szyfrowania dysków Azure, aby chronić całą woluminy. Może to obejmować zarówno pliki systemu operacyjnego, jak i plików danych, które zawierają dane osobowe i innych poufnych danych. Szyfrowanie usługi Magazyn Azure może służyć do ochrony danych osobowych, które są przechowywane w plikach i obiektów blob. W przypadku danych przechowywanych w bazach danych Azure SQL przezroczystego szyfrowania danych zapewnia ochronę przed nieautoryzowanym ujawnienia informacji osobistych.

Do ochrony kluczy, które są używane do szyfrowania danych na platformie Azure, firma może używać usługi Azure Key Vault. Usprawnia proces zarządzania kluczami i umożliwia firmie, aby zachować kontrolę nad kluczami, które dostępu i szyfrowanie danych osobistych.

## <a name="next-steps"></a>Następne kroki

- [Przewodnik rozwiązywania problemów szyfrowania dysków Azure](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-tsg)

- [Szyfrowanie maszyny wirtualnej platformy Azure](https://docs.microsoft.com/en-us/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [Szyfrowanie danych w usłudze Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)

- [Azure DB rozwiązania Cosmos szyfrowania bazy danych w stanie spoczynku](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)
