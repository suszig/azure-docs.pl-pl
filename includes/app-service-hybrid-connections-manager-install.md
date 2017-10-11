
1. W **połączeń hybrydowych** bloku, kliknij połączenie hybrydowe właśnie utworzony, kliknięcie **konfiguracji odbiornika**.
   
    ![Kliknij przycisk Ustawienia odbiornika](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. **Właściwości połączenia hybrydowe** zostanie otwarty blok. W obszarze **Menedżera połączeń hybrydowych lokalnymi**, wybierz **pobrać i skonfigurować ręcznie**, Zapisz pobrany pakiet HybridConnectionManager.msi i skopiuj parametry połączenia bramy.
   
    ![Kliknij tutaj, aby zainstalować](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. Z wiersza polecenia administratora wpisz następujące polecenie, aby uruchomić Instalatora:
   
        start HybridConnectionManager.msi
4. Po uruchomieniu Instalatora, kliknij przycisk **nie teraz**, następnie przejdź do folderu %ProgramFiles%\Microsoft\HybridConnectionManager, uruchom HCMConfigWizard.exe i kliknij przycisk **tak** w **konta użytkownika Formant** okna dialogowego.
5. Wklej parametry połączenia hybrydowe, które wcześniej zostały skopiowane, a następnie kliknij przycisk **OK**. 
   
    ![Instalowanie](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. Po zakończeniu instalacji kliknij przycisk **Zamknij**.
   
    ![Kliknij przycisk Zamknij](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    Na **połączeń hybrydowych** bloku **stan** zawiera obecnie kolumnę **połączony**. 
   
    ![Stan połączenia](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

