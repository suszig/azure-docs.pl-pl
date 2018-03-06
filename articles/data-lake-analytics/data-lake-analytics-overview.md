---
title: "Omówienie usługi Microsoft Azure Data Lake Analytics | Microsoft Docs"
description: "Data Lake Analytics jest usługą przetwarzania danych big data na platformie Azure, która ułatwia prowadzenie działalności dzięki wglądowi w dane w chmurze, bez względu na lokalizację tych danych ani ich rozmiar."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 1e1d443a-48a2-47fb-bc00-bf88274222de
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/23/2017
ms.author: saveenr
ms.openlocfilehash: d327a3c28e928550b361c07569df74060cfcac0d
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Omówienie usługi Microsoft Azure Data Lake Analytics
## <a name="what-is-azure-data-lake-analytics"></a>Co to jest usługa Azure Data Lake Analytics?
Usługa Azure Data Lake Analytics jest usługą zadań analizy na żądanie, która pozwala uprościć analizowanie danych big data. Zamiast wdrażać, konfigurować i dostosowywać sprzęt, możesz pisać zapytania umożliwiające przekształcanie danych i wyodrębniać wartościowe informacje. Usługa analizy może od razu rozpocząć obsługę zadań w dowolnej skali — należy wybrać ustawienie wskazujące potrzebną moc. Płacisz tylko za uruchomione zadanie, dzięki czemu oszczędzasz pieniądze. Ta usługa do analizowania danych obsługuje język U-SQL, który łączy korzyści języka SQL dzięki możliwościom kodu imperatywnego. Język U-SQL umożliwia analizowanie danych w usługach Data Lake Store, Azure SQL Database i Azure SQL Data Warehouse oraz w programie SQL Server na platformie Azure.

## <a name="key-capabilities"></a>Najważniejsze możliwości
* **Dynamiczne skalowanie**
  
    Usługa Data Lake Analytics została zaprojektowana z myślą o wydajności i skalowaniu w chmurze.  Umożliwia ona dynamiczną aprowizację zasobów oraz analizowanie terabajtów, a nawet eksabajtów danych. Po zakończeniu zadania zasoby są automatycznie wyłączane, więc płacisz tylko za użytą moc obliczeniową. Nawet w sytuacjach zwiększania lub zmniejszania rozmiaru przechowywanych danych lub ilości użytych zasobów obliczeniowych nie trzeba ponownie pisać kodu. Można skupić się tylko na logice biznesowej, a nie na sposobie przetwarzania i przechowywania dużych zestawów danych.
* **Szybsze programowanie, debugowanie i bardziej inteligentne optymalizowanie przy użyciu znanych narzędzi**
  
    Usługa Data Lake Analytics jest ściśle zintegrowana z programem Visual Studio, co pozwala uruchamiać, debugować i dostosowywać kod za pomocą znanych narzędzi. Wizualizacje zadań U-SQL umożliwiają sprawdzanie sposobu działania kodu w skali. Dzięki temu można łatwo identyfikować wąskie gardła związane z wydajnością i optymalizować koszty.
* **U-SQL: język prosty i znany, zaawansowany i rozszerzalny**
  
    Usługa Data Lake Analytics obejmuje język zapytań U-SQL, który stanowi rozszerzenie znanego, prostego, deklaratywnego charakteru języka SQL dzięki wszechstronnym możliwościom języka C#. Język U-SQL jest oparty na tym samym rozproszonym środowisku uruchomieniowym, które obsługuje systemy danych big data w firmie Microsoft. Miliony deweloperów języków SQL i .NET mogą teraz przetwarzać i analizować swoje dane, wykorzystując już posiadane umiejętności.
* **Bezproblemowa integracja z inwestycjami związanymi z infrastrukturą IT**
  
    Usługa Data Lake Analytics może korzystać z istniejących inwestycji w infrastrukturę IT w zakresie obsługi tożsamości, zarządzania, zabezpieczeń i magazynowania danych. To podejście ułatwia nadzór nad danymi i rozbudowę istniejących aplikacji obsługujących dane. Usługa Data Lake Analytics jest zintegrowana z usługą Active Directory, co umożliwia zarządzanie użytkownikami i udzielanie im uprawnień, oraz zawiera wbudowane funkcje monitorowania i inspekcji.

* **Przystępna cena i niedroga obsługa**
  
    Usługa Data Lake Analytics to ekonomiczne rozwiązanie służące do uruchamiania obciążeń związanych z danymi big data. Opłaty są naliczane za poszczególne zadania wykonywane podczas przetwarzania danych. Sprzęt, licencje ani umowy dotyczące pomocy technicznej w zakresie usługi nie są wymagane. System jest automatycznie skalowany w pionie i w poziomie w momencie rozpoczęcia i zakończenia zadania, więc nigdy nie płaci się więcej, niż trzeba. [Dowiedz się więcej na temat kontrolowania kosztów i oszczędzania pieniędzy](https://1drv.ms/f/s!AvdZLquGMt47h213Hg3rhl-Tym1c).
    
* **Współdziałanie z danymi na platformie Azure**
  
    Usługa Data Lake Analytics jest zoptymalizowana pod kątem pracy z usługą Azure Data Lake, co zapewnia najwyższy poziom wydajności, przepływności oraz przetwarzania równoległego na potrzeby obsługi obciążeń związanych z danymi big data.  Usługa Data Lake Analytics może również współdziałać z usługami Azure Blob Storage i Azure SQL Database.

## <a name="next-steps"></a>Następne kroki
 
  * Rozpoczynanie pracy z usługą Data Lake Analytics za pomocą [Azure Portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI](data-lake-analytics-get-started-cli2.md)
  * Zarządzanie usługą Azure Data Lake Analytics przy użyciu [Azure portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) | [Azure .NET SDK](data-lake-analytics-manage-use-dotnet-sdk.md) | [Node.js](data-lake-analytics-manage-use-nodejs.md)
  * [Jak kontrolować koszty i oszczędzać pieniądze przy użyciu usługi Data Lake Analytics](https://1drv.ms/f/s!AvdZLquGMt47h213Hg3rhl-Tym1c)
