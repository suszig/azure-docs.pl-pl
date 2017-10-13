Rozliczenie jest przeprowadzane na podstawie wykorzystania usługi Azure Storage w oparciu o Twoje konto magazynu. Koszty przechowywania są uzależnione od następujących czynników: regionu/lokalizacji, typu konta, pojemności magazynu, schematu replikacji, transakcji magazynu i wyjścia danych.

* Region odnosi się do regionu geograficznego konta.
* Typ konta dotyczy tego, czy używane jest konto magazynu ogólnego przeznaczenia czy konto usługi Blob Storage. W przypadku konta usługi Blob Storage warstwa dostępu decyduje również o modelu rozliczenia konta.
* Pojemność usługi Storage dotyczy stopnia wykorzystania przydziału konta magazynu do przechowywania danych.
* Replikacja określa liczbę kopii danych, które są obsługiwane jednocześnie, oraz ich lokalizacje.
* Transakcje dotyczą wszystkich operacji odczytu i zapisu w usłudze Azure Storage.
* Wyjście danych dotyczy danych przekazywanych poza region świadczenia usługi Azure. Jeśli do danych na koncie magazynu uzyskuje się dostęp za pośrednictwem aplikacji, która nie działa w tym samym regionie, naliczane są opłaty za wyjście danych. (W przypadku usług Azure możesz wykonać czynności związane z grupowaniem danych i usług w tych samych centrach danych, aby zredukować lub całkowicie wyeliminować opłaty za wyjście danych).

Strona [Cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/) zawiera szczegółowe informacje o cenach w zależności od typu konta, pojemności magazynu, replikacji i transakcji. Artykuł [Szczegóły cennika transferów danych](https://azure.microsoft.com/pricing/details/data-transfers/) zawiera szczegółowe informacje o cenach za wyjście danych. Artykuł [Kalkulator cen usługi Azure Storage](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) zawiera informacje, które ułatwią szacowanie kosztów.

