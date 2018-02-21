---
title: "Szyfrowanie usługi Magazyn Azure dla danych magazynowanych | Dokumentacja firmy Microsoft"
description: "Szyfrowanie usługi magazynu obiektów Blob Azure na stronie usługi, gdy dane są przechowywane przy użyciu funkcji szyfrowanie usługi Magazyn Azure, a go odszyfrować podczas pobierania danych."
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: tamram
ms.openlocfilehash: d04752c92218d17b4e90bb03a2ed2ac5a0a11b93
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2018
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Szyfrowanie usługi Azure Storage dla danych magazynowanych
Azure magazynu usługi szyfrowania (SSE) dla danych magazynowanych pomaga chronić i ochrony danych w celu spełnienia organizacji bezpieczeństwa i zgodności zobowiązań. Przy użyciu tej funkcji usługa Azure Storage automatycznie szyfruje dane przed trwałym wprowadzeniem ich do magazynu i odszyfrowuje je przed pobraniem. Szyfrowanie, odszyfrowywanie i zarządzanie kluczami jest całkowicie przezroczyste dla użytkowników.

Poniższe sekcje zawierają napotka szczegółowe wskazówki dotyczące sposobu używania funkcji szyfrowanie usługi Magazyn, a także obsługiwane scenariusze i użytkownika.

## <a name="overview"></a>Przegląd
Magazyn Azure oferuje rozbudowany zestaw funkcji zabezpieczeń, które razem umożliwiają deweloperom tworzenie bezpiecznych aplikacji. Dane mogą być chronione przy użyciu przesyłanych między aplikacją a Azure [szyfrowania po stronie klienta](../storage-client-side-encryption.md), HTTPs lub SMB 3.0. Szyfrowanie usługi Magazyn zapewnia szyfrowanie magazynowanych, obsługa szyfrowania, odszyfrowywania i zarządzania kluczami w sposób całkowicie przezroczysty. Wszystkie dane są szyfrowane przy użyciu 256-bitowego [szyfrowania AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednego bloku najwyższy szyfrów dostępne.

SSE polega na szyfrowaniu danych podczas ich zapisywania do magazynu Azure i może służyć do obiektów Blob platformy Azure, plików, tabel i magazyn kolejek. Następujące działania:

* Standard Storage: Konta magazynu ogólnego przeznaczenia obiekty BLOB, Magazyn plików, tabel i kolejek i kont magazynu obiektów Blob
* Premium Storage 
* Domyślnie włączona dla kont Classic i magazynu ARM 
* Nadmiarowość wszystkie poziomy (LRS, ZRS, GRS i RA-GRS)
* Konta magazynu Azure Resource Manager (ale nie klasycznego) 
* Wszystkie regiony.

Aby dowiedzieć się więcej, zapoznaj się często zadawane pytania.

Aby wyświetlić ustawienia szyfrowanie usługi Magazyn dla konta magazynu, zaloguj się do [portalu Azure](https://portal.azure.com) i wybierz konto magazynu. W bloku ustawień Wyszukaj sekcji usługi obiektów Blob, jak pokazano w tym zrzucie ekranu pokazano, a następnie kliknij przycisk szyfrowania.

![Opcja szyfrowania portalu zrzut ekranu przedstawiający](./media/storage-service-encryption/image1.png)
<br/>*Rysunek 1: Włącz SSE dla usługi obiektów Blob (krok 1.)*

![Opcja szyfrowania portalu zrzut ekranu przedstawiający](./media/storage-service-encryption/image3.png)
<br/>*Rysunek 2: Włącz SSE dla usługi plików (krok 1.)*

Kliknij przycisk Ustawienia szyfrowania, można włączyć lub wyłączyć szyfrowanie usługi magazynu.

![Właściwości szyfrowania portalu zrzut ekranu przedstawiający](./media/storage-service-encryption/image2.png)
<br/>*Rysunek 3: Włącz SSE dla obiekt Blob i plików usługi (Step2)*

## <a name="encryption-scenarios"></a>Scenariusze szyfrowania
Szyfrowanie usługi Magazyn jest włączona na poziomie konta magazynu. Jest włączona dla wszystkich usług domyślnie. Obsługuje następujących scenariuszy:

* Szyfrowanie obiektów Blob, plików, tabeli i magazynu kolejek zarówno klasycznym, a Menedżer zasobów kont.

SSE ma następujące ograniczenia:

* Istniejące dane - SSE szyfruje dane nowo utworzony po włączeniu szyfrowania. Jeśli na przykład można utworzyć nowe konto magazynu Resource Manager, ale nigdy nie włączaj szyfrowania, a następnie przekazać do tego konta magazynu obiektów blob lub zarchiwizowane wirtualne dyski twarde, a następnie włącz SSE, te obiekty BLOB nie będą szyfrowane, chyba że są one ulegną lub kopiowane.
* Obsługa Marketplace — Włącz szyfrowanie maszyny wirtualne utworzone z witryny Marketplace przy użyciu [portalu Azure](https://portal.azure.com), programu PowerShell i interfejsu wiersza polecenia Azure. Obraz podstawowy dysk VHD pozostanie niezaszyfrowane; Jednak wszelkie zapisy wykonać po maszyna wirtualna ma przejścia będą szyfrowane.

## <a name="getting-started"></a>Wprowadzenie
### <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Krok 1: [Utwórz nowe konto magazynu](../storage-create-storage-account.md).
### <a name="step-2-verify-encryption"></a>Krok 2: Sprawdź szyfrowania.
Można sprawdzić za pomocą szyfrowania [portalu Azure](https://portal.azure.com).

> [!NOTE]
> Jeśli chcesz programowo zweryfikować szyfrowanie usługi Magazyn na koncie magazynu, możesz użyć [interfejsu API REST dostawcy zasobów magazynu Azure](https://msdn.microsoft.com/library/azure/mt163683.aspx), [biblioteki klienta dostawcy zasobów magazynu dla platformy .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx) , [Programu azure PowerShell](/powershell/azureps-cmdlets-docs), lub [Azure CLI](../storage-azure-cli.md).
> 
> 

### <a name="step-3-copy-data-to-storage-account"></a>Krok 3: Kopiowanie danych do konta magazynu
Po włączeniu SSE dla konta magazynu będą szyfrowane wszystkie dane zapisane do tego konta magazynu. Wszystkie dane, które już znajdują się na tym koncie magazynu nie będą szyfrowane, dopóki ich napisany od nowa. Możesz skopiować dane z jednego kontenera do innego, aby upewnić się, że poprzednie dane są zaszyfrowane. W tym celu, można użyć dowolnego z poniższych narzędzi. To samo dla plików, tabel i magazyn kolejek oraz.

#### <a name="using-azcopy"></a>Przy użyciu narzędzia AzCopy
Narzędzie AzCopy to narzędzie wiersza polecenia systemu Windows przeznaczone do kopiowania danych z magazynu obiektów Blob Microsoft Azure, plików i tabeli przy użyciu prostych poleceń z optymalną wydajnością. Służy to do kopiowania obiektów blob lub plików z jedno konto magazynu do kolejnego, który ma włączoną SSE. 

Aby dowiedzieć się więcej, odwiedź [Transfer danych za pomocą wiersza polecenia Azcopy](storage-use-azcopy.md).

#### <a name="using-smb"></a>Za pomocą protokołu SMB
Usługa pliki Azure oferuje udziały plików w chmurze przy użyciu standardowego protokołu SMB. Można zainstalować udział plików z klienta lokalnie lub na platformie Azure. Po zainstalowaniu, narzędzi, takich jak Robocopy można kopiować pliki do udziałów plików na platformę Azure. Aby uzyskać więcej informacji, zobacz [sposób instalacji udziału plików Azure w systemie Windows](../files/storage-how-to-use-files-windows.md) i [sposób instalacji udziału plików Azure w systemie Linux](../files/storage-how-to-use-files-linux.md).


#### <a name="using-the-storage-client-libraries"></a>Za pomocą biblioteki klienta magazynu
Można skopiować obiektu blob lub pliku danych do i z magazynu obiektów blob lub między kontami magazynu przy użyciu naszego bogaty zestaw biblioteki klienta magazynu w tym .NET, C++, Java, Android, Node.js, PHP, Python i Ruby.

Aby dowiedzieć się więcej, odwiedź naszych [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure przy użyciu platformy .NET](../blobs/storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Za pomocą Eksploratora magazynu
Aby utworzyć konta magazynu, przekazywanie i pobieranie danych, wyświetlanie zawartości obiektów blob i przeglądanie katalogów, można użyć Eksploratora magazynu. Jeden z nich umożliwia przekazywanie obiekty BLOB na koncie magazynu z włączone szyfrowanie. Z niektórych eksploratory usługi storage można także skopiować dane z istniejącego magazynu obiektów blob do innego kontenera konta magazynu lub nowe konto magazynu, który ma włączoną SSE.

Aby dowiedzieć się więcej, odwiedź [eksploratory usługi Storage Azure](../storage-explorers.md).

### <a name="step-4-query-the-status-of-the-encrypted-data"></a>Krok 4: Zapytać o stan zaszyfrowanych danych
Wdrożono zaktualizowanej wersji biblioteki klienta magazynu, który służy do sprawdzania stanu obiektu w celu określenia, czy jest on zaszyfrowany lub nie. To jest obecnie dostępny tylko dla magazynu obiektów Blob. Obsługa magazynu plików znajduje się na plan. 

W międzyczasie możesz wywołać [Get właściwości konta](https://msdn.microsoft.com/library/azure/mt163553.aspx) Sprawdź, czy konto magazynu ma włączone szyfrowanie lub wyświetlić właściwości konta magazynu w portalu Azure.

## <a name="encryption-and-decryption-workflow"></a>Szyfrowanie i odszyfrowywanie przepływu pracy
Poniżej przedstawiono krótki opis przepływu pracy, szyfrowania i odszyfrowywania:

* Szyfrowanie jest włączone dla konta magazynu.
* Gdy klient zapisuje nowe dane (PUT Blob PUT bloku PUT strony, umieść itp. plik) do magazynu obiektów Blob lub pliku; każdego zapisu jest szyfrowana przy użyciu 256-bitowego [szyfrowania AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednego bloku najwyższy szyfrów dostępne.
* Klient musi mieć dostęp do danych (Pobierz obiekt Blob itp.), przed powrotem do użytkownika jest automatycznie odszyfrowania danych.
* Jeśli szyfrowanie jest wyłączone, nowe zapisy nie są szyfrowane i zaszyfrowane dane pozostaną zaszyfrowane, dopóki nie ulegną przez użytkownika. Gdy włączone jest szyfrowanie, będą szyfrowane zapisów do magazynu obiektów Blob lub pliku. Stan danych nie zmienia się z użytkownikiem przełączania się między włączenie/wyłączenie szyfrowania dla konta magazynu.
* Wszystkie klucze szyfrowania są przechowywane, zaszyfrowana i zarządzany przez firmę Microsoft.

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Często zadawane pytania dotyczące szyfrowanie usługi Magazyn danych w stanie spoczynku
**Pytanie: czy masz istniejące konto magazynu klasycznego. Można włączyć SSE na nim?**

A: SSE jest domyślnie włączona dla wszystkich kont magazynu (kont magazynu Classic i Resource Manager).

**Pytanie: jak szyfrować dane w moim koncie magazynu classic**

A. przy użyciu szyfrowania domyślnie włączona nowych danych jest automatycznie szyfrowane przez usługę magazynu. 

Można również utworzyć nowe konto magazynu Resource Manager i skopiuj wszystkie dane przy użyciu [AzCopy](storage-use-azcopy.md) z istniejącego konta magazynu klasycznego do nowo utworzonego konta magazynu usługi Resource Manager. 

Można również przeprowadzić migrację konta magazynu klasycznego do konta magazynu usługi Resource Manager. Ta operacja jest natychmiastowe, zmienia typ konta, ale nie ma wpływu na istniejących danych. Żadnych nowych danych zostanie zaszyfrowana tylko po włączeniu szyfrowania. Aby uzyskać więcej informacji, zobacz [platformy obsługiwana migracja z zasobów IaaS ze środowiska klasycznego do Menedżera zasobów](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/). Należy pamiętać, że jest to obsługiwane tylko dla usług obiektów Blob i pliku.

**Pytanie: czy masz istniejące konto magazynu Menedżera zasobów. Można włączyć SSE na nim?**

Odpowiedź: SSE jest włączona domyślnie na wszystkich istniejących kont magazynu Menedżera zasobów. Ta funkcja jest obsługiwana dla obiektów blob, tabel, pliku i kolejki magazynu. 

**Pytanie: Chcę szyfrowania bieżące dane w istniejącego konta magazynu usługi Resource Manager?**

Odp.: Z SSE domyślnie włączona dla wszystkich kont magazynu — Classic i Menedżer zasobów konta magazynu. Jednak dane, które były obecne nie będą szyfrowane. Aby zaszyfrować dane, można skopiować je do innej nazwy lub innego kontenera, a następnie usuń niezaszyfrowane wersji. 

**Pytanie: czy używany Magazyn w warstwie Premium; można użyć SSE?**

A: tak SSE jest obsługiwany w standardowych magazynu i Magazyn w warstwie Premium.  Magazyn w warstwie Premium nie jest obsługiwana dla usługi plików.

**Pyt.: Jeśli I utworzyć nowe konto magazynu z SSE włączone, należy utworzyć nową maszynę Wirtualną za pomocą, że konto magazynu jest średnia Moja maszyna wirtualna jest szyfrowany?**

Odpowiedź: tak. Żadnych dysków utworzone nowe konto magazynu, będą szyfrowane, pod warunkiem, będą one tworzone po włączeniu SSE. Jeśli maszyna wirtualna została utworzona przy użyciu platformę handlową platformy Azure, podstawowy obraz wirtualnego dysku twardego pozostanie niezaszyfrowane; Jednak wszelkie zapisy wykonać po maszyna wirtualna ma przejścia będą szyfrowane.

**Pytanie: czy można utworzyć nowego konta magazynu z SSE włączyć za pomocą programu Azure PowerShell i interfejsu wiersza polecenia Azure?**

Odpowiedź: SSE jest domyślnie włączona w czasie tworzenia dowolne konto magazynu (konta klasycznym oraz usługi Resource Manager). Możesz sprawdzić, używając programu Azure PowerShell i interfejsu wiersza polecenia Azure właściwości konta.

**Pytanie: jak bardziej usługi Azure Storage koszt włączenie SSE?**

Odpowiedź: nie istnieje bez dodatkowych kosztów.

**Pytanie: zarządzająca klucze szyfrowania?**

A: klucze są zarządzane przez firmę Microsoft.

**Pytanie: czy można użyć własnych kluczy szyfrowania?**

Odpowiedź: pracujemy nad zapewnia możliwości dla klientów do ich własnych kluczy szyfrowania.

**Pytanie: czy czy można odwołać dostęp do kluczy szyfrowania?**

Odpowiedź: nie w tej chwili; klucze są w pełni zarządzany przez firmę Microsoft.

**Pytanie: jest SSE domyślnie włączona, tworząc nowe konto magazynu?**

Odpowiedź: tak SSE przy użyciu kluczy Managed firmy Microsoft jest domyślnie włączona dla wszystkich kont magazynu — usługi Azure Resource Manager i kont magazynu Classic. Jest on włączony dla wszystkich usług również — obiektu Blob, tabeli, kolejki i magazyn plików.

**Pytanie: jak różni się od szyfrowania dysków Azure?**

Odpowiedź: Ta funkcja służy do szyfrowania danych w magazynie obiektów Blob platformy Azure. Szyfrowanie dysków Azure jest używany do szyfrowania dysków systemu operacyjnego i danych na maszynach wirtualnych IaaS. Aby uzyskać więcej informacji, odwiedź stronę naszych [przewodnik zabezpieczeń magazynu](../storage-security-guide.md).

**Pytanie: co zrobić, jeśli włączono SSE, a następnie można włączyć szyfrowania dysków Azure na dyskach?**

A: to będzie działać bezproblemowo. Dane będą szyfrowane za pomocą obu metod.

**Pytanie: moim koncie magazynu skonfigurowano powinny być replikowane geograficznie nadmiarowości. Jeśli włączono SSE, nadmiarowych kopii także będą zaszyfrowane?**

A: tak, wszystkie kopie konta magazynu są szyfrowane, a wszystkie opcje nadmiarowości — lokalnie nadmiarowego magazynu (LRS), magazynu strefy nadmiarowy (ZRS), Magazyn geograficznie nadmiarowy (GRS) i Magazyn geograficznie nadmiarowy dostęp do odczytu (RA-GRS) — są obsługiwane.

**Pytanie: I nie można wyłączyć szyfrowania na moim koncie magazynu.**

Odpowiedź: szyfrowanie jest włączone domyślnie i nie można wyłączyć szyfrowania dla konta magazynu. 

**Pytanie: jest SSE dozwolone tylko w określonych regionach?**

Odpowiedź: SSE jest dostępna we wszystkich regionach dla wszystkich usług. 

**Pytanie: jak można skontaktować się z osobą czy wystąpienia jakichkolwiek problemów lub chcesz wyrazić swoją opinię?**

Odpowiedź: kontaktu [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) wszelkie problemy związane z magazynu usługi szyfrowania.

## <a name="next-steps"></a>Następne kroki
Magazyn Azure oferuje rozbudowany zestaw funkcji zabezpieczeń, które razem umożliwiają deweloperom tworzenie bezpiecznych aplikacji. Aby uzyskać więcej informacji, odwiedź stronę [przewodnik zabezpieczeń magazynu](../storage-security-guide.md).
