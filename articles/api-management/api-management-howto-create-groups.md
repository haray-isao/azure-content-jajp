<properties 
	pageTitle="Azure API Management でグループを作成および使用して開発者アカウントを管理する方法" 
	description="Azure API Management でグループを使用して開発者アカウントを管理する方法について説明します。" 
	services="api-management" 
	documentationCenter="" 
	authors="steved0x" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="api-management" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/25/2016" 
	ms.author="sdanie"/>

# Azure API Management でグループを作成および使用して開発者アカウントを管理する方法

API Management では、開発者に成果物の表示を許可するかどうかが、グループを使用して管理されます。グループに対して成果物の表示が許可されると、そのグループに属する開発者は、グループに関連付けられた成果物を表示してサブスクライブできるようになります。

API Management には、次に示すシステム グループが用意されています。これらのシステム グループを変更することはできません。

-	**管理者** - Azure サブスクリプション管理者は、このグループのメンバーです。管理者は、API Management サービス インスタンスを管理します。開発者が使用する API とその操作、成果物は、管理者が作成します。
-	**開発者** - 認証された開発者ポータル ユーザーは、このグループに分類されます。開発者は、API の利用者です。管理者によって作成された API を使用してアプリケーションを構築します。開発者は、開発者ポータルへのアクセスが認められており、API の操作を呼び出すアプリケーションを構築します。
-	**ゲスト** - API Management インスタンスの開発者ポータルに訪れる開発者ポータル ユーザーのうち、認証を受けていないもの (利用予定者など) がこのグループに該当します。特定の読み取り専用アクセスを許可することができます (API の閲覧はできるが、呼び出すことはできないなど)。

管理者は、これらのシステム グループに加えてカスタム グループを作成できるほか、[関連付けられている Azure Active Directory テナントの外部グループを活用する][]こともできます。カスタム グループと外部グループをシステム グループと共に使用することにより、開発者には API 成果物の可視性とアクセスが提供されます。たとえば、特定のパートナー企業に所属する開発者向けにカスタム グループを 1 つ作成し、関連する API のみが含まれている成果物の API に対してアクセスを許可することができます。ユーザーは 複数のグループのメンバーになることができます。

このガイドでは、API Management インスタンスの管理者が新しいグループを追加して成果物および開発者に関連付ける方法について説明します。

>[AZURE.NOTE] 発行者ポータルでグループを作成および管理するだけでなく、API Management REST API [グループ](https://msdn.microsoft.com/library/azure/dn776329.aspx) エンティティを使用してグループを作成および管理することができます。

## <a name="create-group"> </a>グループの作成

新しいグループを作成するには、API Management サービスの Azure クラシック ポータルで **[管理]** をクリックします。API Management パブリッシャー ポータルが表示されます。

![パブリッシャー ポータル][api-management-management-console]

>まだ API Management サービス インスタンスを作成していない場合は、「[Azure API Management の使用][]」チュートリアルの「[API Management インスタンスの作成][]」をご覧ください。

左側の **[API Management]** メニューの **[グループ]** をクリックし、**[グループの追加]** をクリックします。

![新しいグループを追加する][api-management-add-group]

グループの一意の名前とオプションの説明を入力し、**[保存]** をクリックします。

![新しいグループを追加する][api-management-add-group-window]

新しいグループが [グループ] タブに表示されます。グループの **[名前]** または **[説明]** を編集するには、一覧内のグループの名前をクリックします。グループを削除するには、**[削除]** をクリックします。

![グループが追加された][api-management-new-group]

グループが作成されます。このグループは、成果物および開発者と関連付けることができます。

## <a name="associate-group-product"> </a>グループと成果物の関連付け

グループを成果物に関連付けるには、左側の **[API Management]** メニューの **[成果物]** をクリックし、目的の成果物の名前をクリックします。

![可視性の設定][api-management-add-group-to-product]

**[可視性]** タブを選択すると、グループを追加または削除したり、成果物の現在のグループを表示したりできます。グループを追加または削除するには、目的のグループのチェック ボックスをオンまたはオフにし、**[保存]** をクリックします。

![可視性の設定][api-management-add-group-to-product-visibility]

>[AZURE.NOTE] Azure Active Directory グループを追加するには、「[Azure API Management で Azure Active Directory を使用して開発者アカウントを認証する方法](api-management-howto-aad.md)」をご覧ください。
>
>**[可視性]** タブで成果物のグループを構成するには、**[グループの管理]** をクリックします。

成果物をグループに関連付けると、そのグループに属する開発者は、成果物を表示してサブスクライブすることができます。

## <a name="associate-group-developer"> </a>グループと開発者の関連付け

グループを開発者に関連付けるには、左側の **[API Management]** メニューの **[ユーザー]** をクリックし、グループに関連付ける開発者の横のチェック ボックスをオンにします。

![開発者をグループに追加する][api-management-add-group-to-developer]

目的の開発者を選択したら、**[グループに追加]** ボックスの一覧で目的のグループをクリックします。開発者をグループから削除するには、**[グループから削除]** ボックスを使用します。

![開発者][api-management-add-group-to-developer-saved]

開発者とグループの間に関連付けを追加すると、**[ユーザー]** タブにその関連付けが表示されるようになります。


## <a name="next-steps"> </a>次のステップ

-	開発者をグループに関連付けると、開発者は、グループに関連付けられた成果物を表示してサブスクライブすることができます。詳細については、「[Azure API Management で成果物を作成して発行する方法][]」をご覧ください。
-	発行者ポータルでグループを作成および管理するだけでなく、API Management REST API [グループ](https://msdn.microsoft.com/library/azure/dn776329.aspx) エンティティを使用してグループを作成および管理することができます。


[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[Azure API Management で成果物を作成して発行する方法]: api-management-howto-add-products.md

[API Management インスタンスの作成]: api-management-get-started.md
[Azure API Management の使用]: api-management-get-started.md#create-service-instance
[関連付けられている Azure Active Directory テナントの外部グループを活用する]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group

<!---HONumber=AcomDC_0525_2016-->