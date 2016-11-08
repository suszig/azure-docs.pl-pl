---
title: Tworzenie biletu pomocy technicznej dotyczącej usługi SQL Data Warehouse | Microsoft Docs
description: Tworzenie biletu pomocy technicznej w usłudze Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: sonyam
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 09/01/2016
ms.author: sonyama;barbkess

---
# Tworzenie biletu pomocy technicznej dotyczącej usługi SQL Data Warehouse
Jeśli masz problemy z usługą SQL Data Warehouse, utwórz bilet pomocy technicznej, aby umożliwić naszemu zespołowi inżynierów udzielenie pomocy.

## Tworzenie biletu pomocy technicznej
1. Otwórz witrynę [Azure Portal][Azure Portal].
2. Na ekranie głównym kliknij kafelek **Pomoc i obsługa techniczna**.
   
    ![Pomoc i obsługa techniczna](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. W bloku Pomoc i obsługa techniczna kliknij opcję **Utwórz żądanie obsługi**.
   
    ![Nowe żądanie pomocy technicznej](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. Wybierz typ żądania w polu **Typ żądania**.
   
    ![Typ żądania](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > Domyślnie każdy serwer SQL (np. myserver.database.windows.net) ma **limit przydziału jednostek DTU** wynoszący 45 000. Ten limit przydziału jest po prostu limitem bezpieczeństwa. Możesz zwiększyć limit przydziału, tworząc bilet pomocy technicznej i wybierając pozycję *Limit przydziału* jako typ żądania. Aby obliczyć zapotrzebowanie na jednostki DTU, należy pomnożyć 7,5 przez łączną wymaganą liczbę jednostek [DWU][DWU]. Jeśli na przykład chcesz hostować dwie bazy danych z poziomem celu usługi DW6000 na jednym serwerze SQL, zażądaj limitu przydziału wynoszącego 90 000.  Aktualne użycie jednostek DTU możesz zobaczyć z poziomu bloku serwera SQL w portalu. Limit przydziału jednostek DTU obejmuje zarówno wstrzymane, jak i niewstrzymane bazy danych. 
   > 
   > 
5. W polu **Subskrypcja** wybierz subskrypcję obejmującą problematyczną bazę danych.
   
    ![Subskrypcja](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. Wybierz opcję **SQL Data Warehouse** jako zasób.
   
    ![Zasób](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. Wybierz odpowiedni [Plan pomocy technicznej platformy Azure][Plan pomocy technicznej platformy Azure].
   
   * Pomoc techniczna związana z **rozliczeniami, limitami przydziałów i zarządzaniem subskrypcjami** jest dostępna na wszystkich poziomach pomocy technicznej.
   * **Naprawa w razie awarii** jest oferowana w ramach pomocy technicznej na poziomach [Developer][Developer], [Standard][Standard], [Professional Direct][Professional Direct] i [Premium][Premium]. Naprawa w razie awarii dotyczy problemów napotykanych przez klientów podczas korzystania z platformy Azure, jeśli istnieje uzasadnione podejrzenie, że problem leży po stronie firmy Microsoft.
   * **Wsparcie dla deweloperów** i **usługi doradcze** są dostępne na poziomach pomocy technicznej [Professional Direct][Professional Direct] i [Premium][Premium]. 
     
     Jeśli masz plan pomocy technicznej Premium, możesz zgłaszać także problemy związane z usługą SQL Data Warehouse na portalu [Microsoft Premier Online][Microsoft Premier Online].  Zobacz artykuł [Plany pomocy technicznej platformy Azure][Plan pomocy technicznej platformy Azure], aby dowiedzieć się więcej o różnych planach pomocy technicznej, ich zakresach, czasach reakcji, cenach itp.  Często zadawane pytania na temat pomocy technicznej platformy Azure można znaleźć w temacie [Pomoc techniczna platformy Azure — często zadawane pytania][Pomoc techniczna platformy Azure — często zadawane pytania].  
     
     ![Plan pomocy technicznej](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. Wybierz odpowiednie wartości w polach **Typ problemu** i **Kategoria**.
   
    ![Typ problemu i kategoria](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. Opisz problem i wybierz poziom jego wpływu na prowadzoną działalność.
   
    ![Opis problemu](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. Twoje **informacje kontaktowe** dotyczące tego biletu pomocy technicznej zostaną wstępnie wypełnione. Zaktualizuj je w razie potrzeby.
    
    ![Informacje kontaktowe](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. Kliknij przycisk **Utwórz**, aby przesłać żądanie pomocy technicznej.

## Monitorowanie biletu pomocy technicznej
Po przesłaniu żądania pomocy technicznej zespół pomocy technicznej platformy Azure skontaktuje się z Tobą. Aby sprawdzić stan żądania i szczegółowe informacje, kliknij opcję **Zarządzaj żądaniami obsługi** na pulpicie nawigacyjnym.

![Sprawdzanie stanu](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## Inne zasoby
Ponadto można połączyć się ze społecznością usługi SQL Data Warehouse w witrynie [Stack Overflow][Stack Overflow] lub na [forum MSDN dotyczącym usługi Azure SQL Data Warehouse][forum MSDN dotyczącym usługi Azure SQL Data Warehouse].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Azure Portal]: https://portal.azure.com/
[Plan pomocy technicznej platformy Azure]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premium]: https://azure.microsoft.com/support/plans/premier/  
[Pomoc techniczna platformy Azure — często zadawane pytania]: https://azure.microsoft.com/support/faq/
[Microsoft Premier Online]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[forum MSDN dotyczącym usługi Azure SQL Data Warehouse]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/




<!--HONumber=Sep16_HO3-->


