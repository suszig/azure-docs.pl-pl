---
title: "Szyfrowanie usługi Magazyn Azure dla danych magazynowanych | Dokumentacja firmy Microsoft"
description: "Szyfrowanie usługi magazynu obiektów Blob Azure na stronie usługi, gdy dane są przechowywane przy użyciu funkcji szyfrowanie usługi Magazyn Azure, a go odszyfrować podczas pobierania danych."
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/06/2018
ms.author: lakasa
ms.openlocfilehash: 29f6a38f7a7f0f107dab8c99e750a8dec803f6f0
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Szyfrowanie usługi Azure Storage dla danych magazynowanych

Azure magazynu usługi szyfrowania (SSE) dla danych magazynowanych pomaga chronić i ochrony danych w celu spełnienia organizacji bezpieczeństwa i zgodności zobowiązań. W przypadku tej funkcji usługi Magazyn Azure automatycznie szyfruje dane przed trwałym go do magazynu Azure i odszyfrowuje je przed pobierania. Obsługa szyfrowania, szyfrowanie w rest, odszyfrowywania i zarządzania kluczami pochodzącymi SSE są niewidoczne dla użytkowników. Wszystkie dane zapisane w magazynie Azure jest szyfrowana przy użyciu 256-bitowego [szyfrowania AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednego bloku najwyższy szyfrów dostępne.

Nie można wyłączyć SSE, jest włączona dla wszystkich kont magazynu nowych i istniejących. Ponieważ domyślnie jest zabezpieczone dane, nie należy modyfikować z kodu lub aplikacji, aby móc korzystać z SSE.

 SSE automatycznie szyfruje dane w wszystkie warstwy wydajności (Standard i Premium), wszystkie modele wdrażania (usługi Azure Resource Manager i model klasyczny) i wszystkich usług Azure Storage (obiektu Blob, kolejki, tabel i plików). 

Klucze szyfrowania zarządzany przez firmę Microsoft można używać z SSE lub skorzystać z kluczy szyfrowania. Aby uzyskać więcej informacji o korzystaniu z własnych kluczy, zobacz [szyfrowanie usługi Magazyn przy użyciu klienta zarządzane klucze w usłudze Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Wyświetl ustawienia szyfrowania w portalu Azure

Aby wyświetlić ustawienia szyfrowanie usługi Magazyn, zaloguj się do [portalu Azure](https://portal.azure.com) i wybierz konto magazynu. Na **ustawienia** bloku, kliknij na ustawienie szyfrowania.

![Opcja szyfrowania portalu zrzut ekranu przedstawiający](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Często zadawane pytania dotyczące szyfrowanie usługi magazynu

**Pytanie: czy masz istniejące konto magazynu klasycznego. Można włączyć SSE na nim?**

A: SSE jest domyślnie włączona dla wszystkich kont magazynu (kont magazynu Classic i Resource Manager).

**Pytanie: jak szyfrować dane w moim koncie magazynu classic**

A. przy użyciu szyfrowania domyślnie włączona nowych danych jest automatycznie szyfrowane przez usługę magazynu. 

**Pytanie: czy masz istniejące konto magazynu Menedżera zasobów. Można włączyć SSE na nim?**

Odpowiedź: SSE jest włączona domyślnie na wszystkich istniejących kont magazynu Menedżera zasobów. Ta funkcja jest obsługiwana dla obiektów blob, tabel, pliku i kolejki magazynu. 

**Pytanie: Chcę szyfrowania bieżące dane w istniejącego konta magazynu usługi Resource Manager?**

Odp.: Z SSE domyślnie włączona dla wszystkich kont magazynu — Classic i Menedżer zasobów konta magazynu. Jednak dane, które były obecne nie będą szyfrowane. Aby zaszyfrować dane, można skopiować je do innej nazwy lub innego kontenera, a następnie usuń niezaszyfrowane wersji. 

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

A: szyfrowanie dysków azure jest używany do szyfrowania dysków systemu operacyjnego i danych na maszynach wirtualnych IaaS. Aby uzyskać więcej informacji, odwiedź stronę naszych [przewodnik zabezpieczeń magazynu](../storage-security-guide.md).

**Pytanie: co zrobić, jeśli włączyć szyfrowania dysków Azure na mój dysk danych?**

A: to będzie działać bezproblemowo. Dane będą szyfrowane za pomocą obu metod.

**Pytanie: moim koncie magazynu skonfigurowano powinny być replikowane geograficznie nadmiarowości. Z SSE nadmiarowych kopii także będą zaszyfrowane?**

A: tak, wszystkie kopie konta magazynu są szyfrowane, a wszystkie opcje nadmiarowości — lokalnie nadmiarowego magazynu (LRS), magazynu strefy nadmiarowy (ZRS), Magazyn geograficznie nadmiarowy (GRS) i Magazyn geograficznie nadmiarowy dostęp do odczytu (RA-GRS) — są obsługiwane.

**Pytanie: I nie można wyłączyć szyfrowania na moim koncie magazynu.**

Odpowiedź: szyfrowanie jest włączone domyślnie i nie można wyłączyć szyfrowania dla konta magazynu. 

**Pytanie: jest SSE dozwolone tylko w określonych regionach?**

Odpowiedź: SSE jest dostępna we wszystkich regionach dla wszystkich usług. 

**Pytanie: jak można skontaktować się z osobą czy wystąpienia jakichkolwiek problemów lub chcesz wyrazić swoją opinię?**

Odpowiedź: kontaktu [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) wszelkie problemy związane z magazynu usługi szyfrowania.

## <a name="next-steps"></a>Następne kroki
Magazyn Azure oferuje rozbudowany zestaw funkcji zabezpieczeń, które razem umożliwiają deweloperom tworzenie bezpiecznych aplikacji. Aby uzyskać więcej informacji, odwiedź stronę [przewodnik zabezpieczeń magazynu](../storage-security-guide.md).
