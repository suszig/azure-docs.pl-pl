---
title: Omówienie usługi Microsoft Azure Data Lake Analytics | Microsoft Docs
description: 'Data Lake Analytics jest usługą przetwarzania danych big data na platformie Azure, która ułatwia prowadzenie działalności dzięki wglądowi w dane w chmurze, bez względu na lokalizację tych danych oraz ich rozmiar. Usługa Data Lake Analytics zapewnia te możliwości w najprostszy, najbardziej skalowalny i najbardziej ekonomiczny sposób. '
services: data-lake-analytics
documentationcenter: ''
author: edmacauley
manager: paulettm
editor: cgronlun

ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: edmaca

---
# Omówienie usługi Microsoft Azure Data Lake Analytics
## Co to jest usługa Azure Data Lake Analytics?
Azure Data Lake Analytics to nowa usługa zaprojektowana z myślą o upraszczaniu analizy danych big data. Ta usługa pozwala skoncentrować się pisaniu i uruchamianiu zadań oraz zarządzaniu nimi, a nie na działaniu rozproszonej infrastruktury. Zamiast wdrażać, konfigurować i dostosowywać sprzęt, możesz pisać zapytania umożliwiające przekształcanie danych i wyodrębniać wartościowe informacje. Usługa analizy może od razu rozpocząć obsługę zadań w dowolnej skali — wystarczy wybrać ustawienie wskazujące potrzebną moc. Płacisz tylko za uruchomione zadanie, dzięki czemu oszczędzasz pieniądze. Usługa analizy obsługuje usługę Azure Active Directory, dzięki czemu możesz w prosty sposób zarządzać dostępem i rolami zintegrowanymi z lokalnym systemem tożsamości. Obejmuje ona również język U-SQL, który łączy korzyści z użycia języka SQL z wszechstronnymi możliwościami kodu użytkownika. Skalowalne rozproszone środowisko uruchomieniowe języka U SQL umożliwia efektywne analizowanie danych w magazynie i na serwerach SQL Server na platformie Azure oraz w usługach Azure SQL Database i Azure SQL Data Warehouse.

## Najważniejsze możliwości
* **Dynamiczne skalowanie** 
  
    Usługa Data Lake Analytics została zaprojektowana od podstaw z myślą o wydajności i skalowaniu w chmurze.  Umożliwia ona dynamiczną aprowizację zasobów oraz analizowanie terabajtów, a nawet eksabajtów danych. Po zakończeniu zadania zasoby są automatycznie wyłączane, więc płacisz tylko za użytą moc obliczeniową. Nawet w sytuacjach zwiększania lub zmniejszania rozmiaru przechowywanych danych lub ilości użytych zasobów obliczeniowych nie trzeba ponownie pisać kodu. Dzięki temu można skupić się tylko na logice biznesowej, a nie na sposobie przetwarzania i przechowywania dużych zestawów danych. 
* **Szybsze programowanie i bardziej inteligentne debugowanie i optymalizowanie przy użyciu znanych narzędzi**
  
    Usługa Data Lake Analytics jest ściśle zintegrowana z programem Visual Studio, co pozwala uruchamiać, debugować i dostosowywać kod za pomocą znanych narzędzi. Wizualizacje zadań U-SQL umożliwiają sprawdzanie sposobu działania kodu w skali. Dzięki temu można łatwo identyfikować wąskie gardła związane z wydajnością i optymalizować koszty. 
* **U-SQL: język prosty i znany, zaawansowany i rozszerzalny**
  
    Usługa Data Lake Analytics obejmuje język zapytań U-SQL, który stanowi rozszerzenie znanego, prostego, deklaratywnego charakteru języka SQL dzięki wszechstronnym możliwościom języka C#. Język U-SQL jest oparty na tym samym rozproszonym środowisku uruchomieniowym, które obsługuje systemy danych big data w firmie Microsoft. Miliony deweloperów języków SQL i .NET mogą teraz przetwarzać i analizować wszystkie swoje dane, wykorzystując już posiadane umiejętności.
* **Bezproblemowa integracja z inwestycjami związanymi z infrastrukturą IT**
  
    Usługa Data Lake Analytics może korzystać z istniejących inwestycji w infrastrukturę IT w zakresie obsługi tożsamości, zarządzania, zabezpieczeń i magazynowania danych. Ułatwia to nadzór nad danymi i rozbudowę istniejących aplikacji obsługujących dane. Usługa Data Lake Analytics jest zintegrowana z usługą Active Directory, co umożliwia zarządzanie użytkownikami i udzielanie im uprawnień, oraz oferuje wbudowane funkcje monitorowania i inspekcji.
* **Przystępna cena i niedroga obsługa**
  
    Usługa Data Lake Analytics to ekonomiczne rozwiązanie służące do uruchamiania obciążeń związanych z danymi big data. Opłaty są naliczane za poszczególne zadania wykonywane podczas przetwarzania danych. Sprzęt, licencje ani umowy dotyczące pomocy technicznej w zakresie usługi nie są wymagane. System jest automatycznie skalowany w pionie i w poziomie w momencie rozpoczęcia i zakończenia zadania, co oznacza, że nigdy płacisz więcej niż trzeba. 
* **Współdziałanie z danymi na platformie Azure**
  
    Usługa Azure Data Lake Analytics może współpracować z wieloma źródłami danych platformy Azure, takimi jak usługi Azure Blob Storage i Azure SQL Database. Oczywiście usługa Data Lake Analytics została również specjalnie zoptymalizowana pod kątem pracy z usługą Azure Data Lake Store i zapewnia najwyższy poziom wydajności, przepływności oraz przetwarzania równoległego na potrzeby obsługi obciążeń związanych z danymi big data.

## Zobacz też
* Rozpoczynanie pracy
  
  * [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-get-started-portal.md)
  * [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu programu Azure PowerShell](data-lake-analytics-get-started-powershell.md)
  * [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu zestawu Azure SDK programu .NET](data-lake-analytics-get-started-net-sdk.md)
  * [Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
  * [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md)
* Język U-SQL i programowanie
  
  * [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md)
  * [Korzystanie z funkcji okna języka U-SQL na potrzeby zadań usługi Azure Data Lake Analytics](data-lake-analytics-use-window-functions.md)
  * [Opracowywanie operatorów języka U-SQL definiowanych przez użytkownika na potrzeby zadań usługi Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md)
* Zarządzanie
  
  * [Zarządzanie usługą Azure Data Lake Analytics przy użyciu witrynie Azure Portal](data-lake-analytics-manage-use-portal.md)
  * [Zarządzanie usługą Azure Data Lake Analytics przy użyciu programu Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
  * [Monitorowanie zadań usługi Azure Data Lake Analytics i rozwiązywanie problemów przy użyciu witryny Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
  * [Accessing Diagnostics logs for Azure Data Lake Analytics (Dostęp do dzienników diagnostycznych usługi Azure Data Lake Analytics)](data-lake-analytics-diagnostic-logs.md)
* Kompleksowy samouczek
  
  * [Korzystanie z interaktywnych samouczków usługi Azure Data Lake Analytics](data-lake-analytics-use-interactive-tutorials.md)
  * [Analizowanie dzienników witryn sieci Web przy użyciu usługi Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)
* Przekaż swoje opinie
  
  * [Dodaj komentarz dotyczący listy planowanej dokumentacji](data-lake-analytics-documentation-backlog.md)
  * [Prześlij zgłoszenie dotyczące funkcji](http://aka.ms/adlafeedback)
  * [Uzyskaj pomoc na forach](http://aka.ms/adlaforums)

<!--HONumber=sep16_HO1-->


