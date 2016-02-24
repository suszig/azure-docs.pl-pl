<properties 
    pageTitle="Azure Machine Learning でカスタム R モジュールを作成する | Microsoft Azure" 
    description="Azure Machine Learning における作成者カスタム R モジュールのクイック スタート。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"  
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="tbd" 
    ms.date="12/11/2015" 
    ms.author="bradsev;ankarloff" />


# Azure Machine Learning でカスタム R モジュールを作成する

このトピックでは、Azure Machine Learning でカスタム R モジュールを作成し、デプロイする方法について説明します。 カスタム R モジュールの概要と、このモジュールの定義に使用するファイルについて説明します。 また、モジュールを定義するファイルを作成する方法と、Machine Learning ワークスペースにデプロイするためにモジュールを登録する方法も示します。 カスタム モジュールの定義で使用する要素および属性についてさらに詳しく説明します。 補助機能と補助ファイルおよび複数の出力を使用する方法についても説明します。 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## カスタム R モジュールとは
A **カスタム モジュール** ワークスペースにアップロードして Azure Machine Learning の実験の一部として実行できるユーザー定義モジュールです。 A **カスタム R モジュール** カスタム モジュールには、ユーザー定義の R 関数を実行します。 R とは、アルゴリズムを実装するために統計学者やデータ科学者によって広く使用されている統計コンピューティングおよびグラフィックス用のプログラミング言語です。 現在、カスタム モジュールでサポートされている言語は R だけですが、今後のリリースで他の言語のサポートが追加される予定です。

カスタム モジュールが **ファースト クラス ステータス** 他のモジュールと同じように使用できるという意味での Azure Machine Learning でします。 カスタム モジュールは、発行済みの実験や視覚化に含まれる他のモジュールとともに実行できます。 ユーザーは、モジュールによって実装されたアルゴリズム、使用される入出力ポート、モデリング パラメーター、およびその他さまざまな実行時の動作を制御できます。 カスタム モジュールは、そのモジュールが作成されたワークスペースでのみ使用できます。コミュニティの実験に発行することはできません。

## カスタム R モジュールのファイル
カスタム R モジュールは、少なくとも以下の 2 つのファイルが含まれる .zip ファイルによって定義されます。

* A **ソースファイル** モジュールによって公開される R 関数を実装します。
*  **XML 定義ファイル** カスタム モジュール インターフェイスを記述します。

カスタム モジュールからアクセスできる機能を提供する追加の補助ファイルも .zip ファイルに含まれる場合があります。 このオプションは、以下について説明します。

## クイック スタートの例: カスタム R モジュールの定義、パッケージ化、登録
この例では、カスタム R モジュールに必要なファイルを作成し、それらのファイルを zip ファイルにパッケージ化して、Machine Learning ワークスペースにモジュールを登録する方法を示します。 この例の zip パッケージとサンプル ファイルはからダウンロードできます [ダウンロード CustomAddRows.zip ファイル](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409)します。

例を考えて、 **Custom Add Rows** 2 つのデータセット (データ フレーム) を行 (監視) を連結するために使用する Add Rows モジュールの標準的な実装を変更するモジュールです。 標準の Add Rows モジュールは、rbind アルゴリズムを使用して、最初の入力データセットの末尾に、2 番目の入力データセットの行を追加します。 カスタマイズされた `CustomAddRows` 関数も 2 つのデータセットを同様に受け入れますが、入力として追加のブール型スワップ パラメーターも受け取ります。 スワップ パラメーターが場合 **FALSE**, 、標準の実装と同じデータセットが返されます。 スワップ パラメーターがある場合、 **TRUE**, 、2 番目のデータセットの末尾に最初の入力データセットの行の代わりに追加します。 R の実装ファイル `CustomAddRows` によって公開されている関数、 **Custom Add Rows** モジュールには、次の R コードが含まれています。

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

このメソッドを公開する `CustomAddRows` を指定する XML 定義ファイルを Azure Machine Learning モジュールとして関数を作成する必要がありますが、どのように **Custom Add Rows** モジュールの表示し、動作する必要があります。 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another. Dataset 2 is concatenated to Dataset 1 when Swap is false, and vice versa when Swap is true.</Description>
    
    <!-- Specify the base language, script file and R function to use for this module. -->      
        <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />  
        
    <!-- Define module input and output ports -->
    <!-- Note: The values of the id attributes in the Input and Arg elements must match the parameter names in the R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>Combined dataset</Description>
            </Output>
        </Ports>
        
    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>

 
注意の値、 **id** の属性、 **入力** と **Arg** XML ファイル内の要素では、R コードの関数のパラメーター名を完全に一致する必要があります (*dataset1*, 、*dataset2*, 、および *スワップ* 例では)。 同様の値、 **entryPoint** の属性、 **言語** 要素は、R スクリプトの関数の名前を正確に一致する必要があります (*CustomAddRows* 例では)。 これに対し、 **id** 属性を **出力** 要素は、R スクリプト内のどの変数には対応しません。 複数の出力が必要な場合は、XML ファイルで出力が宣言されている順序と同じ順序で結果が配置されたリストを R 関数から返します。

これら 2 つのファイルとして保存 *CustomAddRows.R* と *CustomAddRows.xml* および zip それらにまとめて、 *CustomAddRows.zip* ファイルです。

Machine Learning ワークスペースには、それらを登録、Machine Learning Studio でワークスペースに移動] をクリックして、 **+ 新規** 下部にあるボタンをクリックし、選択 **モジュール ZIP パッケージから]-> [** 新しい Custom Add Rows モジュールをアップロードします。

![Zip のアップロード](./media/machine-learning-custom-r-modules/upload-from-zip-package.png)

 **Custom Add Rows** モジュールは Machine Learning の実験によってアクセスできるようになりました。

## XML 定義ファイルの要素

### Module 要素
 **モジュール** 要素を使用して XML ファイルでカスタム モジュールを定義します。 複数のモジュールは、複数を使用して 1 つの XML ファイルで定義できます **モジュール** 要素。 ワークスペース内の各モジュールには、一意の名前が必要です。 既存のカスタム モジュールと同じ名前でカスタム モジュールを登録すると、既存のモジュールが新しいモジュールに置き換えられます。 ただし、既存の Azure Machine Learning モジュールと同じ名前でカスタム モジュールを登録できます。この場合、カスタム モジュールはモジュール パレットのカスタム カテゴリに表示されます。

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another...</Description>/> 


内で、 **モジュール** 要素、オプションを指定できます **所有者** モジュールに組み込まれる要素と同様に、 **説明** モジュールのクイック ヘルプに表示されるテキストであるし、Machine Learning の UI でモジュールの上マウスを置いたときの要素。

**Module 要素内の文字数制限に関する規則**:

* 値、 **名前** 属性、 **モジュール** 要素の長さは 64 文字を超えない必要があります。 
* 内容、 **説明** 要素の長さは 128 文字を超えない必要があります。
* 内容、 **所有者** 要素は、32 文字を超えない必要があります。

**モジュールの結果が確定的か非確定的かを示す

既定では、すべてのモジュールが確定的であると見なされます。 つまり、不変の一連のパラメーターを指定するには、モジュール必要があります同じ結果を返す実行時に各します。 Azure Machine Learning Studio では、この動作を想定して、パラメーターまたは入力データが変更されていない限り、確定的としてマークされたモジュールは再実行されません。 キャッシュされた結果が返されるので、実験の実行が高速化されます。

ただし、独自のモジュールが実行のたびに異なる結果を返す関数、RAND や、現在の日付または時刻を返す関数を使用する場合は、非確定的なオプションを設定するあると、モジュールを指定できます **isDeterministic** 属性を **false**します。 モジュールの入力やパラメーターが変更されていなくても、実験を実行するたびにモジュールが再実行されます。 

### 言語定義
 **言語** XML 定義ファイル内の要素を使用して、カスタム モジュールの言語を指定します。 現在、サポートされている言語は R だけです。 値、 **sourceFile** 属性は、モジュールの実行時に呼び出す関数が含まれた R ファイルの名前を指定する必要があります。 このファイルは zip パッケージに含める必要があります。 値、 **entryPoint** 属性が呼び出される関数の名前を指定し、ソース ファイル内で定義されている有効な関数に一致する必要があります。

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### ポート
子要素でカスタム モジュールの入力と出力ポートが指定されている、 **ポート** XML 定義ファイルのセクションです。 これらの要素の順序によって、ユーザーに表示されるレイアウト (UX) が決まります。 最初の子 **入力** または **出力** に示されている、 **ポート** XML ファイルの要素が Machine Learning の UX で一番左の入力ポートになります。
各入力し、出力ポートは省略可能な必要があります **説明** 、ユーザーが Machine Learning の UI でポート上でマウス カーソルを置いたときに表示されるテキストを指定する子要素です。

**ポートの規則**:

* 最大数 **入力と出力ポート** 各 8 です。

### Input 要素
入力ポートを使用して、ユーザーは R 関数とワークスペースにデータを渡すことができます。  **データ型** については、次のように入力ポートをサポートします。 

**DataTable:** この型は、data.frame として R 関数に渡されます。 実際には、互換性のある Machine Learning によってサポートされている型 (CSV ファイルや ARFF ファイルなど) **DataTable** data.frame に自動的に変換されます。 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
        </Input>

 **Id** 属性が関連付けられている各 **DataTable** 入力ポートは、一意の値を持つ必要があり、この値は、その対応する R 関数のパラメーターの名前と一致する必要があります。
省略可能な **DataTable** 実験で入力として渡されないポートは、値を持つ **NULL** 関数に渡される、R オプションの zip ポートは、入力が接続されていない場合に無視されます。  **IsOptional** 属性は、両方のオプション、 **DataTable** と **Zip** 種類であり、 *false* 既定です。
       
**郵便番号:** カスタム モジュールは、入力としての zip ファイルを受け取ることができます。 この入力は、関数の R 作業ディレクトリにアンパックされます。

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files will be extracted to the R working directory.</Description>
        </Input>

カスタム R モジュールでは、zip ファイルは自動的に R 作業ディレクトリに展開されるので、Zip ポートの ID は R 関数のどのパラメーターとも一致する必要はありません。

**入力の規則:**

* 値、 **id** の属性、 **入力** 要素が有効な R 変数名にする必要があります。
* 値、 **id** の属性、 **入力** 要素を 64 文字より長くすることはできません。
* 値、 **名前** の属性、 **入力** 要素を 64 文字より長くすることはできません。
* 内容、 **説明** 要素を 128 文字より長くすることはできません
* 値、 **型** の属性、 **入力** 要素である必要があります *Zip* または *DataTable*します。
* 値、 **isOptional** の属性、 **入力** 要素は必要ありません (は *false* 指定されていない場合、既定で); 指定されている場合がありますが、 *true* または *false*します。

### Output 要素

**標準出力ポート:**
出力ポートは、後続のモジュールで使用して、R 関数の戻り値にマップされます。 *DataTable* は現在サポートされている唯一の標準出力ポートの種類。 (サポート *学習器* と *変換* は近日公開予定です)。A *DataTable* として出力を定義します。

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

カスタム R モジュールの値の出力での **id** 属性は、R スクリプトの何かと対応する必要はありませんが、一意である必要があります。 1 つのモジュールの出力が、R 関数の戻り値がある必要があります、 *data.frame*します。 サポート対象のデータ型の複数のオブジェクトを出力するために、適切な出力ポートを XML 定義ファイルで指定する必要があります。また、オブジェクトをリストとして返す必要があります。 返されたリストに配置されたオブジェクトの順序を反映して、左から右に、出力オブジェクトが出力ポートに割り当てられます。
 
たとえば、dataset、Dataset1、Dataset2 をそれぞれ出力ポート dataset、dataset1、dataset2 に左から右に出力する場合、"CustomAddRows.xml" ファイルで出力ポートを次のように定義します。

変更する場合など、 **Custom Add Rows** 元の 2 つのデータセットを出力するモジュール *dataset1*, 、および *dataset2*, 、だけでなく、新しい結合データセット *データセット* (左から右への順序でとして: *データセット*, 、*dataset1*, 、*dataset2*)、次のように、CustomAddRows.xml ファイルで出力ポートを定義します。

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


"CustomAddRows.R" に示されている正しい順序でオブジェクトのリストを返します。

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 
    
**視覚化出力:**
型の出力ポートを指定することも *視覚化* R グラフィックス デバイスとコンソール出力からの出力を表示します。 このポートは R 関数の出力には含まれず、他の出力ポートの型の順序に干渉しません。 カスタム モジュールに視覚化ポートを追加するには、追加、 **出力** の値を持つ要素 *視覚化* の **型** 属性。

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View the R console graphics device output.</Description>
    </Output>
    
**出力の規則:**

* 値、 **id** の属性、 **出力** 要素が有効な R 変数名にする必要があります。
* 値、 **id** の属性、 **出力** 要素を 32 文字より長くすることはできません。
* 値、 **名前** の属性、 **出力** 要素を 64 文字より長くすることはできません。
* 値、 **型** の属性、 **出力** 要素である必要があります *視覚化*します。

### 引数
追加のデータで定義されているモジュールのパラメーターを使用して、R 関数に渡すことが、 **引数** 要素。 これらのパラメーターは、モジュールを選択したときに、Machine Learning の UI の一番右にあるプロパティ ウィンドウに表示されます。 引数はサポートされているどの型でもかまいません。また、必要に応じてカスタム列挙型を作成することもできます。 ような **ポート** 要素、 **引数** 要素は省略可能なことができますが **説明** パラメーター名の上にマウスを置いたときに表示されるテキストを指定する要素。
DefaultValue、minValue、maxValue などのモジュールの省略可能なプロパティは、属性として引数に追加できる、 **プロパティ** 要素。 有効なプロパティ、 **プロパティ** 要素は、引数の型に依存しはサポートされている引数の型を以下に説明します。
入力および出力と同様、各パラメーターで一意の ID 値をそれらに関連付けることが重要になります。 また、ID 値は R 関数で指定したパラメーターに対応している必要があります。 このクイック スタートの例では、関連付けられている id/パラメーターは *スワップ*します。

### Arg 要素
使用してモジュールのパラメーターを定義、 **Arg** の子要素、 **引数** XML 定義ファイルのセクションです。 内の子要素と同様、 **ポート** セクションのパラメーターの順序、 **引数** セクションは、UX で発生するレイアウトを定義します。 UI では、パラメーターは XML ファイルで定義されている順序で上から下に表示されます。 パラメーターについて Machine Learning によってサポートされるタイプを以下にリストします。 

**int** -整数 (32 ビット) 型パラメーターです。

        <Arg id="intValue1" name="Int Param" type="int">
            <Properties min="0" max="100" default="0" />
            <Description>Integer Parameter</Description>
       </Arg>



* *省略可能なプロパティ*: **min**, 、**max** と **既定**

**二重** – double 型のパラメーターです。

       <Arg id="doubleValue1" name="Double Param" type="double">
           <Properties min="0.000" max="0.999" default="0.3" />
           <Description>Double Parameter</Description>
       </Arg>


* *省略可能なプロパティ*: **min**, 、**max** と **既定**

**bool** – ux のチェック ボックスで表されるブール型パラメーター

        <Arg id="boolValue1" name="Boolean Param" type="bool">
            <Properties default="true" />
            <Description>Boolean Parameter</Description>
        </Arg>



* *省略可能なプロパティ*: **既定** に設定されていない場合は false

**文字列**: 標準の文字列

        <Arg id="stringValue1" name="My string Param" type="string">
           <Properties default="Default string value." isOptional="true" />
           <Description>String Parameter 1</Description>
        </Arg>


* *省略可能なプロパティ*: **既定** と **isOptional** -既定値のないオプションの文字列は null として R 関数に、値がそれ以外の場合、ユーザーが指定されていない場合。

**ColumnPicker**: 列選択パラメーター。 この型は、列の選択として UX に表示されます。  **プロパティ** 列が選択される、ターゲット ポートの種類がある必要がありますポートの id を指定する要素がここでは使用 *DataTable*します。 列選択の結果は、選択された列名を含む文字列のリストとして R 関数に渡されます。 

        <Arg id="colset" name="Column set" type="ColumnPicker">   
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *必須プロパティ*: **portId** -型の Input 要素の id と一致 *DataTable*します。
* *省略可能なプロパティ*:
    * **allowedTypes** -ユーザーが選択できる列の型をフィルター処理します。 有効な値は、次のとおりです。 
        *   数値
        *   Boolean
        *   カテゴリ
        *   String
        *   ラベル
        *   機能
        *   Score
        *   すべて

    * **既定** -列の選択は有効な既定の設定が含まれます。 
        * なし
        * NumericFeature
        * NumericLabel
        * NumericScore
        * NumericAll
        * BooleanFeature
        * BooleanLabel
        * BooleanScore
        * BooleanAll
        * CategoricalFeature
        * CategoricalLabel
        * CategoricalScore
        * CategoricalAll
        * StringFeature
        * StringLabel
        * StringScore
        * StringAll
        * AllLabel
        * AllFeature
        * AllScore
        * すべて

                                                        
**ドロップダウン**: 指定されたユーザーが (ドロップダウン) リストを列挙します。 ドロップダウン項目は内で指定されている、 **プロパティ** 要素を使用して、 **項目** 要素。  **Id** 各 **項目** で一意である必要があり、有効な R 変数と、アイテムの名前は、ユーザーと、R 関数に渡される値に表示されるテキスト。

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>  

* *省略可能なプロパティ*:
    * **既定** -既定のプロパティの値は、のいずれかの id 値と一致する必要があります、 **項目** 要素。


### 補助ファイル

カスタム モジュールの ZIP ファイル内に配置されたすべてのファイルは、実行時に使用できるようなります。 ディレクトリ構造がある場合は保持されます。 つまり、ファイル ソーシングはローカルでの実行と Azure Machine Learning での実行で同じように機能します。 すべてのファイルが "src" ディレクトリに展開されることに注意してください。これにより、すべてのパスに "src/" プレフィックスが含まれます。

たとえばをデータセットから行を削除しても、CustomAddRows に出力する前に、重複する行を削除して、R 関数が RemoveDupNARows.R ファイルには既に作成されているとします。

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
次のように、CustomAddRows 関数で補助ファイル RemoveDupNARows.R をソースとして参照できます。

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
            } else { 
                dataset <- rbind(dataset1, dataset2)) 
            } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

次に、"CustomAddRows.R"、"CustomAddRows.xml"、"RemoveDupNARows.R" を含む zip ファイルをカスタム R モジュールとしてアップロードします。

## 実行環境 ##
R スクリプトの実行環境では、同じバージョンの R を使用して、 **R スクリプトの実行** モジュール、同じ既定のパッケージを使用することができます。 他の R パッケージをカスタム モジュールに追加するには、独自の R 環境の場合と同様に、それらのパッケージをカスタム モジュールの zip パッケージに含め、R スクリプトに読み込みます。 

**実行環境の制限事項** が含まれます。

* 非永続的なファイル システム: カスタム モジュールの実行時に書き込まれるファイルは、同じモジュールの複数の実行間で保持されません。
* ネットワーク アクセスはありません。



 

