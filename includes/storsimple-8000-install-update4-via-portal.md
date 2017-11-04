<!--author=alkohli last changed: 07/07/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a>Aby zainstalować aktualizację z witryny Azure Portal

1. Na stronie usługi StorSimple wybierz swoje urządzenie.

    ![Wybierz urządzenie](./media/storsimple-8000-install-update4-via-portal/update1.png)

2. Przejdź do **ustawienia urządzenia** > **aktualizacji urządzenia**.

    ![Kliknij przycisk aktualizacji urządzenia](./media/storsimple-8000-install-update4-via-portal/update2.png)

2. Pojawi się powiadomienie, jeśli są dostępne nowe aktualizacje. Alternatywnie w **aktualizacji urządzenia** bloku, kliknij przycisk **Wyszukaj aktualizacje**. Zostanie utworzone zadanie skanowania dostępnych aktualizacji. Otrzymasz powiadomienie, gdy zadanie zostanie pomyślnie ukończone.

    ![Kliknij przycisk aktualizacji urządzenia](./media/storsimple-8000-install-update4-via-portal/update3.png)

3. Przed zastosowaniem aktualizacji na urządzeniu zalecamy przejrzenie informacji o wersji. Aby zastosować aktualizacje, kliknij przycisk **instalowania aktualizacji**. W **potwierdzić regularnie** bloku Przeglądanie wymagań wstępnych, aby zakończyć przed zastosowaniem aktualizacji. Zaznacz pole wyboru, aby wskazać, że można przystąpić do aktualizacji urządzenia, a następnie kliknij przycisk **zainstalować**.

    ![Kliknij przycisk aktualizacji urządzenia](./media/storsimple-8000-install-update4-via-portal/update4.png)

6. Zostanie rozpoczęty zestaw testów wymagań wstępnych. Testy te obejmują:
   
   * **Testy kondycji kontrolera** w celu sprawdzenia, czy oba kontrolery urządzeń są w dobrej kondycji i w trybie online.
   * **Testy kondycji składnika sprzętowego** w celu sprawdzenia, czy wszystkie składniki sprzętowe Twojego urządzenia StorSimple są w dobrej kondycji.
   * **Testy interfejsu DATA 0** w celu sprawdzenia, czy interfejs DATA 0 jest włączony na Twoim urządzeniu. Jeśli ten interfejs nie jest włączony, należy go włączyć i ponowić próbę.

    Aktualizacja jest pobierane i instalowane tylko wtedy, gdy wszystkie testy zostały wykonane pomyślnie. Zobaczysz powiadomienie o trwających testach. W przypadku awarii prechecks, następnie należy otrzymają przyczyny niepowodzenia. Rozwiązania tych problemów, a następnie spróbuj ponownie wykonać operację. Jeśli nie będziesz w stanie samodzielnie rozwiązać tych problemów, konieczne może być skontaktowanie się z pomocą techniczną firmy Microsoft.

7. Po pomyślnym ukończeniu prechecks jest utworzone zadanie aktualizacji. Otrzymasz powiadomienie o pomyślnym utworzeniu zadania aktualizacji.
   
    ![Tworzenie zadania aktualizacji](./media/storsimple-8000-install-update4-via-portal/update6.png)
   
    Następnie aktualizacja jest stosowania do Twojego urządzenia.

9. Aktualizacja zajmuje kilka godzin. Aby w dowolnym momencie wyświetlić szczegóły zadania, wybierz zadanie aktualizacji i kliknij pozycję **Szczegóły**.

    ![Tworzenie zadania aktualizacji](./media/storsimple-8000-install-update4-via-portal/update8.png)

     Można również monitorować postęp zadania aktualizacji z **ustawienia urządzenia > zadań**. Na **zadania** bloku można wyświetlić postęp aktualizacji.

     ![Tworzenie zadania aktualizacji](./media/storsimple-8000-install-update4-via-portal/update7.png)

10. Po zakończeniu zadania, przejdź do **ustawienia urządzenia > aktualizacji urządzenia**. Teraz należy uaktualnić wersji oprogramowania.

    ![Tworzenie zadania aktualizacji](./media/storsimple-8000-install-update4-via-portal/update9.png)

