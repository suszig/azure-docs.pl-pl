<properties 
    pageTitle="クラスター モデル | Microsoft Azure" 
    description="クラスター モデル" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/24/2015" 
    ms.author="lazzeri"/> 


#クラスター モデル    

クレジット カード発行者の償却リスクを軽減するために、クレジット カード所有者グループの行動を予測することはできますか。 職場でのパフォーマンスを向上させるために、従業員の性格特性グループを定義することができますか。 医師が、病気の特性に基づいて患者をグループに分類できますか。 原則として、こうした質問はすべて、クラスター分析によって回答できます。   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
クラスター分析は、変数の組み合わせに基づいて、一連の観察を 2 つ以上の相互に排他的な未知のグループに分類します。 クラスター分析の目的は、観察 (通常は人々やその特性に対する観察) をグループに整理する方法を発見することです。グループのメンバーは、通常、プロパティを共有しています。 これは、 [サービス](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) K 平均法、任意のデータをグループに、一般的に使用されるクラスタ リングの手法を使用します。 この Web サービスは、入力としてデータとクラスターの数 k を取得し、各観察がどの k グループに属するかの予測を生成します。 

>この Web サービスは、モバイル アプリ、Web サイト、ローカル コンピューターなどからユーザーが使用できます。 この Web サービスのもう 1 つの目的は、Azure Machine Learning を使用して R コード上に Web サービスを作成する方法の例を示すことです。 数行の R コードを記述し、Azure Machine Learning Studio 内でボタンを何回かクリックするだけで、R コードで実験を作成し、Web サービスとして発行できます。 この Web サービスは Azure Marketplace に発行され、Web サービスの作成者がインフラストラクチャを設定することなく、世界中のユーザーやデバイスで使用されます。  

##Web サービスの使用   
この Web サービスは、一連の k 個のグループにデータをグループ化し、行ごとにグループの割り当てを出力します。 Web サービスでは、エンド ユーザーが、行をコンマ (,) で区切り、列をセミコロン (;) で区切った文字列として、データを入力する必要があります。 Web サービスでは、一度に 1 行を入力します。 データセットの例は、次のようになります。

![サンプル データ][1]

ユーザーがこのデータを相互に排他的な 3 つのグループに分割するとします。 上のデータセットに次のデータを入力します。値 = “10;5;2,18;1;6,7;5;5,22;3;4,12;2;1,10;3;4”; k=”3”. 出力は、それぞれの行に対して予測されたグループ メンバーシップです。

>Azure Marketplace でホストされているこのサービスは、OData サービスです。これらは、POST や GET メソッドによって呼び出すことができます。 

自動でサービスの利用の複数の方法があります (アプリケーション例が [ここ](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx ))。

###Web サービスを使用する C# コードを開始します。

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




##Web サービスの作成  
>この Web サービスは、Azure Machine Learning を使用して作成されました。 無料評価版として入門ビデオ実験を作成し、 [web サービスを公開](machine-learning-publish-a-machine-learning-web-service.md), を参照してください [azure.com/ml](http://azure.com/ml)します。 Web サービスを作成した実験のスクリーン ショット、および実験内の各モジュールに対するコード例を以下に示します。

Azure Machine Learning 内で、新しい空白の実験が作成された 2 つと [R スクリプトの実行][execute-r-script] 、ワークスペース上に取得しました。 データ スキーマが簡単に作成された [R スクリプトの実行][execute-r-script]します。 次に、データ スキーマをやり直して作成、クラスター モデル セクションにリンク、 [R スクリプトの実行][execute-r-script]します。  [R スクリプトの実行][execute-r-script] クラスター モデルに使用する、web サービス、利用「k 平均」関数に事前に組み込まれており、 [R スクリプトの実行][execute-r-script] Azure Machine Learning のです。    
   

     
![実験フロー][3]

####モジュール 1: 
    #文字列として入力データを入力してください。 
    mydata <-data.frame (値 ="1 になります。3 です。5 です。6 です。7 です。7, 5 です。5 です。6 です。7 です。2 です。1、3 です。7 です。2 です。9 です。56 です。6, 1 になります。4;5 です。26 です。4;23, 15 です。35 です。6 です。7 です。12 です。1、32 です。51 です。62 です。7 です。21 です。1", k = 5, stringsAsFactors = FALSE)
    
    maml.mapOutputPort("mydata");     
    

####モジュール 2:
    # オプションの入力ポートを 1 から始まるを変数にマップします。
    mydata <-maml.mapInputPort(1) # クラス: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##制限事項
これは、Web サービスのクラスタリングのきわめて単純な例です。 上のコード例からわかるように、エラーのキャッチは実装されていません。このサービスでは、Web サービスの作成時に数値のみを入力するため、すべてが連続した変数 (カテゴリ別機能は使用できません) である必要があります。 また、サービスが処理できるデータ サイズは制限されています。これは、Web サービス呼び出しの要求/応答の性質と、Web サービスを呼び出すたびにモデルが適合されることによります。 

##FAQ
Web サービスまたは Azure Marketplace への発行の使用に関するよく寄せられる質問は、次を参照してください。 [ここ](machine-learning-marketplace-faq.md)します。

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 


