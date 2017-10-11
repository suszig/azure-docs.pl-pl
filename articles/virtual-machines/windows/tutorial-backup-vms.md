---
title: Maszyny wirtualne systemu Windows Azure Backup | Dokumentacja firmy Microsoft
description: "Ochrona maszyn wirtualnych systemu Windows przez tworzenie ich kopii zapasowych za pomocą usługi Kopia zapasowa Azure."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 8e58a2290e5034ef393f65cbcddb86e18cf4a6ec
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a>Tworzenie kopii zapasowych maszyn wirtualnych systemu Windows na platformie Azure

Możesz chronić swoje dane, tworząc kopie zapasowe w regularnych odstępach czasu. Kopia zapasowa Azure tworzy punkty odzyskiwania, które są przechowywane w magazynach odzyskiwania z magazynu geograficznie nadmiarowego. Po przywróceniu z punktu odzyskiwania, można przywrócić całej maszyny Wirtualnej lub po prostu określonych plików. W tym artykule opisano sposób przywracania jednego pliku do maszyny Wirtualnej z systemem Windows Server i usług IIS. Jeśli nie masz jeszcze maszynę Wirtualną do obsługi, możesz utworzyć go przy użyciu [Szybki Start systemu Windows](quick-create-portal.md). Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz kopię zapasową maszyny Wirtualnej
> * Harmonogram tworzenia codziennej kopii zapasowej
> * Przywróć plik z kopii zapasowej




## <a name="backup-overview"></a>Omówienie usługi Backup

Gdy usługa Azure Backup Inicjuje zadania tworzenia kopii zapasowej, wyzwala zapasowy numer wewnętrzny do tworzenia migawki punktu w czasie. Używa usługi Azure Backup _VMSnapshot_ rozszerzenia. Rozszerzenie jest zainstalowany podczas pierwszego tworzenia kopii zapasowej maszyny Wirtualnej, jeśli maszyna wirtualna jest uruchomiona. Jeśli maszyna wirtualna nie działa, usługa Kopia zapasowa tworzy migawkę powiązany magazyn (ponieważ nie zapisy aplikacji są wykonywane, gdy maszyna wirtualna zostanie zatrzymana).

Podczas wykonywania migawki maszyn wirtualnych systemu Windows, usługi Kopia zapasowa koordynuje z woluminów w tle kopii Service (VSS) do uzyskania migawki spójne z dysków maszyny wirtualnej. Gdy usługa Kopia zapasowa Azure przyjmuje migawki, dane są przesyłane do magazynu. Aby zmaksymalizować wydajność, usługa identyfikuje i transferuje tylko bloki danych, które uległy zmianie od czasu poprzedniej kopii zapasowej.

Po ukończeniu transferu danych migawki zostaną usunięte i utworzenia punktu odzyskiwania.


## <a name="create-a-backup"></a>Tworzenie kopii zapasowej
Utwórz prostą, zaplanowaną, codzienną operację tworzenia kopii zapasowych w magazynie usługi Recovery Services. 

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. W menu po lewej stronie wybierz pozycję **Maszyny wirtualne**. 
3. Z listy wybierz maszynę wirtualną do utworzenia kopii zapasowej.
4. W bloku maszyny Wirtualnej w **ustawienia** kliknij **kopii zapasowej**. **Kopii zapasowej Włącz** zostanie otwarty blok.
5. W **magazyn usług odzyskiwania i**, kliknij przycisk **Utwórz nowy** i podaj nazwę dla nowego magazynu. Nowy magazyn jest tworzony w tej samej lokalizacji co maszyna wirtualna i grupy zasobów.
6. Kliknij przycisk **kopii zapasowej zasad**. W tym przykładzie należy zachować ustawienia domyślne, a następnie kliknij przycisk **OK**.
7. Na **kopii zapasowej Włącz** bloku, kliknij przycisk **Włącz kopię zapasową**. Spowoduje to utworzenie kopii zapasowej codziennie na podstawie harmonogramu domyślnego.
10. Tworzenie punktu odzyskiwania początkowej na **kopii zapasowej** kliknij bloku **wykonaj kopię zapasową teraz**.
11. Na **Utwórz kopię zapasową teraz** bloku, kliknij ikonę kalendarza, użyj formant kalendarza, aby wybrać ostatni dzień tego punktu odzyskiwania jest zachowywana, a następnie kliknij przycisk **kopii zapasowej**.
12. W **kopii zapasowej** bloku dla maszyny Wirtualnej, zobaczysz liczbę punktów odzyskiwania, które są spełnione.

    ![Punkty odzyskiwania](./media/tutorial-backup-vms/backup-complete.png)
    
Pierwszej kopii zapasowej trwa około 20 minut. Po zakończeniu tworzenia kopii zapasowej, należy przejść do następnej części tego samouczka.

## <a name="recover-a-file"></a>Odzyskiwanie pliku

Jeśli przypadkowo usunięte lub wprowadzić zmiany w pliku, możesz użyć odzyskiwania plików, aby odzyskać plik z magazynu kopii zapasowej. Odzyskiwanie plików używa skryptu uruchamianego na maszynie Wirtualnej, należy zainstalować punkt odzyskiwania jako dysk lokalny. Dyski te pozostanie zainstalowanego na 12 godzin, co może kopiować pliki z punktu odzyskiwania i przywrócić je do maszyny Wirtualnej.  

W tym przykładzie zostanie przedstawiony sposób odzyskać plik obrazu, który jest używany w domyślnej strony sieci web dla usług IIS. 

1. Otwórz przeglądarkę i połącz się adres IP maszyny wirtualnej można wyświetlić domyślną stronę usług IIS.

    ![Domyślna strona sieci web usług IIS](./media/tutorial-backup-vms/iis-working.png)

2. Połączenie z maszyną Wirtualną.
3. Na Maszynie wirtualnej, należy otworzyć **Eksploratora plików** i przejdź do \inetpub\wwwroot i usuń plik **iisstart.png**.
4. Na komputerze lokalnym Odśwież przeglądarkę, aby zobaczyć, czy obraz na domyślną stronę usług IIS został usunięty.

    ![Domyślna strona sieci web usług IIS](./media/tutorial-backup-vms/iis-broken.png)

5. Na komputerze lokalnym, otwórz nową kartę i Przejdź [portalu Azure](https://portal.azure.com).
6. W menu po lewej stronie wybierz **maszyn wirtualnych** i wybierz z listy formularza maszyny Wirtualnej.
8. W bloku maszyny Wirtualnej w **ustawienia** kliknij **kopii zapasowej**. **Kopii zapasowej** zostanie otwarty blok. 
9. Wybierz z menu w górnej części bloku **odzyskiwanie plików**. **Odzyskiwanie plików** zostanie otwarty blok.
10. W **krok 1: Wybierz punkt odzyskiwania**, wybierz punkt odzyskiwania z listy rozwijanej.
11. W **krok 2: Pobieranie skryptu, aby przeglądać i odzyskiwanie plików**, kliknij przycisk **Pobierz plik wykonywalny** przycisku. Zapisz plik, aby Twoje **pobiera** folderu.
12. Na komputerze lokalnym, otwórz **Eksploratora plików** i przejdź do Twojej **pobiera** folderu i skopiuj pobrany plik .exe. Nazwa pliku będzie ona poprzedzona przez nazwę maszyny Wirtualnej. 
13. Na maszynie Wirtualnej (za pośrednictwem połączenia RDP) Wklej plik .exe na pulpicie maszyny Wirtualnej. 
14. Przejdź do pulpitu z maszyną Wirtualną, a następnie kliknij dwukrotnie ikonę na .exe. Zostanie uruchomiony wiersz polecenia i następnie zainstalować punkt odzyskiwania jako udział pliku, który można uzyskać dostęp. Po zakończeniu tworzenia udziału wpisz **q** aby zamknąć okno wiersza polecenia.
15. Na maszynie Wirtualnej, należy otworzyć **Eksploratora plików** i przejdź do litery dysku, którego użyto do udziału plików.
16. Przejdź do \inetpub\wwwroot i skopiuj **iisstart.png** z pliku udziału i wklej go do \inetpub\wwwroot. Na przykład F:\inetpub\wwwroot\iisstart.png skopiować i wkleić c:\inetpub\wwwroot odzyskać pliku.
17. Na komputerze lokalnym otwórz kartę przeglądarki, w której są podłączone do adresu IP maszyny wirtualnej przedstawiający domyślną stronę usług IIS. Naciśnij klawisze CTRL + F5, aby odświeżyć stronę przeglądarki. Powinna zostać wyświetlona, że obraz został przywrócony.
18. Na komputerze lokalnym, wróć do karty przeglądarki dla portalu Azure i w **krok 3: odinstaluj dyski po odzyskaniu** kliknij **odinstalować dyski** przycisku. Jeśli zapomnisz wykonać ten krok, połączenie punktu instalacji jest automatycznie Zamknij po 12 godzinach. Po tych 12 godzin należy pobrać nowe skryptu można utworzyć nowego punktu instalacji.


## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz kopię zapasową maszyny Wirtualnej
> * Harmonogram tworzenia codziennej kopii zapasowej
> * Przywróć plik z kopii zapasowej

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat monitorowania maszyn wirtualnych.

> [!div class="nextstepaction"]
> [Monitorowanie maszyn wirtualnych](tutorial-monitoring.md)









