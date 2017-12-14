---
title: "Microsoft Genomics: Często zadawane pytania | Dokumentacja firmy Microsoft"
titleSuffix: Azure
description: "Odpowiedzi na typowe pytania klientów poproś o Genomics firmy Microsoft."
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 12/07/2017
ms.openlocfilehash: 2077eeb5177b07c458476ae900f81b72e35f0dc3
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="microsoft-genomics-common-questions"></a>Genomics firmy Microsoft: Często zadawane pytania

W tym artykule wymieniono najczęściej używanych zapytania, które mogą mieć dotyczą Genomics firmy Microsoft. Aby uzyskać więcej informacji w usłudze Microsoft Genomics, zobacz [co to jest Microsoft Genomics?](overview-what-is-genomics.md) 


## <a name="what-is-the-sla-for-microsoft-genomics"></a>Co to jest umowy SLA dla Genomics firmy Microsoft?
Firma Microsoft gwarantuje, że 99,9% czasu usługi Microsoft Genomics będą dostępne do odbierania żądań interfejsu API przepływu pracy. Aby uzyskać więcej informacji, zobacz [SLA](https://azure.microsoft.com/support/legal/sla/genomics/v1_0/).

## <a name="how-does-the-usage-of-microsoft-genomics-show-up-on-my-bill"></a>Jak jest użycie Genomics Microsoft wyświetlany na mojego rachunku?
Na podstawie liczby przetwarzanych na przepływie pracy gigabases rachunków Genomics firmy Microsoft. Aby uzyskać więcej informacji, zobacz [cennik](https://azure.microsoft.com/pricing/details/genomics/).


## <a name="where-can-i-find-a-list-of-all-possible-commands-and-arguments-for-the-msgen-client"></a>Gdzie mogę znaleźć listę wszystkich możliwych polecenia i argumentów `msgen` klienta?
Pełną listę dostępnych poleceń i argumenty można uzyskać, uruchamiając `msgen help`. Jeśli nie inne argumenty są dostarczane, zawiera listę dostępnych pomocy sekcje, po jednej dla każdego z `submit`, `list`, `cancel`, i `status`. Aby uzyskać pomoc dotyczącą danego polecenia, wpisz `msgen help command`, na przykład `msgen help submit` zawiera listę wszystkich opcji przesyłania danych.

## <a name="what-are-the-most-commonly-used-commands-for-the-msgen-client"></a>Co to są najczęściej używane polecenia dla `msgen` klienta?
Argumenty są najczęściej używanych poleceń `msgen` klienta obejmują: 

 |**Polecenie**          |  **Opis pola** |
 |:--------------------|:-------------         |
 |`list`               |Zwraca listę zadań, które zostały przesłane. Dla argumentów, zobacz `msgen help list`.  |
 |`submit`             |Przesyła żądanie przepływu pracy z usługą. Dla argumentów, zobacz `msgen help submit`.|
 |`status`             |Zwraca informacje o stanie przepływu pracy, określony przez `--workflow-id`. Zobacz też `msgen help status`. |
 |`cancel`             |Wysyła żądanie anulowania przetwarzania przepływu pracy, określony przez `--workflow-id`. Zobacz też `msgen help cancel`. |

## <a name="where-do-i-get-the-value-for---api-url-base"></a>Gdzie uzyskać wartość `--api-url-base`?
Przejdź do portalu Azure i Otwórz stronę genomika konta. W obszarze **zarządzania** nagłówek, wybierz **klucze dostępu**. Możesz znaleźć zarówno adres URL interfejsu API i kluczy dostępu.

## <a name="where-do-i-get-the-value-for---access-key"></a>Gdzie uzyskać wartość `--access-key`?
Przejdź do portalu Azure i Otwórz stronę genomika konta. W obszarze **zarządzania** nagłówek, wybierz **klucze dostępu**. Możesz znaleźć zarówno adres URL interfejsu API i kluczy dostępu.

## <a name="why-do-i-need-two-access-keys"></a>Dlaczego muszę dwa klucze dostępu?
W przypadku, gdy chcesz zaktualizować (regenerate), należy się dwa klucze dostępu do ich bez przerywania użycia usługi. Na przykład chcesz zaktualizować pierwszego klucza. W takim przypadku można rozpocząć wszystkich nowych przepływów pracy za pomocą drugiego klucza. Poczekaj aż do chwili zakończenia są już uruchomione przepływy pracy przy użyciu klucza pierwszy. Następnie zaktualizować klucza.

## <a name="do-you-save-my-storage-account-keys"></a>Czy można zapisać Moje klucze konta magazynu?
Klucz konta magazynu jest używany do tworzenia krótkoterminowych tokenów dostępu do usługi Microsoft Genomics pliki danych wejściowych do odczytu i zapisu plików wyjściowych. Domyślny czas trwania tokenu wynosi 48 godzin. Czas trwania tokenu można zmienić z `-sas/--sas-duration` opcji polecenia Prześlij; wartość jest w godzinach.

## <a name="what-genome-references-can-i-use"></a>Jakie genomu odwołuje się do czego służy?

Odwołania te są obsługiwane:
 |Dokumentacja              | Wartość`-pa/--process-args` |
 |:-------------         |:-------------                 |
 |b37                    | `R=b37m1`                     |
 |hg38                   | `R=hg38m1`                    |      
 |hg38 (nie analiza alt) | `R=hg38m1x`                   |  
 |hg19                   | `R=hg19m1`                    |    

## <a name="how-do-i-format-my-command-line-arguments-as-a-config-file"></a>Jak formatować Moje argumenty wiersza polecenia w pliku konfiguracji? 

msgen rozumie pliki konfiguracyjne w następującym formacie:
* Wszystkie opcje służą jako pary klucz wartość z wartościami z kluczy oddzielone dwukropkiem.
Spacja jest ignorowana.
* Wiersze, rozpoczynając od `#` są ignorowane.
* Wszystkich argumentów wiersza polecenia w formacie długim może zostać przekonwertowany z kluczem przez usuwanie jego początku kresek i zastępowania kresek między wyrazami znakami podkreślenia. Oto kilka przykładów konwersji:

 |Argument wiersza polecenia            | Wiersz w pliku konfiguracji |
 |:-------------                   |:-------------                 |
 |`-u/--api-url-base https://url`  | *api_url_base:https://URL*    |
 |`-k/--access-key KEY`            | *access_key:key*              |      
 |`-pa/--process-args R=B37m1`     | *process_args:R-b37m1*        |  

## <a name="next-steps"></a>Następne kroki

Aby zacząć korzystać z Genomics firmy Microsoft, należy korzystać z następujących zasobów:
- Rozpoczynanie pracy przez uruchomienie pierwszego przepływu pracy za pośrednictwem usługi Microsoft Genomics. [Uruchamianie przepływu pracy za pośrednictwem usługi Microsoft Genomics](quickstart-run-genomics-workflow-portal.md)
- Przesyłanie danych do przetwarzania przez usługę Microsoft Genomics: [sparować FASTQ](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [wiele FASTQ lub BAM](quickstart-input-multiple.md) 

