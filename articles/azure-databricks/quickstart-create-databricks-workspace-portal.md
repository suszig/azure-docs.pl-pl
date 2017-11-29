---
title: "Szybki Start: Uruchom zadanie Spark Databricks Azure przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Procedury szybkiego startu przedstawia sposób użycia portalu Azure do tworzenia obszaru roboczego Azure Databricks klastra Apache Spark i uruchom zadanie Spark."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: nitinme
ms.openlocfilehash: cffc78c06de6be252704c2a116d7ded2c7e8567f
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-portal"></a>Szybki Start: Uruchom zadanie Spark Azure Databricks przy użyciu portalu Azure

Ta opcja szybkiego startu przedstawia sposób tworzenia obszaru roboczego Azure Databricks i klastra Apache Spark w tym obszarze roboczym. Na koniec zostanie przedstawiony sposób Uruchom zadanie Spark dla klastra Databricks. Aby uzyskać więcej informacji na Azure Databricks, zobacz [co to jest Azure Databricks?](what-is-azure-databricks.md)

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do [portalu Azure](https://portal.azure.com).

## <a name="create-a-databricks-workspace"></a>Tworzenie obszaru roboczego Databricks

W tej sekcji utworzysz obszarem roboczym Azure Databricks przy użyciu portalu Azure. 

1. W portalu Azure kliknij  **+** , kliknij przycisk **dane i analiza**, a następnie kliknij przycisk **Databricks Azure (wersja zapoznawcza)**. 

    ![Databricks w portalu Azure](./media/quickstart-create-databricks-workspace-portal/azure-databricks-on-portal.png "Databricks w portalu Azure")

2. W obszarze **Databricks Azure (wersja zapoznawcza)**, kliknij przycisk **Utwórz**.

    > [!NOTE]
    > Azure Databricks jest obecnie ograniczone w wersji zapoznawczej. Jeśli chcesz subskrypcji platformy Azure to zostać uwzględnione podczas listę dozwolonych podobnej skorzystania z wersji zapoznawczej, musisz wypełnić [wypełnieniu formularza](https://databricks.azurewebsites.net/).

2. W obszarze **usługę Azure Databricks**, podaj następujące wartości:

    ![Utwórz obszar roboczy Azure Databricks](./media/quickstart-create-databricks-workspace-portal/create-databricks-workspace.png "Utwórz obszar roboczy Azure Databricks")

    * Aby uzyskać **nazwa obszaru roboczego**, podaj nazwę obszaru roboczego Databricks.
    * Aby uzyskać **subskrypcji**, z listy rozwijanej wybierz subskrypcję platformy Azure.
    * Aby uzyskać **grupy zasobów**, określ, czy chcesz utworzyć nową grupę zasobów lub użyć istniejącego. Grupa zasobów jest kontenerem, który zawiera powiązane zasoby Azure rozwiązania. Aby uzyskać więcej informacji, zobacz [grupy zasobów platformy Azure — omówienie](../azure-resource-manager/resource-group-overview.md).
    * Aby uzyskać **lokalizacji**, wybierz pozycję **wschodnie stany USA 2**. Inne dostępne regiony, zobacz [usług platformy Azure dostępna w regionie](https://azure.microsoft.com/regions/services/).

3. Kliknij przycisk **Utwórz**.

## <a name="create-a-spark-cluster-in-databricks"></a>Tworzenie klastra Spark w Databricks

1. W portalu Azure, przejdź do obszaru roboczego Databricks, który został utworzony, a następnie kliknij przycisk **zainicjować obszaru roboczego**.

2. Nastąpi przekierowanie do portalu Azure Databricks. Z portalu, kliknij przycisk **klastra**.

    ![Databricks na platformie Azure](./media/quickstart-create-databricks-workspace-portal/databricks-on-azure.png "Databricks na platformie Azure")

3. W **nowy klaster** Podaj wartości, aby utworzyć klaster.

    ![Tworzenie klastra Databricks Spark na platformie Azure](./media/quickstart-create-databricks-workspace-portal/create-databricks-spark-cluster.png "klastra Databricks Spark utworzyć na platformie Azure")

    * Wprowadź nazwę dla klastra.
    * Upewnij się, że wybrano **przerwania po ___ minut aktywności** wyboru. Podaj czas trwania (w minutach) zakończenie klastra, jeśli nie jest używany klaster.
    * Zaakceptuj wszystkie pozostałe wartości domyślne. 
    * Kliknij przycisk **Utwórz klaster**. Po uruchomieniu klastra, możesz dołączyć notesów do klastra i uruchamianie zadań Spark.

Aby uzyskać więcej informacji na temat tworzenia klastrów, zobacz [Tworzenie klastra Spark w usłudze Azure Databricks](https://docs.azuredatabricks.net/user-guide/clusters/create.html).

## <a name="run-a-spark-sql-job"></a>Uruchom zadanie Spark SQL

Przed rozpoczęciem tej sekcji, należy wykonać następujące czynności:

* [Tworzenie konta magazynu Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account). 
* Pobierz przykładowy plik JSON [z usługi Github](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json). 
* Przekaż przykładowy plik JSON do utworzone konto magazynu platformy Azure. Można użyć [Eksploratora usługi Microsoft Azure Storage](../vs-azure-tools-storage-manage-with-storage-explorer.md) przekazywania plików.

Wykonaj poniższe kroki, aby tworzenie notesu w Databricks, skonfiguruj notesu można odczytać danych z konta magazynu obiektów Blob platformy Azure, a następnie uruchom zadanie Spark SQL w danych.

1. W okienku po lewej stronie kliknij **obszaru roboczego**. Z **obszaru roboczego** listy rozwijanej kliknij **Utwórz**, a następnie kliknij przycisk **notesu**.

    ![Tworzenie notesu w Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-create-notebook.png "tworzenie notesu w Databricks")

2. W **tworzenie notesu** oknie dialogowym wpisz nazwę, wybierz **Scala** jako język i wybierz utworzony wcześniej klastra Spark.

    ![Tworzenie notesu w Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-details.png "tworzenie notesu w Databricks")

    Kliknij przycisk **Utwórz**.

3. W poniższy fragment kodu, Zastąp `{YOUR STORAGE ACCOUNT NAME}` nazwą konta magazynu platformy Azure został utworzony i `{YOUR STORAGE ACCOUNT ACCESS KEY}` kluczem dostępu konta magazynu. Wklej fragment w pustej komórce w notesie, a następnie naciśnij klawisz SHIFT + ENTER, aby uruchomić komórki kodu. Ta Wstawka kodu konfiguruje notesu można odczytać danych z magazynu obiektów blob platformy Azure.

       spark.conf.set("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net", "{YOUR STORAGE ACCOUNT ACCESS KEY}")
    
    Aby uzyskać instrukcje dotyczące pobierania klucz konta magazynu, zobacz [zarządzanie kluczami dostępu do magazynu](../storage/common/storage-create-storage-account.md#manage-your-storage-account)

    > [!NOTE]
    > Można również używać usługi Azure Data Lake Store z klastra Spark w usłudze Azure Databricks. Aby uzyskać instrukcje, zobacz [Użyj Data Lake Store z Azure Databricks](https://go.microsoft.com/fwlink/?linkid=864084).

4. Uruchom instrukcję SQL, można utworzyć tabeli tymczasowej przy użyciu danych z przykładowy plik danych JSON, **small_radio_json.json**. W poniższy fragment kodu Zastąp symbole zastępcze swoją nazwę kontenera i nazwy konta magazynu. Wklej fragment w komórce kodu w notesie, a następnie naciśnij klawisz SHIFT + ENTER. We fragmencie `path` wskazuje lokalizację przykładowy plik JSON, który został przekazany do konta magazynu Azure.

    ```sql
    %sql 
    CREATE TEMPORARY TABLE radio_sample_data
    USING json
    OPTIONS (
     path "wasbs://{YOUR CONTAINER NAME}@{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net/small_radio_json.json"
    )
    ```

    Po pomyślnym zakończeniu działania polecenia masz wszystkich danych z pliku JSON jako tabeli Databricks klastra.

    `%sql` Magic polecenia języka umożliwia uruchamianie kodu SQL z notesu, nawet jeśli notesu jest innego typu. Aby uzyskać więcej informacji, zobacz [łączenie języków w notesie](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook).

5. Przyjrzyjmy się migawkę przykładowych danych JSON, aby lepiej zrozumieć zapytanie, do którego jest przeprowadzana. Wklej poniższy fragment kodu w komórce kod i naciśnij klawisze **SHIFT + ENTER**.

    ```sql
    %sql 
    SELECT * from radio_sample_data
    ```

6. Zobaczysz tabelaryczne dane wyjściowe, jak pokazano na poniższym zrzucie ekranu (wyświetlane są tylko niektóre kolumny):

    ![Przykładowe dane JSON](./media/quickstart-create-databricks-workspace-portal/databricks-sample-csv-data.png "JSON przykładowych danych")

    Oprócz innych szczegółów przykładowych danych przechwytuje płeć odbiorców kanał radiowych (nazwa kolumny **płci**) oraz czy swoją subskrypcję, jest wolne lub płatną (nazwa kolumny **poziom**).

7. Możesz teraz utworzyć wizualną reprezentację tych danych do pokazu dla każdej płci, ile użytkownicy mają konta bezpłatne i ile są kupowane subskrybentów. W dolnej części tabelaryczne dane wyjściowe, kliknij przycisk **wykres słupkowy** ikonę, a następnie kliknij przycisk **wykreślenia opcje**.

    ![Utwórz wykres słupkowy](./media/quickstart-create-databricks-workspace-portal/create-plots-databricks-notebook.png "utworzyć wykres słupkowy")

8. W **dostosować wykreślenia**, przeciągnij i upuść wartości, jak pokazano na zrzucie ekranu.

    ![Dostosować wykres słupkowy](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-customize-plot.png "dostosować wykres słupkowy")

    * Ustaw **klucze** do **płci**.
    * Ustaw **grupowania serii** do **poziom**.
    * Ustaw **wartości** do **poziom**.
    * Ustaw **agregacji** do **liczba**.

    Kliknij przycisk **Zastosuj**.

9. Dane wyjściowe zawierają wizualnej reprezentacji, jak pokazano na poniższym zrzucie ekranu:

     ![Dostosować wykres słupkowy](./media/quickstart-create-databricks-workspace-portal/databricks-sql-query-output-bar-chart.png "dostosować wykres słupkowy")

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Podczas tworzenia klastra Spark, jeśli zaznaczono pole wyboru **przerwania po ___ minut aktywności**, klaster zostanie automatycznie przerywane, jeśli było nieaktywne przez czas określony.

Jeśli nie zostały wybrane pole wyboru, musisz ręcznie przerwanie klastra. Aby to zrobić, w obszarze roboczym Azure Databricks, w lewym okienku kliknij **klastrów**. Dla klastra, aby zakończyć, przesuń kursor nad wielokropka w obszarze **akcje** kolumny, a następnie kliknij przycisk **przerwania** ikony.

![Klaster Databricks przerwania](./media/quickstart-create-databricks-workspace-portal/terminate-databricks-cluster.png "Databricks przerwanie klastra")

## <a name="next-steps"></a>Następne kroki

W tym artykule utworzyć klaster Spark w usłudze Azure Databricks i uruchomiono zadanie Spark, przy użyciu danych w magazynie Azure. Można również sprawdzić [Spark źródeł danych](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html) informacje na temat importowania danych z innych źródeł danych do Azure Databricks. Przejdź do następnego artykuł, aby dowiedzieć się, jak używać usługi Azure Data Lake Store z Azure Databricks.

> [!div class="nextstepaction"]
>[Użyj Data Lake Store z Azure Databricks](https://go.microsoft.com/fwlink/?linkid=864084)