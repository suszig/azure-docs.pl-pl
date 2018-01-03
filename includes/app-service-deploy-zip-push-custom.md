## <a name="deployment-customization"></a>Dostosowanie wdrożenia

Proces wdrażania zakłada, że plik zip, który wypychanych zawiera gotowe do uruchomienia aplikacji. Domyślnie są uruchamiane bez dostosowań. Aby włączyć tego samego procesu kompilacji, które można uzyskać z ciągłej integracji, dodaj następującą wartość do ustawienia aplikacji:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

Użycie opcji wdrażania wypychania zip, to ustawienie jest **false** domyślnie. Wartość domyślna to **true** wdrożeń ciągłej integracji. Jeśli wartość **true**, dotyczący wdrażania ustawień są używane podczas wdrażania. Te ustawienia można skonfigurować zgodnie z ustawieniami aplikacji lub w pliku konfiguracyjnym .deployment, który znajduje się w katalogu głównym pliku zip. Aby uzyskać więcej informacji, zobacz [repozytorium i ustawień dotyczących wdrażania](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) w odwołaniu do wdrożenia.