<properties
	pageTitle="Azure AD SAML のプロトコル リファレンス | Microsoft Azure"
	description="この記事では、Azure Active Directory のシングル サインオンおよびシングル サインアウト SAML プロファイルの概要を示します。"
	services="active-directory"
	documentationCenter=".net"
	authors="priyamohanram"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/31/2016"
	ms.author="priyamo"/>


# Azure Active Directory での SAML プロトコルの使用方法

[AZURE.INCLUDE [active-directory-protocols](../../includes/active-directory-protocols.md)]


Azure Active Directory (Azure AD) は SAML 2.0 プロトコルを使用して、アプリケーションでユーザーにシングル サインオン エクスペリエンスを提供できるようにします。Azure AD の[シングル サインオン](active-directory-single-sign-on-protocol-reference.md)と[シングル サインアウト](active-directory-single-sign-out-protocol-reference.md) SAML プロファイルには、ID プロバイダー サービスでの SAML アサーション、プロトコルおよびバインドの使用方法について説明されています。

SAML プロトコルで、ID プロバイダー (Azure AD) とサービス プロバイダー (アプリケーション) に関する情報を交換する必要があります。

アプリケーションが Azure AD に登録されると、アプリ開発者は Azure AD にフェデレーションに関する情報を登録します。これには、アプリケーションの**リダイレクト URI** と**メタデータ URI** が含まれます。

Azure AD ではクラウド サービスの**メタデータ URI** を使用して、クラウド サービスの署名キーとログアウト URI を取得します。アプリケーションでメタデータ URI がサポートされていない場合、開発者は Microsoft サポートに連絡してログアウト URI と署名キーを提供する必要があります。

Azure Active Directory は、テナント固有および共通 (テナント独立) のシングル サインオンとシングル サインアウト エンドポイントを公開します。これらの URL は、単なる ID ではなくアドレス指定可能な場所を表します。したがって、ユーザーはエンドポイントに移動してメタデータを読み取ることができます。

 - テナント固有のエンドポイントは、`https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml` にあります。<TenantDomainName> プレース ホルダーは、Azure AD テナントの登録済みドメイン名または TenantID GUID を表します。たとえば、contoso.com テナントのフェデレーション メタデータは、https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml にあります。

- テナント独立のエンドポイントは `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml` にあります。このエンドポイント アドレスの **common** は、テナントのドメイン名または ID の代わりに表示されます。

Azure AD で発行されるフェデレーション メタデータ ドキュメントについては、「[フェデレーション メタデータ](active-directory-federation-metadata.md)」を参照してください。

<!---HONumber=AcomDC_0608_2016-->