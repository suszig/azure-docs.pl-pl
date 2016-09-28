<!--author=alkohli last changed: 9/17/15-->

#### Dodawanie konta magazynu w usłudze StorSimple 8000 Series Update 1.0

1. Na stronie początkowej usługi StorSimple Manager zaznacz usługę i kliknij ją dwukrotnie. Nastąpi przekierowanie do strony **Szybki start**. Wybierz stronę **Konfigurowanie**.

2. Kliknij przycisk **Dodaj/edytuj konto magazynu**.

3. W oknie dialogowym **Dodaj/edytuj konto magazynu** kliknij opcję **Dodaj nowe**.

4. W polu **Dostawca** wybierz odpowiedniego dostawcę usługi w chmurze. Obsługiwanymi dostawcami są Azure, Amazon S3, Amazon S3 z RRS, HP i OpenStack. Określ poświadczenia i lokalizację skojarzone z kontem magazynu dostawców usługi w chmurze. Pola wyświetlone dla poświadczeń będą różnić się w zależności od wybranego dostawcy usługi w chmurze. 
  - Jeśli jako dostawcę usługi w chmurze wybrano platformę Azure, w polu **Nazwa** podaj nazwę oraz w polu **Klucz dostępu** wpisz podstawowy klucz dostępu dla konta usługi Microsoft Azure Storage. Lokalizacja dla konta platformy Azure zostanie wypełniona automatycznie.

        ![Add Azure storage account](./media/storsimple-configure-new-storage-account-u1/AddAzureStorageaccount-include.png)

 - W przypadku wybrania dostawcy Amazon S3 lub Amazon S3 z RRS wpisz przyjazną nazwę w polu **Nazwa konta magazynu** oraz wypełnij pola **Klucz dostępu** i **Klucz tajny**. Dla dostawców Amazon S3 i Amazon S3 z RRS obsługiwane są następujące lokalizacje:

        - US Standard
        - US West (Oregon)
        - US West (Northern California)
        - EU (Ireland)
        - Asia Pacific (Singapore)
        - Asia Pacific (Sydney)
        - Asia Pacific (Tokyo)
        - South America (Sao Paulo)

        ![Add Amazon storage account](./media/storsimple-configure-new-storage-account-u1/AddAmazonStorageaccount-include.png)
            
 - W przypadku wybrania HP jako dostawcy usługi w chmurze wpisz przyjazną nazwę w polu **Nazwa konta magazynu** oraz wypełnij pola **Identyfikator dzierżawcy**, **Nazwa użytkownika** i **Hasło**. Dla dostawcy HP obsługiwane są następujące lokalizacje:

        - US East
        - US West
      
        ![Add HP storage account](./media/storsimple-configure-new-storage-account-u1/AddHPStorageaccount-include.png)
            
 - Jeśli jako dostawcę usługi w chmurze wybrano **Openstack**, wypełnij pola **Nazwa hosta**, **Klucz dostępu** i **Klucz tajny**.

        > [AZURE.NOTE] W przypadku wszystkich dostawców usługi w chmurze z wyjątkiem platformy Azure dozwolone są przyjazne nazwy. Można użyć różnych przyjaznych nazw i utworzyć więcej niż jedno konto magazynu przy użyciu tego samego zestawu poświadczeń.

        ![Add Openstack storage account](./media/storsimple-configure-new-storage-account-u1/AddOpenstackStorageaccount-include.png)

5. Wybierz opcję **Włącz tryb SSL**, aby utworzyć bezpieczny kanał komunikacji sieciowej między urządzeniem i chmurą. Pole wyboru **Włącz tryb SSL** należy odznaczyć tylko wówczas, gdy pracujesz w chmurze prywatnej.

      > [AZURE.NOTE] Jeśli używanym dostawcą jest HP, protokół SSL będzie zawsze włączony.
        
6. Po ustawieniu filtrów ![ikona znacznika wyboru](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png). Po pomyślnym utworzeniu konta magazynu otrzymasz powiadomienie.

7. Nowo utworzone konto magazynu zostanie wyświetlone na stronie **Konfigurowanie** w obszarze **Konto magazynu**. Kliknij przycisk **Zapisz**, aby zapisać nowe konto magazynu. Po wyświetleniu monitu o potwierdzenie kliknij przycisk **OK**.


<!--HONumber=Sep16_HO3-->


