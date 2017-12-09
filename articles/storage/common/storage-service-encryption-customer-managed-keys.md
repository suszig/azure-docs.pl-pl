---
title: "Szyfrowanie usługi Magazyn Azure przy użyciu klienta zarządzane klucze w usłudze Azure Key Vault | Dokumentacja firmy Microsoft"
description: "Szyfrowanie usługi magazynu obiektów Blob Azure na stronie usługi, gdy dane są przechowywane przy użyciu funkcji szyfrowanie usługi Magazyn Azure, a go odszyfrować podczas pobierania danych przy użyciu klienta zarządzanych kluczy."
services: storage
documentationcenter: .net
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: lakasa
ms.openlocfilehash: 0a05a0d28899cc3db11f8fda8aec5bd6ed9bd5f8
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Szyfrowanie usługi Magazyn przy użyciu klienta zarządzane klucze w usłudze Azure Key Vault

Microsoft Azure jest zaangażowana ochrony i ochrony danych w celu spełnienia organizacji bezpieczeństwa i zgodności zobowiązań.  Jednym ze sposobów może chronić dane w stanie spoczynku jest magazynu usługi szyfrowania (SSE), które automatycznie szyfruje dane podczas zapisywania go do magazynu i odszyfrowuje dane podczas pobierania go używać. Szyfrowanie i odszyfrowywanie jest automatyczne, całkowicie niewidoczne i używa 256-bitowego [szyfrowania AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednego bloku najwyższy szyfrów dostępne.

Klucze szyfrowania zarządzany przez firmę Microsoft można używać z SSE lub skorzystać z kluczy szyfrowania. W tym artykule omówiono te ostatnie. Aby uzyskać więcej informacji o użyciu kluczy zarządzany przez firmę Microsoft lub o SSE ogólnie, zobacz [szyfrowanie usługi Magazyn danych magazynowanych](../storage-service-encryption.md).

Aby zapewnić możliwość korzystania z kluczy szyfrowania, SSE dla magazynu obiektów Blob jest zintegrowana z magazynu kluczy Azure (AKV). Można utworzyć kluczy szyfrowania i przechowywać je w AKV lub interfejsów API firmy AKV służy do generowania kluczy szyfrowania. Nie tylko czy AKV pozwala na zarządzanie i sterowanie klucze, umożliwia również inspekcji użycie klucza. 

Dlaczego czy chcesz utworzyć własne klucze? Udostępnia bardziej elastyczne, umożliwiając tworzenie, obracanie, wyłącz i zdefiniuj kontroli dostępu. Umożliwia on również przeprowadzać inspekcję kluczy szyfrowania używany do ochrony danych.

## <a name="sse-with-customer-managed-keys-preview"></a>SSE z customer preview zarządzanych kluczy

Ta funkcja jest obecnie dostępna w wersji zapoznawczej. Aby użyć tej funkcji, musisz utworzyć nowe konto magazynu. Możesz utworzyć nowego magazynu kluczy oraz klucz lub użyć istniejącego magazynu kluczy oraz klucz. Konto magazynu i magazynu kluczy musi być w tym samym regionie, ale można je w ramach różnych subskrypcji.

Aby wziąć udział w wersji zapoznawczej, skontaktuj się z [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com). Firma Microsoft udostępni specjalne łącze do udziału w wersji zapoznawczej.

Aby dowiedzieć się więcej, zapoznaj się [— często zadawane pytania](#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).

> [!IMPORTANT]
> Możesz zarejestrować się, aby podgląd przed wykonać kroki opisane w tym artykule. Bez dostępu do wersji zapoznawczej nie będzie można włączyć tę funkcję w portalu.

## <a name="getting-started"></a>Wprowadzenie
## <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Krok 1: [Utwórz nowe konto magazynu](../storage-create-storage-account.md)

## <a name="step-2-enable-encryption"></a>Krok 2: Włączanie szyfrowania
SSE można włączyć dla konta magazynu za pomocą [portalu Azure](https://portal.azure.com). W bloku ustawień dla konta magazynu Wyszukaj sekcję usługi obiektów Blob, jak pokazano na poniższej ilustracji, a następnie kliknij przycisk szyfrowania.

![Opcja szyfrowania portalu zrzut ekranu przedstawiający](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)
<br/>*Włącz SSE dla usługi Blob*

Jeśli chcesz, aby programowo włączyć lub wyłączyć szyfrowanie usługi Magazyn na koncie magazynu, możesz użyć [interfejsu API REST dostawcy zasobów magazynu Azure](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN), [biblioteki klienta dostawcy zasobów magazynu dla platformy .NET](https://docs.microsoft.com/dotnet/api/?redirectedfrom=MSDN), [programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.0.0), lub [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/azure/storage/storage-azure-cli).

Na tym ekranie Jeśli nie jest widoczne pole wyboru "Użyj własnego klucza", możesz nie zostały zatwierdzone skorzystania z wersji zapoznawczej. Wyślij wiadomość e-mail do [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) i zażądać zatwierdzenia.

![Portal zrzut ekranu przedstawiający podgląd szyfrowania](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

Domyślnie SSE używa kluczy zarządzany przez firmę Microsoft. Aby korzystać z własnych kluczy, zaznacz pole wyboru. Następnie można określić klucz identyfikatora URI, lub wybierz klucz i Key Vault z próbnika.

## <a name="step-3-select-your-key"></a>Krok 3: Wybierz klucz

![Szyfrowanie zrzut ekranu przedstawiający Portal opcja własnego klucza](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

![Portalu zrzut ekranu przedstawiający szyfrowania z wprowadź identyfikator uri klucza opcji](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Jeśli konto magazynu nie ma dostępu do magazynu kluczy, można uruchomić następujące polecenie, aby udzielić dostępu do konta magazynu do magazynu kluczy wymagane przy użyciu programu Azure Powershell.

![Portal zrzut ekranu przedstawiający odmowa dostępu do magazynu kluczy](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Można również przyznać dostęp za pośrednictwem portalu Azure, przechodząc do usługi Azure Key Vault w portalu Azure i udzielanie dostępu do konta magazynu.

## <a name="step-4-copy-data-to-storage-account"></a>Krok 4: Kopiowanie danych do konta magazynu
Jeśli chcesz przenieść dane do nowego konta magazynu, dzięki czemu jest on zaszyfrowany, zapoznaj się [krok 3 z wprowadzenie w szyfrowanie usługi Magazyn danych magazynowanych](https://docs.microsoft.com/azure/storage/storage-service-encryption#step-3-copy-data-to-storage-account).

## <a name="step-5-query-the-status-of-the-encrypted-data"></a>Krok 5: Zapytać o stan zaszyfrowanych danych
Aby zbadać stan zaszyfrowanych danych, zobacz [kroku 4 z wprowadzenie w szyfrowanie usługi Magazyn danych magazynowanych](https://docs.microsoft.com/azure/storage/storage-service-encryption#step-4-query-the-status-of-the-encrypted-data).

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Często zadawane pytania dotyczące szyfrowanie usługi Magazyn danych w stanie spoczynku
**Pytanie: czy używany Magazyn w warstwie Premium; z kluczami zarządzanego klienta można używać SSE?**

A: tak, SSE z zarządzanego przez firmę Microsoft i klientów zarządzanych kluczy jest obsługiwany w standardowych magazynu i Magazyn w warstwie Premium. 

**Pytanie: czy można utworzyć nowego konta magazynu z SSE z kluczami klientów zarządzanych włączyć za pomocą programu Azure PowerShell i interfejsu wiersza polecenia Azure?**

Odpowiedź: tak.

**Pytanie: jak dużo więcej pojawia się koszt usługi Azure Storage SSE klientowi zarządzanych kluczy jest włączone?**

Odpowiedź: Brak koszt skojarzony przy użyciu usługi Azure Key Vault. Więcej informacji można znaleźć [klucza magazynu cennik](https://azure.microsoft.com/en-us/pricing/details/key-vault/). Nie ma żadnych dodatkowych kosztów stosowania SSE.

**Pytanie: czy czy można odwołać dostęp do kluczy szyfrowania?**

Odpowiedź: tak, w dowolnym momencie można odwołać dostęp. Istnieje kilka sposobów, aby odwołać dostęp do kluczy. Zapoznaj się [PowerShell magazynu kluczy Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.0.0) i [interfejsu wiersza polecenia Azure Key Vault](https://docs.microsoft.com/cli/azure/keyvault) więcej szczegółów. Odbieranie prawa dostępu zablokuje skutecznie dostęp do wszystkich obiektów blob na koncie magazynu jako konto klucz szyfrowania jest niedostępne dla magazynu Azure.

**Pytanie: czy można utworzyć konta magazynu i klucza w innym regionie?**

A: nie, konto magazynu i klucza/magazynu kluczy muszą znajdować się w tym samym regionie. 

**Pytanie: czy można włączyć SSE z klientów zarządzanych kluczy podczas tworzenia konta magazynu?**

Odpowiedź: nie. Po włączeniu SSE podczas tworzenia konta magazynu, można używać tylko klucze zarządzany przez firmę Microsoft. Jeśli chcesz używać kluczy klienta zarządzane, należy zaktualizować właściwości konta magazynu. Użyj REST lub jednej z bibliotek klienckich magazynu, aby programowo zaktualizować konta magazynu lub aktualizowanie właściwości konta magazynu przy użyciu portalu Azure po utworzeniu konta.

**Pytanie: czy szyfrowanie można wyłączyć podczas z klienta przy użyciu SSE zarządzane klucze?**

A: nie, nie można wyłączyć szyfrowania, gdy z klienta przy użyciu SSE zarządzanych kluczy. Aby wyłączyć szyfrowanie, musisz przełączyć się przy użyciu kluczy zarządzany przez firmę Microsoft. Można to zrobić za pomocą portalu Azure lub programu PowerShell.

**Pytanie: jest SSE domyślnie włączona, tworząc nowe konto magazynu?**

Odpowiedź: SSE nie jest włączona domyślnie; Aby je włączyć, można użyć portalu Azure. Można również programowo włączyć tę funkcję za pomocą interfejsu API REST dostawcy zasobów magazynu. 

**Pytanie: nie można włączyć szyfrowanie na moim koncie magazynu.**

A: to konto magazynu Resource Manager? Klasycznych kont magazynu nie są obsługiwane. SSE z kluczami klientów zarządzanych także można włączyć tylko na nowo utworzonych kont magazynu Menedżera zasobów.

**Pytanie: SSE jest klientowi zarządzane klucze dozwolone tylko w określonych regionach?**

Odpowiedź: SSE jest dostępny w tylko niektóre regiony dla magazynu obiektów Blob dla tej wersji zapoznawczej. Wiadomości e-mail [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) do sprawdzenia dostępności i szczegółowe informacje o wersji zapoznawczej. 

**Pytanie: jak można skontaktować się z osobą czy wystąpienia jakichkolwiek problemów lub chcesz wyrazić swoją opinię?**

Odpowiedź: kontaktu [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) wszelkie problemy związane z magazynu usługi szyfrowania. 

## <a name="next-steps"></a>Następne kroki

*   Aby uzyskać więcej informacji na rozbudowany zestaw zabezpieczeń możliwości, które ułatwiają deweloperom tworzenie bezpiecznych aplikacji, zobacz [przewodnik zabezpieczeń magazynu](https://docs.microsoft.com/azure/storage/storage-security-guide).
*   Aby uzyskać przegląd informacji dotyczących usługi Azure Key Vault, zobacz [co to jest usługa Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
*   W ramach wprowadzenia usługi Azure Key Vault, zobacz [wprowadzenie do korzystania z usługi Azure Key Vault](../../key-vault/key-vault-get-started.md).
