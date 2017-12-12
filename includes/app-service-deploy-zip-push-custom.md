## <a name="deployment-customization"></a>Dostosowanie wdrożenia

Proces wdrażania zakłada, że plik zip, który wypychanych zawiera gotowe do uruchomienia aplikacji. Domyślnie są uruchamiane bez dostosowań. Można włączyć tego samego procesu kompilacji przypisywany o ciągłej integracji, dodając następujące ustawienia aplikacji:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

Korzystając z wdrożenia wypychania zip, to ustawienie jest **false** domyślnie. Wartość domyślna to **true** wdrożeń ciągłej integracji. Jeśli wartość **true**, dotyczący wdrażania ustawień są używane podczas wdrażania. Te ustawienia można ustawić jako ustawień aplikacji lub w `.deployment` pliku konfiguracji umieszczonym w katalogu głównym pliku zip. Aby uzyskać więcej informacji, zobacz [repozytorium i ustawień dotyczących wdrażania](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) w odwołaniu do wdrożenia.