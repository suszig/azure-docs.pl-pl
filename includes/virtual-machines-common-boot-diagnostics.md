Na platformie Azure jest teraz dostępna obsługa dwóch funkcji debugowania: obsługa danych wyjściowych konsoli i zrzutu ekranu dla modelu wdrażania przy użyciu usługi Azure Virtual Machines Resource Manager. 

Podczas korzystania z własnego obrazu na platformie Azure, a nawet wykonywania rozruchu jednego z obrazów platformy, może wystąpić wiele przyczyn przejścia maszyny wirtualnej do stanu uniemożliwiającego rozruch. Te funkcje umożliwiają łatwe diagnozowanie i odzyskiwanie maszyn wirtualnych po niepowodzeniach rozruchu.

Dla maszyn wirtualnych z systemem Linux można łatwo wyświetlić dane wyjściowe konsoli dziennika za pomocą portalu:

![Azure Portal](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
Jednak zarówno dla maszyn wirtualnych z systemem Linux, jak i Windows, platforma Azure umożliwia również wyświetlenie zrzutu ekranu maszyny wirtualnej za pomocą funkcji hypervisor:

![Błąd](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

Obie te funkcje są obsługiwane dla maszyn wirtualnych platformy Azure we wszystkich regionach. Należy pamiętać, że może minąć do 10 minut, zanim zrzuty ekranu i dane wyjściowe pojawią się na koncie magazynu.

## <a name="common-boot-errors"></a>Typowe błędy rozruchu

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Nie znaleziono systemu operacyjnego](https://support.microsoft.com/help/4010142)
- [Niepowodzenie rozruchu lub błąd INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Włączanie diagnostyki na nowej maszynie wirtualnej
1. Podczas tworzenia nowej maszyny wirtualnej w portalu w wersji zapoznawczej wybierz pozycję **Azure Resource Manager** z listy rozwijanej modelu wdrażania:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. Skonfiguruj opcję Monitorowanie, aby wybrać konto magazynu, w którym chcesz umieścić te pliki diagnostyczne.
 
    ![Tworzenie maszyny wirtualnej](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. Jeśli wykonujesz wdrożenie z szablonu usługi Azure Resource Manager, przejdź do zasobu maszyny wirtualnej i dołącz sekcję profilu diagnostyki. Pamiętaj, aby użyć nagłówka wersji interfejsu API „2015-06-15”.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Profil diagnostyki umożliwia wybranie konta magazynu, na którym chcesz umieścić te dzienniki.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

Aby wdrożyć przykładową maszynę wirtualną z włączoną diagnostyką rozruchu, zapoznaj się z naszym repozytorium w tym miejscu.

## <a name="update-an-existing-virtual-machine"></a>Aktualizowanie istniejącej maszyny wirtualnej ##

Aby włączyć diagnostykę rozruchu za pośrednictwem portalu, można także zaktualizować istniejącą maszynę wirtualną za pośrednictwem portalu. Wybierz opcję Diagnostyka rozruchu i kliknij ikonę Zapisz. Uruchom ponownie maszynę wirtualną, aby zmiany zaczęły obowiązywać.

![Aktualizowanie istniejącej maszyny wirtualnej](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

