---
title: "Dzienników serwera w bazie danych Azure PostgreSQL | Dokumentacja firmy Microsoft"
description: "Generuje dziennikami błędów i kwerendy w bazie danych Azure dla PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 696af85cd5609171a719a7e77efbfcdeba0aaaaa
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>Dzienników serwera w bazie danych Azure PostgreSQL 
Bazy danych platformy Azure dla PostgreSQL generuje zapytań i błąd dzienników. Jednak dostęp do dzienników transakcji nie jest obsługiwany. Dzienniki zapytania i błąd może służyć do identyfikowania, rozwiązywania, nieoptymalne wydajności i błędów konfiguracji. Aby uzyskać więcej informacji, zobacz [raportowanie błędów i rejestrowanie](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Dzienniki dostępu serwera
Można wyświetlić listę i Pobierz dzienniki błędów serwera Azure PostgreSQL przy użyciu portalu Azure [interfejsu wiersza polecenia Azure](howto-configure-server-logs-using-cli.md)i interfejsów API REST Azure.

## <a name="log-retention"></a>Przechowywanie dziennika
Można ustawić okresu przechowywania dla dzienniki systemu za pomocą **dziennika\_przechowywania\_okres** parametru skojarzonego z serwerem. Jednostki dla tego parametru jest kilka dni. Wartość domyślna to 3 dni. Wartość maksymalna wynosi 7 dni. Serwer musi mieć wystarczającej ilości przydzielone magazynu zawiera pliki dziennika zachowanych.
Obróć plik dziennika co godzinę lub 100 MB rozmiar, zależnie od zostanie osiągnięty jako pierwszy.

## <a name="configure-logging-for-azure-postgresql-server"></a>Konfigurowanie rejestrowania dla serwera Azure PostgreSQL
Dla serwera, można włączyć rejestrowanie zapytań i rejestrowania błędów. Dzienniki błędów mogą zawierać informacje automatycznego próżni, połączenia i punkty kontrolne.

Rejestrowanie zapytań można włączyć dla swojego wystąpienia bazy danych PostgreSQL, ustawiając dwa parametry serwera: `log\_statement` i `log\_min\_duration\_statement`.

**Dziennika\_instrukcji** parametr określa instrukcji SQL, które są rejestrowane. Firma Microsoft zaleca ustawienie tego parametru ***wszystkie*** do rejestrowania wszystkich instrukcji; wartość domyślna to none.

**Dziennika\_min\_czas trwania\_instrukcji** parametr ustawia limit (w milisekundach) oświadczenia mają być rejestrowane. Wszystkie instrukcje SQL działające dłużej niż ustawienie parametru są rejestrowane. Ten parametr jest wyłączona i domyślnie ustawiany na minus 1 (-1). Włączenie tego parametru mogą być pomocne w śledzeniu zoptymalizowanego zapytania w aplikacji.

**Dziennika\_min\_wiadomości** zezwala na określanie komunikatu poziomy są zapisywane w dzienniku serwera. Wartość domyślna to ostrzeżenie. 

Aby uzyskać więcej informacji na temat tych ustawień, zobacz [raportowanie błędów i rejestrowanie](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) dokumentacji. Do konfiguracji bazy danych Azure szczególnie PostgreSQL parametry serwera, zobacz [dostosować parametry konfiguracji serwera przy użyciu interfejsu wiersza polecenia Azure](howto-configure-server-parameters-using-cli.md).

## <a name="next-steps"></a>Następne kroki
- Aby uzyskać dostęp do dzienników przy użyciu interfejsu wiersza polecenia interfejsu wiersza polecenia Azure, zobacz [Konfigurowanie i dostępu do dzienników serwera przy użyciu interfejsu wiersza polecenia Azure](howto-configure-server-logs-using-cli.md).
- Aby uzyskać więcej informacji na parametry serwera, zobacz [dostosować parametry konfiguracji serwera przy użyciu interfejsu wiersza polecenia Azure](howto-configure-server-parameters-using-cli.md).
