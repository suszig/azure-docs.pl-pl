## <a name="create-a-self-hosted-ir"></a>Tworzenie własnego środowiska IR

W tej sekcji utworzysz własne środowisko Integration Runtime (Self-hosted) i skojarzysz je z maszyną lokalną za pomocą bazy danych programu SQL Server. Środowisko Integration Runtime (Self-hosted) jest składnikiem, który kopiuje dane z programu SQL Server na Twojej maszynie do magazynu usługi Azure Blob Storage. 

1. Utwórz zmienną dla nazwy środowiska Integration Runtime. Użyj unikatowej nazwy i zanotuj ją. Będziesz jej używać w dalszej części tego samouczka. 

    ```powershell
   $integrationRuntimeName = "ADFTutorialIR"
    ```
1. Utwórz własne środowisko Integration Runtime. 

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Oto przykładowe dane wyjściowe:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Uruchom następujące polecenie, aby pobrać stan utworzonego środowiska Integration Runtime. Upewnij się, że właściwość **State** ma ustawioną wartość **NeedRegistration**. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Oto przykładowe dane wyjściowe:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Uruchom następujące polecenie, aby pobrać **klucze uwierzytelniania** w celu zarejestrowania własnego środowiska Integration Runtime (Self-hosted) za pomocą usługi Data Factory w chmurze. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Oto przykładowe dane wyjściowe:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy="
   }
   ```    
4. Skopiuj jeden z kluczy (pomiń cudzysłowy) na potrzeby rejestracji środowiska Integration Runtime (Self-hosted), które zainstalujesz na swojej maszynie w następnym kroku.  

## <a name="install-integration-runtime"></a>Instalowanie środowiska Integration Runtime
1. Jeśli środowisko **Microsoft Integration Runtime** jest już zainstalowane na maszynie, odinstaluj je za pomocą apletu **Dodaj lub usuń programy**. 
2. [Pobierz](https://www.microsoft.com/download/details.aspx?id=39717) środowisko Integration Runtime (Self-hosted) na lokalną maszynę z systemem Windows, a następnie uruchom instalację. 
3. Na **stronie powitalnej Kreatora instalacji środowiska Microsoft Integration Runtime** kliknij przycisk **Dalej**.  
4. Na stronie **postanowień licencyjnych** zaakceptuj warunki i umowę licencyjną, a kliknij pozycję **Dalej**. 
5. Na stronie **Folder docelowy** kliknij pozycję **Dalej**. 
6. Na stronie **Gotowe do zainstalowania środowiska Microsoft Integration Runtime** kliknij pozycję **Zainstaluj**. 
7. Jeśli zostanie wyświetlony komunikat ostrzegawczy informujący o tym, że konfigurowany komputer przejdzie w tryb uśpienia lub hibernacji, jeśli nie będzie używany, kliknij przycisk **OK**. 
8. Jeśli zostanie wyświetlone okno **Opcje zasilania**, zamknij je i przejdź do okna Instalatora. 
9. Na stronie **zakończenia pracy z Kreatorem instalacji środowiska Microsoft Integration Runtime** kliknij pozycję **Zakończ**.
10. Na stronie **rejestracji środowiska Integration Runtime (Self-hosted)** wklej klucz zapisany w poprzedniej sekcji i kliknij pozycję **Zarejestruj**. 

   ![Rejestrowanie środowiska Integration Runtime](media/data-factory-create-install-integration-runtime/register-integration-runtime.png)
2. Gdy własne środowisko Integration Runtime zostanie pomyślnie zarejestrowane, zostanie wyświetlony następujący komunikat:

   ![Pomyślnie zarejestrowano](media/data-factory-create-install-integration-runtime/registered-successfully.png)

3. Na stronie **nowego węzła środowiska Integration Runtime (Self-hosted)** kliknij pozycję **Dalej**. 

    ![Strona nowego węzła środowiska Integration Runtime (Self-hosted)](media/data-factory-create-install-integration-runtime/new-integration-runtime-node-page.png)
4. Na stronie **kanału komunikacyjnego Intranet** kliknij pozycję **Pomiń**. Możesz wybrać certyfikację protokołów TLS/SSL w celu zabezpieczenia komunikacji w środowisku Integration Runtime z wieloma węzłami. 

    ![Strona kanału komunikacyjnego Intranet](media/data-factory-create-install-integration-runtime/intranet-communication-channel-page.png)
5. Na stronie **rejestrowania środowiska Integration Runtime (Self-hosted)** kliknij pozycję **Uruchom program Configuration Manager**. 
6. Gdy węzeł zostanie połączony z usługą w chmurze, zostanie wyświetlona następująca strona:

   ![Węzeł jest połączony](media/data-factory-create-install-integration-runtime/node-is-connected.png)
7. Teraz przetestuj łączność z bazą danych programu SQL Server.

    ![Karta Diagnostyka](media/data-factory-create-install-integration-runtime/config-manager-diagnostics-tab.png)   

    - W oknie programu **Configuration Manager** przejdź na kartę **Diagnostyka**.
    - Wybierz **SqlServer** jako **Typ źródła danych**.
    - Wprowadź nazwę **serwera**.
    - Wprowadź nazwę **bazy danych**. 
    - Wybierz tryb **uwierzytelniania**. 
    - Wprowadź nazwę **użytkownika**. 
    - Wprowadź **hasło** dla nazwy użytkownika.
    - Kliknij przycisk **Testuj**, aby upewnić się, że produkt Integration Runtime może połączyć się z serwerem SQL. Jeśli połączenie zostanie pomyślnie nawiązane, zostanie wyświetlony zielony znacznik wyboru. W przeciwnym razie zostanie wyświetlony komunikat o błędzie skojarzony z określonym błędem. Rozwiąż wszelkie problemy i upewnij się, że środowisko Integration Runtime może połączyć się z programem SQL Server.    

    > [!NOTE]
    > Zanotuj te wartości (typ uwierzytelniania, serwer, baza danych, użytkownik, hasło). Będziesz ich używać w dalszej części tego samouczka. 
    
