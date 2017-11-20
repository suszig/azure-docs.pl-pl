---
title: "Usługa Azure SQL Data Warehouse — automatyzowanie poziomów obliczeniowych przy użyciu usługi Azure Functions | Microsoft Docs"
description: "Jak zarządzać zasobami obliczeniowymi magazynu danych przy użyciu usługi Azure Functions."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A901
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 11/06/2017
ms.author: elbutter
ms.openlocfilehash: 56543946c2a2cfe63feb249fcd4ac63517700115
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="use-azure-functions-to-automate-sql-dw-compute-levels"></a>Automatyzowanie poziomów obliczeniowych usługi SQL DW przy użyciu usługi Azure Functions

W tym samouczku przedstawiono, jak zarządzać poziomami obliczeniowymi usługi Azure SQL Data Warehouse przy użyciu usługi Azure Functions. Zaleca się używanie tych architektur z usługą SQL Data Warehouse [zoptymalizowaną pod kątem elastyczności][Performance Tiers].

Aby móc używać aplikacji funkcji platformy Azure z usługą SQL Data Warehouse, należy utworzyć [konto nazwy głównej usługi](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal) z dostępem współautora w ramach tej samej subskrypcji, w której znajduje się wystąpienie magazynu danych. 

## <a name="deploy-timer-based-scaler-with-an-azure-resource-manager-template"></a>Wdrażanie opartego na czasomierzu programu ScaleR przy użyciu szablonu usługi Azure Resource Manager

Aby wdrożyć szablon, potrzebne będą następujące informacje:

- Nazwa grupy zasobów, w której znajduje się wystąpienie usługi SQL DW
- Nazwa serwera logicznego, na którym znajduje się wystąpienie usługi SQL DW
- Nazwa wystąpienia usługi SQL DW
- Identyfikator dzierżawy (identyfikator katalogu ) usługi Azure Active Directory
- Identyfikator subskrypcji 
- Identyfikator aplikacji nazwy głównej usługi
- Klucz tajny usługi nazwy głównej usługi

Gdy uzyskasz powyższe informacje, wdróż następujący szablon:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

Po wdrożeniu szablonu powinny być dostępne trzy nowe zasoby: bezpłatny plan usługi Azure App Service, plan usługi aplikacji funkcji oparty na użyciu oraz konto magazynu, które będzie obsługiwać rejestrowanie i kolejkę operacji. Kontynuuj czytanie pozostałych sekcji, aby dowiedzieć się, jak zmodyfikować wdrożone funkcje i dostosować je do swoich potrzeb.

### <a name="change-the-scale-up-or-scale-down-compute-level"></a>Zmienianie skalowania w górę lub w dół poziomu obliczeniowego

1. Przejdź do usługi aplikacji funkcji. Jeśli szablon został wdrożony przy użyciu wartości domyślnych, usługa ta powinna mieć nazwę *DWOperations*. Po otwarciu aplikacji funkcji powinno być widocznych pięć funkcji wdrożonych w usłudze aplikacji funkcji. 

   ![Funkcje wdrażane przy użyciu szablonu](media/manage-compute-with-azure-functions/five-functions.png)

2. Wybierz pozycję *DWScaleDownTrigger* lub *DWScaleUpTrigger* w zależności od tego, czy chcesz zmienić czas skalowania w górę, czy czas skalowania w dół. W menu rozwijanym wybierz pozycję Integruj.

   ![Wybieranie pozycji Integruj dla funkcji](media/manage-compute-with-azure-functions/select-integrate.png)

3. Obecnie powinna być wyświetlana wartość *%ScaleDownTime%* lub *%ScaleUpTime%*. Te wartości wskazują, że harmonogram jest oparty na wartościach określonych w [ustawieniach aplikacji][Application Settings]. Na razie możesz to zignorować i zmienić harmonogram na preferowaną godzinę, wykonując następne kroki.

4. W obszarze harmonogramu dodaj godzinę, o której wyrażenie CRON ma odzwierciedlać, jak często usługa SQL Data Warehouse ma być skalowana w górę. 

  ![Zmienianie harmonogramu funkcji](media/manage-compute-with-azure-functions/change-schedule.png)

  Wartość `schedule` jest [wyrażeniem CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) obejmującym sześć pól: 
  ```json
  {second} {minute} {hour} {day} {month} {day-of-week}
  ```

  Na przykład *„0 30 9 * * 1-5”* spowoduje odzwierciedlanie wyzwalacza w każdy dzień roboczy o godzinie 9:30. Aby uzyskać więcej informacji, zapoznaj się z [przykładami harmonogramów][schedule examples] usługi Azure Functions.


### <a name="change-the-scale-up-or-scale-down-time"></a>Zmienianie czasu skalowania w górę lub w dół

1. Przejdź do usługi aplikacji funkcji. Jeśli szablon został wdrożony przy użyciu wartości domyślnych, usługa ta powinna mieć nazwę *DWOperations*. Po otwarciu aplikacji funkcji powinno być widocznych pięć funkcji wdrożonych w usłudze aplikacji funkcji. 

2. Wybierz pozycję *DWScaleDownTrigger* lub *DWScaleUpTrigger* w zależności od tego, czy chcesz zmienić wartość obliczeniową skalowania w górę, czy wartość obliczeniową skalowania w dół. Po wybraniu funkcji w okienku powinien zostać wyświetlony plik *index.js*.

   ![Zmienianie poziomu obliczeniowego wyzwalacza funkcji](media/manage-compute-with-azure-functions/index-js.png)

3. Zmień wartość elementu *ServiceLevelObjective* na odpowiedni poziom i wybierz pozycję Zapisz. To jest poziom obliczeniowy, na podstawie którego wystąpienie magazynu danych będzie skalowane według harmonogramu określonego w sekcji Integracja.

### <a name="use-pause-or-resume-instead-of-scale"></a>Używanie wstrzymywania lub wznawiania zamiast skalowania 

Obecnie funkcje włączone domyślnie to *DWScaleDownTrigger* i *DWScaleUpTrigger*. Jeśli zamiast tego chcesz korzystać z funkcji wstrzymywania i wznawiania, możesz włączyć funkcje *DWPauseTrigger* lub *DWResumeTrigger*.

1. Przejdź do okienka Funkcje.

   ![Okienko Funkcje](media/manage-compute-with-azure-functions/functions-pane.png)



2. Kliknij przełącznik obok wyzwalaczy, które chcesz włączyć.

3. Przejdź do kart *Integracja* odpowiednich wyzwalaczy, aby zmienić ich harmonogram.

   [!NOTE]: The functional difference between the scaling triggers and the pause/resume triggers is the message that is sent to the queue. See [Add a new trigger function][Add a new trigger function] for more information.



### <a name="add-a-new-trigger-function"></a>Dodawanie nowej funkcji wyzwalacza

Obecnie szablon zawiera tylko dwie funkcje skalowania. To oznacza, że w ciągu dnia możliwe jest tylko jednokrotne skalowanie w dół i jednokrotne skalowanie w górę. W celu uzyskania bardziej szczegółowej kontroli, na przykład możliwości skalowania w dół wielokrotnie w ciągu dnia lub określenia innego zachowania skalowania w weekendy, konieczne będzie dodanie kolejnego wyzwalacza.

1. Utwórz nową pustą funkcję. Wybierz przycisk *+* obok pozycji Funkcje, aby wyświetlić okienko szablonu funkcji.

   ![Tworzenie nowej funkcji](media/manage-compute-with-azure-functions/create-new-function.png)

2. W polu Język wybierz pozycję *JavaScript*, a następnie wybierz pozycję *TimerTrigger*.

   ![Tworzenie nowej funkcji](media/manage-compute-with-azure-functions/timertrigger-js.png)

3. Nazwij funkcję i ustaw harmonogram. Ilustracja przedstawia, jak można ustawić wyzwalanie funkcji w każdą sobotę o północy (późny wieczór w piątek).

   ![Skalowanie w dół w sobotę](media/manage-compute-with-azure-functions/scale-down-saturday.png)

4. Skopiuj zawartość pliku *index.js* z jednej z funkcji wyzwalacza.

   ![Kopiowanie pliku index.js](media/manage-compute-with-azure-functions/index-js.png)

5. Ustaw odpowiednie zachowanie zmiennej operacji w następujący sposób:

   ```javascript
   // Resume the data warehouse instance
   var operation = {
       "operationType": "ResumeDw"
   }

   // Pause the data warehouse instance
   var operation = {
       "operationType": "PauseDw"
   }

   // Scale the data warehouse instance to DW600
   var operation = {
       "operationType": "ScaleDw",
       "ServiceLevelObjective": "DW600"
   }
   ```


### <a name="complex-scheduling"></a>Sporządzanie złożonego harmonogramu

W tej sekcji przedstawiono pokrótce, co jest potrzebne do uzyskania bardziej złożonych możliwości wstrzymywania, wznawiania i skalowania.

#### <a name="example-1"></a>Przykład 1:

Codzienne skalowanie w górę o godz. 8:00 do wartości DW600 i skalowanie w dół o godz. 20:00 do wartości DW200.

| Funkcja  | Harmonogram     | Operacja                                |
| :-------- | :----------- | :--------------------------------------- |
| Function1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW600"}` |
| Function2 | 0 0 20 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

#### <a name="example-2"></a>Przykład 2: 

Codzienne skalowanie w górę o godz. 8:00 do wartości DW1000, jednokrotne skalowanie w dół do wartości DW600 o godz. 16:00 i skalowanie w dół o godz. 22:00 do wartości DW200.

| Funkcja  | Harmonogram     | Operacja                                |
| :-------- | :----------- | :--------------------------------------- |
| Function1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW1000"}` |
| Function2 | 0 0 16 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| Function3 | 0 0 22 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

#### <a name="example-3"></a>Przykład 3: 

Skalowanie w górę o godz. 8:00 do wartości DW1000, jednokrotne skalowanie w dół do wartości DW600 o godz. 16:00 w dni robocze. Wstrzymanie w piątek o godz. 23:00, wznowienie w poniedziałek rano o godz. 7:00.

| Funkcja  | Harmonogram       | Operacja                                |
| :-------- | :------------- | :--------------------------------------- |
| Function1 | 0 0 8 * * 1-5  | `var operation = {"operationType": "ScaleDw",    "ServiceLevelObjective": "DW1000"}` |
| Function2 | 0 0 16 * * 1-5 | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| Function3 | 0 0 23 * * 5   | `var operation = {"operationType": "PauseDw"}` |
| Function4 | 0 0 7 * * 0    | `var operation = {"operationType": "ResumeDw"}` |



## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o funkcjach platformy Azure z [wyzwalaczem czasomierza](../azure-functions/functions-create-scheduled-function.md).

Zapoznaj się z [repozytorium przykładów](https://github.com/Microsoft/sql-data-warehouse-samples) usługi SQL Data Warehouse.



[schedule examples]: ../azure-functions/functions-bindings-timer.md#schedule-examples

[Application Settings]: ../azure-functions/functions-how-to-use-azure-function-app-settings.md
[Add a new trigger function]: manage-compute-with-azure-functions.md#add-a-new-trigger-function

[Performance Tiers]: performance-tiers.md
