<properties
	pageTitle="How to define a custom API in a .NET backend mobile service | Azure Mobile Services"
	description="Learn how to define a custom API endpoint in a .NET backend mobile service."
	services="mobile-services"
	documentationCenter=""
	authors="ggailey777"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-multiple"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="02/07/2016"
	ms.author="glenga"/>


# How to: define a custom API endpoint in a .NET backend mobile service

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


> [AZURE.SELECTOR]
- [JavaScript backend](./mobile-services-javascript-backend-define-custom-api.md)
- [.NET backend](./mobile-services-dotnet-backend-define-custom-api.md)

This topic shows you how to define a custom API endpoint in a .NET backend mobile service. A custom API lets you define custom endpoints with server functionality, but it does not map to a database insert, update, delete, or read operation. By using a custom API, you have more control over messaging, including HTTP headers and body format.

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-custom-api](../../includes/mobile-services-dotnet-backend-create-custom-api.md)]

For information on how to invoke a custom API in your app using a Mobile Services client library, see [Call a custom API](mobile-services-windows-dotnet-how-to-use-client-library.md#custom-api) in the client SDK reference.


<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->

