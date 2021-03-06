<properties 
	pageTitle="Analytics のトラブルシューティング - Application Insights の強力な検索ツール | Microsoft Azure" 
	description="Application Insights Analyticsで問題が発生しましたか? ここから開始します。" 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/07/2016" 
	ms.author="awills"/>


# Application Insights の Analytics のトラブルシューティング


[Application Insights Analytics](app-insights-analytics.md)で問題が発生しましたか? ここから開始します。Analytics は、Visual Studio Application Insights の強力な検索ツールです。



## 制限

* 現時点では、クエリの結果は過去 1 週間のデータだけに制限されます。
* テストを実施したブラウザーは、Chrome、Edge、および Internet Explorer の最新のエディションです。

## 「予期しないエラー」

![予期しないエラーの画面](./media/app-insights-analytics-troubleshooting/010.png)

ポータルの実行時に内部エラー (未処理の例外) が発生しました。

* ブラウザーのキャッシュを削除します。 

## 403 ... 再読み込みしてください

![403 \... 再読み込みしてください](./media/app-insights-analytics-troubleshooting/020.png)

認証に関連するエラーが (認証中またはアクセス トークンの生成中に) 発生しました。ポータルでは、ブラウザーの設定を変更しないと回復できない可能性があります。

* ブラウザーでサード パーティの Cookie が有効になっていることを確認します。 

    「[how to disable third party cookies (サード パーティの Cookie を無効にする方法)](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers)」を参照し、サード パーティの Cookie を**有効**にする必要があります。

## 403 ... セキュリティ ゾーンを確認します

![403 \... セキュリティ ゾーンを確認します](./media/app-insights-analytics-troubleshooting/030.png)

認証に関連するエラーが (認証中またはアクセス トークンの生成中に) 発生しました。ポータルでは、ブラウザーの設定を変更しないと回復できない可能性があります。

1. ブラウザーでサード パーティの Cookie が有効になっていることを確認します。 

    「[how to disable third party cookies (サード パーティの Cookie を無効にする方法)](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers)」を参照し、サード パーティの Cookie を**有効**にする必要があります。

2. お気に入り、ブックマーク、または保存したリンクを使用して Analytics ポータルを開きましたか? リンクを保存したときに使用した資格情報とは異なる資格情報でサインインしていますか?

 2. (このようなウィンドウをすべて閉じてから) InPrivate または Incognito のブラウザー ウィンドウを使用してみてください。資格情報を指定する必要があります。

 2. 別の (通常の) ブラウザー ウィンドウを開いて [Azure](https://portal.azure.com) に移動します。サインアウトします。次に、リンクを開き、適切な資格情報を使用してサインインします。

2. Edge および Internet Explorer のユーザーも、信頼済みゾーンの設定がサポートされていないと、このエラーが表示される場合があります。

	[Analytics ポータル](https://analytics.applicationinsights.io)と [Azure Active Directory ポータル](https://portal.azure.com)の両方が同じセキュリティ ゾーン内にあることを確認します。

 * Internet Explorer で、**[インターネット オプション]** 、**[セキュリティ]**、**[信頼済みサイト]**、**[サイト]** の順に開きます。

    ![信頼済みサイトにサイトを追加するときの [インターネット オプション] ダイアログ](./media/app-insights-analytics-troubleshooting/033.png)

    Web サイトの一覧で、次の URL のいずれかが含まれている場合、他の URL も追加します。

    * https://login.microsoftonline.com
    * https://login.windows.net
    * https://analytics.applicationinsights.io 


## 404 ...リソースが見つかりません

![404 \... リソースが見つかりません](./media/app-insights-analytics-troubleshooting/040.png)

アプリケーションのリソースが Application Insights から削除されており、使用できません。これは、Analytics ページへの URL を保存していると発生する場合があります。


## 403 ...アクセス権限がありません

![403 \... アクセス権限がありません](./media/app-insights-analytics-troubleshooting/050.png)

Analytics でこのアプリケーションを開くためのアクセス許可がありません。

* 他のユーザーからリンクを取得しましたか? 自分が[このリソース グループの閲覧者または投稿者](app-insights-resources-roles-access-control.md)に入っていることを確認するようにそのユーザーに依頼してください。
* 別の資格情報を使用してリンクを保存しましたか? [Azure ポータル](https://portal.azure.com)を開いてサインアウトしてから、適切な資格情報を指定して、もう一度このリンクを試してください。

## 403 ...HTML5 のストレージ

当社のポータルは、HTML5 localStorage および sessionStorage を使用します。

* Chrome: 設定、プライバシー、コンテンツの設定
* Internet Explorer: インターネット オプション、詳細設定タブ、セキュリティ、DOM ストレージを有効にする


![403 \... HTML5 のストレージを有効にしてください](./media/app-insights-analytics-troubleshooting/060.png)

## 404 ...サブスクリプションが見つかりません


![404 \...サブスクリプションが見つかりません](./media/app-insights-analytics-troubleshooting/070.png)

URL が無効です。

* [Application Insights ポータル](https://portal.azure.com)でアプリのリソースを開きます。次に、[分析] ボタンを使用します。

## 404 ... ページが存在しません

![404 \...ページが存在しません](./media/app-insights-analytics-troubleshooting/080.png)

URL が無効です。

* [Application Insights ポータル](https://portal.azure.com)でアプリのリソースを開きます。次に、[分析] ボタンを使用します。

## すべてうまくいかなかった場合    

[サポート チケット](app-insights-get-dev-support.md)を開いてください。
 
[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

<!---HONumber=AcomDC_0615_2016-->