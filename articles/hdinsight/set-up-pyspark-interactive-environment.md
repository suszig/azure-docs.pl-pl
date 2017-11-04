---
title: "Usługa Azure HDInsight Tools — Konfigurowanie środowiska interakcyjne PySpark dla kodu programu Visual Studio | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać narzędzi HDInsight Azure dla programu Visual Studio Code do tworzenia, przesyłania zapytań i skryptów."
Keywords: "VScode, narzędzia Azure HDInsight, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, interakcyjne Hive, interakcyjne zapytania"
services: HDInsight
documentationcenter: 
author: jejiang
manager: 
editor: 
tags: azure-portal
ms.assetid: 
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 24839aadaee07b98ac5a6e6cfd14e44de54e7e7e
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="set-up-pyspark-interactive-environment-for-visual-studio-code"></a>Konfigurowanie środowiska interakcyjne PySpark dla kodu programu Visual Studio

Poniższe kroki pokazują, jak zainstalować pakiety języka python uruchamianych **HDInsight: PySpark interakcyjne**.


## <a name="set-up-pyspark-interactive-environment-on-macos-and-linux"></a>Konfigurowanie środowiska interakcyjne PySpark na MacOS i Linux
Należy użyć polecenia **pip3** dla następujących kroków, jeśli jest **python 3.x**.
1. Upewnij się, że **Python** i **pip** zainstalowane.
 
    ![wersja narzędzia pip Python](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Instalacja oprogramowania Jupyter
    ```
    sudo pip install jupyter
    ```
    +  Może być następujący komunikat o błędzie wynik w systemie Linux i MacOS:

        ![error1](./media/set-up-pyspark-interactive-environment/error1.png)
        ```Resolve:
        sudo pip uninstall asyncio
        sudo pip install trollies
        ```

    + Zainstaluj libkrb5 dev(For Linux only), może być wyświetlany następujący komunikat o błędzie:

        ![error2](./media/set-up-pyspark-interactive-environment/error2.png)
        ```Resolve:
        sudo apt-get install libkrb5-dev 
        ```

3. Zainstaluj sparkmagic
   ```
   sudo pip install sparkmagic
   ```

4. Upewnij się, że ten ipywidgets jest poprawnie zainstalowany, uruchamiając:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![Zainstaluj jądra otoki](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Zainstaluj jądra otoki. Uruchom **pip Pokaż sparkmagic** i zawiera ścieżkę tego sparkmagic jest zainstalowany. 

    ![Lokalizacja sparkmagic](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Przejdź do lokalizacji i uruchom:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![Zainstaluj kernelspec jupyter](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Sprawdź stan instalacji: 

    ```
    jupyter-kernelspec list
    ```
    ![Lista kernelspec jupyter](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Dla jądra dostępne: **python2** i **pysparkkernel** odpowiadają **python 2.x**, **python3** i  **pyspark3kernel** odpowiadają **python 3.x**. 

8. Uruchom ponownie VScode i z powrotem do uruchomiony Edytor skryptów **HDInsight: PySpark interakcyjne**.

## <a name="next-steps"></a>Następne kroki

### <a name="demo"></a>Demonstracja
* HDInsight dla VScode: [wideo](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Narzędzia i rozszerzenia
* [Narzędzie Azure HDInsight dla kodu programu Visual Studio](hdinsight-for-vscode.md)
* [Zestaw narzędzi platformy Azure dla IntelliJ umożliwia tworzenie i przesyłanie aplikacji Spark Scala](spark/apache-spark-intellij-tool-plugin.md)
* [Debugowanie aplikacji Spark zdalnie za pośrednictwem usługi SSH przy użyciu zestawu narzędzi Azure for IntelliJ](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Debugowanie aplikacji Spark zdalnie za pośrednictwem sieci VPN przy użyciu zestawu narzędzi Azure for IntelliJ](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Użyj narzędzia HDInsight Tools w zestawie narzędzi Azure dla programu Eclipse tworzenie aplikacji Spark](spark/apache-spark-eclipse-tool-plugin.md)
* [Użyj narzędzia HDInsight Tools for IntelliJ z Hortonworks piaskownicy](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Korzystanie z notesów Zeppelin w klastrze Spark w usłudze HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Korzystanie z zewnętrznych pakietów z notesami Jupyter](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalacja oprogramowania Jupyter na komputerze i nawiązywanie połączenia z klastrem Spark w usłudze HDInsight](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Wizualizacja gałęzi przy użyciu usługi Microsoft Power BI w usłudze Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Umożliwia uruchamianie zapytań Hive w usłudze Azure HDInsight Zeppelin](hdinsight-connect-hive-zeppelin.md)
