<properties
pageTitle="Add the Office 365 Outlook API to your Logic Apps | Microsoft Azure"
description="Overview of Office 365 Outlook API with REST API parameters"
services=""	
documentationCenter="" 	
authors="msftman"	
manager="dwrede"	
editor="" tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="02/25/2016"
ms.author="mandia"/>

# Get started with the Office 365 Outlook API 

Connect to Office 365 Outlook to get email, reply to an email, update your calendar and contacts, and more. The Office 365 Outlook API can be used from:

- PowerApps 
- Logic apps 

>[AZURE.NOTE] This version of the article applies to logic apps 2015-08-01-preview schema version. For the 2014-12-01-preview schema version, click [Office 365 API](../app-service-logic/app-service-logic-connector-office365.md).

With Office 365 Outlook, you can:

- Build your business flow based on the data you get from Office 365 Outlook. 
- Use a trigger when there is a new email, when you create a new contact, and more.
- Use actions that reply to an email, create a new calendar event, and more. These actions get a response, and then make the output available for other actions. For example, when there is a new object in Salesforce, you can take that object and update your Office 365 Outlook contacts. 
- Add the Office 365 Outlook API to PowerApps Enterprise. Then, your users can use this API within their apps. 

For information on how to add an API in PowerApps Enterprise, go to [Register an API in PowerApps](../power-apps/powerapps-register-from-available-apis.md). 

To add an operation in logic apps, see [Create a logic app](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Triggers and actions

The Office 365 Outlook API has the following triggers and actions available. 

| Triggers | Actions|
| --- | --- |
|<ul><li>On event starting soon</li><li>On new email</li><li>On new items</li><li>On updated items</li></ul>| <ul><li>Create contact</li><li>Create event</li><li>Send approval email</li><li>Send email</li><li>Delete contact</li><li>Delete email</li><li>Delete event</li><li>Get attachment</li><li>Get calendars</li><li>Get contact</li><li>Get contact folders</li><li>Get contacts</li><li>Get emails</li><li>Get event</li><li>Get events</li><li>Mark as read</li><li>On event starting soon</li><li>On new email</li><li>On new items</li><li>On updated items</li><li>Reply to message</li><li>Send email with options</li><li>Update contact</li><li>Update event</li></ul> |

All APIs support data in JSON and XML formats. 


## Create a connection to Office365

### Add additional configuration in PowerApps
When you add this API to PowerApps Enterprise, you enter the **App Key** and **App Secret** values of your Office 365 Azure Active Directory (AAD) application. The **Redirect URL** value is also used in your Office 365 application. If you don't have an Office 365 application, you can use the following steps to create the application: 

1. In the [Azure Portal][5], open **Active Directory**, and open your organization's tenant name.
2. Select the **Applications** tab, and select **Add**:  
![AAD tenant applications][7]

3. In **Add application**:  

	1. Enter a **Name** for your application.  
	2. Leave the application type as **Web**.  
	3. Select **Next**.  

	![Add AAD application - app info][8]

6. In **App Properties**:  

	1. Enter the **SIGN-ON URL** of your application. Since you are going to authenticate with AAD for PowerApps, set the sign-on url to _https://login.windows.net_.  
	2. Enter a valid **APP ID URI** for your app.  
	3. Select **OK**.  

	![Add AAD application - app properties][9]

7. When complete, the new AAD app opens. Select **Configure**:  
![Contoso AAD app][10]

8. Under the _OAuth 2_ section, set the **Reply URL**  to the redirect URL value shown when you added the Office 365 Outlook API in the Azure Portal. Select **Add application**:  
![Configure Contoso AAD app][11]

9. In **Permissions to other applications**, select **Office 365 Exchange Online**, and select **OK**:  
![Contoso app delegate][12]

	Back in the configure page, note that _Office 365 Exchange Online_ is added to the _Permission to other applications_ list.

10. For **Office 365 Exchange Online**, select **Delegated Permissions** , and select the following permissions:  

	- Read and write user contacts
	- Read user contacts
	- Read and write user calendars
	- Read user calendars
	- Send mail as a user
	- Read and write user mail
	- Read user mail

	![Contoso app delegate permissions][13]

A new Azure Active Directory app is created. You can copy/paste the **App Key** and **App Secret** values into your Office 365 Outlook API configuration in the Azure portal. 

Some good info on AAD applications at [How and why applications are added to Azure AD](../active-directory/active-directory-how-applications-are-added.md).

### Add additional configuration in logic apps
When you add this API to your logic apps, you must sign-in to your Office 365 Outlook account and allow logic apps to connect to your account.

1. Sign in to your Office 365 Outlook account account.
2. Allow your logic apps to connect and use your Office 365 account. 

After you create the connection, you enter the Office 365 Outlook properties, like the inbox folder path or email message. The **REST API reference** in this topic describes these properties.

>[AZURE.TIP] You can use this same Office 365 Outlook connection in other logic apps.

## Swagger REST API reference
Applies to version: 1.0.


### On event starting soon 
Triggers a flow when an upcoming calendar event is starting.  
```GET: /Events/OnUpcomingEvents``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|query|none|Unique identifier of the calendar|
|lookAheadTimeInMinutes|integer|no|query|15|Time (in minutes) to look ahead for upcoming events.|

#### Response
|Name|Description|
|---|---|
|200|Operation was successful|
|202|Operation was successful|
|400|BadRequest|
|401|Unauthorized|
|403|Forbidden|
|500|Internal Server Error|
|default|Operation Failed.|


### Get emails 
Retrieves emails from a folder.  
```GET: /Mail```

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|folderPath|string|no|query|Inbox|Path of the folder to retrieve messages (default: 'Inbox')|
|top|integer|no|query|10|Number of emails to retrieve (default: 10)|
|fetchOnlyUnread|boolean|no|query|true|Retrieve only unread messages?|
|includeAttachments|boolean|no|query|false|If set to true, attachments will also be retrieved along with the email message.|
|searchQuery|string|no|query|none|Search query to filter emails|
|skip|integer|no|query|0|Number of emails to skip (default: 0)|
|skipToken|string|no|query|none|Skip token to fetch new page|

#### Response

|Name|Description|
|---|---|
|200|Operation was successful|
|400|BadRequest|
|401|Unauthorized|
|403|Forbidden|
|500|Internal Server Error|
|default|Operation Failed.|


### Send Email 
Sends an email message.  
```POST: /Mail``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|emailMessage| |yes|body|none|Email message instance|


#### Response

|Name|Description|
|---|---|
|200|Operation was successful|
|400|BadRequest|
|401|Unauthorized|
|403|Forbidden|
|500|Internal Server Error|
|default|Operation Failed.|


### Delete email 
Deletes an email message by id.  
```DELETE: /Mail/{messageId}``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|messageId|string|yes|path|none|Id of the message to delete.|

#### Response

|Name|Description|
|---|---|
|200|Operation was successful|
|400|BadRequest|
|401|Unauthorized|
|403|Forbidden|
|500|Internal Server Error|
|default|Operation Failed.|


### Mark as read 
Marks an email message as having been read.  
```POST: /Mail/MarkAsRead/{messageId}``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|messageId|string|yes|path|none|Id of the message to be marked as read|

#### Response

|Name|Description|
|---|---|
|200|Operation was successful|
|400|BadRequest|
|401|Unauthorized|
|403|Forbidden|
|500|Internal Server Error|
|default|Operation Failed.|


### Reply to message 
Replies to an email message.  
```POST: /Mail/ReplyTo/{messageId}``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|messageId|string|yes|path|none|Id of the message to reply to|
|comment|string|yes|query|none|Reply comment|
|replyAll|boolean|no|query|false|Reply to all recipients|

#### Response

|Name|Description|
|---|---|
|200|Operation was successful|
|400|BadRequest|
|401|Unauthorized|
|403|Forbidden|
|500|Internal Server Error|
|default|Operation Failed.|


### Get attachment 
Retrieves message attachment by id.  
```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|messageId|string|yes|path|none|Id of the message|
|attachmentId|string|yes|path|none|Id of the attachment to download|

#### Response

|Name|Description|
|---|---|
|200|Operation was successful|
|400|BadRequest|
|401|Unauthorized|
|403|Forbidden|
|500|Internal Server Error|
|default|Operation Failed.|


### On new email 
Triggers a flow when a new email arrives.  
```GET: /Mail/OnNewEmail``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|folderPath|string|no|query|Inbox|Email folder to retrieve (default: Inbox)|
|to|string|no|query|none|Recipient email addresses|
|from|string|no|query|none|From address|
|importance|string|no|query|Normal|Importance of the email (High, Normal, Low) (default: Normal)|
|fetchOnlyWithAttachment|boolean|no|query|false|Retrieve only emails with an attachment|
|includeAttachments|boolean|no|query|false|Include attachments|
|subjectFilter|string|no|query|none|String to look for in the subject.|

#### Response

|Name|Description|
|---|---|
|200|Operation was successful.|
|202|Accepted|
|400|BadRequest|
|401|Unauthorized|
|403|Forbidden|
|500|Internal Server Error|
|default|Operation Failed.|


### Send email with options 
Send an email with multiple options and wait for the recipient to respond back with one of the options.  
```POST: /mailwithoptions/$subscriptions``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |yes|body|none|Subscription Request for options Email|

#### Response

|Name|Description|
|---|---|
|200|OK|
|201|Subscription Created|
|400|BadRequest|
|401|Unauthorized|
|403|Forbidden|
|500|Internal Server Error|
|default|Operation Failed.|


### Send approval email 
Send an approval email and wait for a response from the To recipient.  
```POST: /approvalmail/$subscriptions``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |yes|body|none|Subscription Request for Approval Email.|


#### Response

|Name|Description|
|---|---|
|200|OK|
|201|Subscription Created|
|400|BadRequest|
|401|Unauthorized|
|403|Forbidden|
|500|Internal Server Error|
|default|Operation Failed.|




### Get calendars 
Retrieves calendars.  
```GET: /datasets/calendars/tables``` 

There are no parameters for this call.

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|




### Get events 
Retrieves items from a calendar.  
```GET: /datasets/calendars/tables/{table}/items```

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of the calendar to retrieve|
|$skip|integer|no|query|none|Number of entries to skip (default = 0)|
|$top|integer|no|query|none|Maximum number of entries to retrieve (default = 256)|
|$filter|string|no|query|none|An ODATA filter query to restrict the number of entries|
|$orderby|string|no|query|none|An ODATA orderBy query for specifying the order of entries|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### Create event 
Creates a new event.  
```POST: /datasets/calendars/tables/{table}/items``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of a calendar|
|item| |yes|body|none|Calendar item to create|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### Get event 
Retrieves a specific item from a calendar.  
```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of a calendar|
|id|string|yes|path|none|Unique identifier of a calendar item to retrieve|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### Delete event 
Deletes a calendar item.  
```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of a calendar.|
|id|string|yes|path|none|Unique identifier of calendar item to delete|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### Update event 
Partially updates a calendar item.  
```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of a calendar|
|id|string|yes|path|none|Unique identifier of calendar item to update|
|item| |yes|body|none|Calendar item to update|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### On new items 
Triggered when a new calendar item is created.  
```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of a calendar|
|$skip|integer|no|query|none|Number of entries to skip (default = 0)|
|$top|integer|no|query|none|Maximum number of entries to retrieve (default = 256)|
|$filter|string|no|query|none|An ODATA filter query to restrict the number of entries|
|$orderby|string|no|query|none|An ODATA orderBy query for specifying the order of entries|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### On updated items 
Triggered when a calendar item is modified.  
```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of a calendar|
|$skip|integer|no|query|none|Number of entries to skip (default = 0)|
|$top|integer|no|query|none|Maximum number of entries to retrieve (default = 256)|
|$filter|string|no|query|none|An ODATA filter query to restrict the number of entries|
|$orderby|string|no|query|none|An ODATA orderBy query for specifying the order of entries|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### Get contact folders 
Retrieves contacts folders.  
```GET: /datasets/contacts/tables``` 

There are no parameters for this call.

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### Get contacts 
Retrieves contacts from a contacts folder.  
```GET: /datasets/contacts/tables/{table}/items```

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of the contacts folder to retrieve|
|$skip|integer|no|query|none|Number of entries to skip (default = 0)|
|$top|integer|no|query|none|Maximum number of entries to retrieve (default = 256)|
|$filter|string|no|query|none|An ODATA filter query to restrict the number of entries|
|$orderby|string|no|query|none|An ODATA orderBy query for specifying the order of entries|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### Create contact 
Creates a new contact.  
```POST: /datasets/contacts/tables/{table}/items``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of a contacts folder|
|item| |yes|body|none|Contact to create|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### Get contact 
Retrieves a specific contact from a contacts folder.  
```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of a contacts folder|
|id|string|yes|path|none|Unique identifier of a contact to retrieve|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### Delete contact 
Deletes a contact.  
```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of a contacts folder.|
|id|string|yes|path|none|Unique identifier of contact to delete|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


### Update contact 
Partially updates a contact.  
```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| Name| Data Type|Required|Located In|Default Value|Description|
| ---|---|---|---|---|---|
|table|string|yes|path|none|Unique identifier of a contacts folder|
|id|string|yes|path|none|Unique identifier of contact to update|
|item| |yes|body|none|Contact item to update|

#### Response

|Name|Description|
|---|---|
|200|OK|
|default|Operation Failed.|


## Object definitions

#### TriggerBatchResponse[IDictionary[String,Object]]

| Name | Data Type |Required|
|---|---|---|
|value|array|no|


#### SendMessage: Send Email Message

| Name | Data Type |Required|
|---|---|---|
|Attachments|array|no|
|From|string|no|
|Cc|string|no|
|Bcc|string|no|
|Subject|string|yes|
|Body|string|yes|
|Importance|string|no|
|IsHtml|boolean|no|
|To|string|yes|

#### SendAttachment: Attachment

| Name | Data Type |Required|
|---|---|---|
|@odata.type|string|no|
|Name|string|yes|
|ContentBytes|string|yes|


#### ReceiveMessage: Receive Email Message

| Name | Data Type |Required|
|---|---|---|
|Id|string|no|
|IsRead|boolean|no|
|HasAttachment|boolean|no|
|DateTimeReceived|string|no|
|Attachments|array|no|
|From|string|no|
|Cc|string|no|
|Bcc|string|no|
|Subject|string|yes|
|Body|string|yes|
|Importance|string|no|
|IsHtml|boolean|no|
|To|string|yes|


#### ReceiveAttachment: Receive Attachment

| Name | Data Type |Required|
|---|---|---|
|Id|string|yes|
|ContentType|string|yes|
|@odata.type|string|no|
|Name|string|no|
|ContentBytes|string|yes|


#### DigestMessage: Send Email Message

| Name | Data Type |Required|
|---|---|---|
|Subject|string|yes|
|Body|string|no|
|Importance|string|no|
|Digest|array|yes|
|Attachments|array|no|
|To|string|yes|

#### TriggerBatchResponse[ReceiveMessage]

| Name | Data Type |Required|
|---|---|---|
|value|array|no|


#### DataSetsMetadata

| Name | Data Type |Required|
|---|---|---|
|tabular|not defined|no|
|blob|not defined|no|


#### TabularDataSetsMetadata

| Name | Data Type |Required|
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|
|tableDisplayName|string|no|
|tablePluralName|string|no|


#### BlobDataSetsMetadata

| Name | Data Type |Required|
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|


#### TableMetadata

| Name | Data Type |Required|
|---|---|---|
|name|string|no|
|title|string|no|
|x-ms-permission|string|no|
|schema|not defined|no|


#### OptionsEmailSubscription: Model for Options Email Subscription

| Name | Data Type |Required|
|---|---|---|
|NotificationUrl|string|no|
|Message|not defined|no|

#### MessageWithOptions: User Options Email Message. This is the message expected as part of user input

| Name | Data Type |Required|
|---|---|---|
|Subject|string|yes|
|Options|string|yes|
|Body|string|no|
|Importance|string|no|
|Attachments|array|no|
|To|string|yes|

#### SubscriptionResponse: Model for Approval Email Subscription

| Name | Data Type |Required|
|---|---|---|
|id|string|no|
|resource|string|no|
|notificationType|string|no|
|notificationUrl|string|no|


#### ApprovalEmailSubscription: Model for Approval Email Subscription

| Name | Data Type |Required|
|---|---|---|
|NotificationUrl|string|no|
|Message|not defined|no|


#### ApprovalMessage: Approval Email Message. This is the message expected as part of user input

| Name | Data Type |Required|
|---|---|---|
|Subject|string|yes|
|Options|string|yes|
|Body|string|no|
|Importance|string|no|
|Attachments|array|no|
|To|string|yes|

#### ApprovalEmailResponse: Approval Email Response

| Name | Data Type |Required|
|---|---|---|
|SelectedOption|string|no|

#### TablesList

| Name | Data Type |Required|
|---|---|---|
|value|array|no|


#### Table

| Name | Data Type |Required|
|---|---|---|
|Name|string|no|
|DisplayName|string|no|


#### Item

| Name | Data Type |Required|
|---|---|---|
|ItemInternalId|string|no|


#### CalendarItemsList: The list of calendar items

| Name | Data Type |Required|
|---|---|---|
|value|array|no|


#### CalendarItem: Represents a calendar table item

| Name | Data Type |Required|
|---|---|---|
|ItemInternalId|string|no|


#### ContactItemsList: The list of contact items

| Name | Data Type |Required|
|---|---|---|
|value|array|no|


#### ContactItem: Represents a contact table item

| Name | Data Type |Required|
|---|---|---|
|ItemInternalId|string|no|


#### DataSetsList

| Name | Data Type |Required|
|---|---|---|
|value|array|no|


#### DataSet

| Name | Data Type | Required|
|---|---|---|
|Name|string|no|
|DisplayName|string|no|


## Next Steps
After you add the Office 365 API to PowerApps Enterprise, [give users permissions](../power-apps/powerapps-manage-api-connection-user-access.md) to use the API in their apps.

[Create a logic app](../app-service-logic/app-service-logic-create-a-logic-app.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/create-api-office365-outlook/aad-tenant-applications.png
[8]: ./media/create-api-office365-outlook/aad-tenant-applications-add-appinfo.png
[9]: ./media/create-api-office365-outlook/aad-tenant-applications-add-app-properties.png
[10]: ./media/create-api-office365-outlook/contoso-aad-app.png
[11]: ./media/create-api-office365-outlook/contoso-aad-app-configure.png
[12]: ./media/create-api-office365-outlook/contoso-aad-app-delegate-office365-outlook.png
[13]: ./media/create-api-office365-outlook/contoso-aad-app-delegate-office365-outlook-permissions.png
