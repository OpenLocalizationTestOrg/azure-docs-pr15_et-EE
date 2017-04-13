<properties
pageTitle="MailChimp | Microsoft Azure'i"
description="Loogika rakenduste teenusega Azure'i rakenduse loomisel. MailChimp on SaaS teenus, mis võimaldab ettevõtetel haldamiseks ja e-posti turunduskampaaniate, sh turundus e-kirju, automatiseeritud sõnumeid ja sihtarvutites kampaaniat saatmise automatiseerimiseks."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-mailchimp-connector"></a>Alustamine MailChimp konnektor

MailChimp on SaaS teenus, mis võimaldab ettevõtetel haldamiseks ja e-posti turundustegevusi, sh turundus e-kirju, automatiseeritud sõnumeid ja suunatud kampaaniat saatmise automatiseerimiseks.


>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2015-08-01-eelvaade skeemi versioon.

Saate alustada, luues loogika rakenduse kohe leiate [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Päästikute ja toimingud

MailChimp konnektor saab kasutada toimingu; See on trigger(s). Kõik konnektorid toetavad andmete JSON ja XML-vormingus.

 MailChimp konnektor on järgmised toimingud ja/või trigger(s) saadaval.

### <a name="mailchimp-actions"></a>MailChimp toimingud
Tehke järgmised toimingud:

|Toiming|Kirjeldus|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|Looge uus turunduskampaania, mis põhineb turunduskampaania tüüp, adressaatide loend ja turunduskampaania sätted (teemareal, pealkirja, from_name ja reply_to)|
|[newlist](connectors-create-api-mailchimp.md#newlist)|Teie konto MailChimp uue loendi loomine|
|[addmember](connectors-create-api-mailchimp.md#addmember)|Lisage või värskendage liikme loend|
|[removemember](connectors-create-api-mailchimp.md#removemember)|Liikme kustutamiseks.|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|Kindla loendi liikme teabe värskendamine|
### <a name="mailchimp-triggers"></a>Päästikute MailChimp
Saate kuulata nende sündmus (ed) jaoks:

|Päästik | Kirjeldus|
|--- | ---|
|Kui loendisse on lisatud liikme|Käivitab töövoo, kui loendisse on lisatud uue liikme|
|Uue loendi loomise|Käivitab töövoo, kui luuakse uus loend|


## <a name="create-a-connection-to-mailchimp"></a>MailChimp ühenduse loomine
Loogika rakenduste MailChimp loomiseks peate esmalt **ühenduse** loomine siis üksikasjad pakuvad järgmised atribuudid:

|Atribuut| Nõutav|Kirjeldus|
| ---|---|---|
|Turbeloa|Jah|MailChimp mandaat|

>[AZURE.INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] Saate selle ühenduse loogika muudes rakendustes.

## <a name="reference-for-mailchimp"></a>MailChimp tutvustus
Kehtib versioon: 1.0

## <a name="newcampaign"></a>newcampaign
Uue turunduskampaania: Luua uut kampaaniat turunduskampaania tüüp, adressaatide loend ja turunduskampaania sätted (teemareal, pealkirja, from_name ja reply_to)

```POST: /campaigns```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|newCampaignRequest| |Jah|sisu|ükski|JSON objekti kehas parameetritega uue turunduskampaania kutse saatmine|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="newlist"></a>newlist
Uue loendi: MailChimp konto uue loendi loomine

```POST: /lists```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|newListRequest| |Jah|sisu|ükski|JSON objekti kehas parameetritega uue turunduskampaania kutse saatmine|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="addmember"></a>addmember
Liikme lisamiseks loendisse: lisage või värskendage liikme loend

```POST: /lists/{list_id}/members```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|string|Jah|tee|ükski|Loendi ainuidentifikaator|
|newMemberInList| |Jah|sisu|ükski|JSON objekti kehas uue liikme teabe saatmiseks|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="removemember"></a>removemember
Liikme eemaldamine loendist: liikme kustutamine loendist.

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|string|Jah|tee|ükski|Loendi ainuidentifikaator|
|member_email|string|Jah|tee|ükski|Liikme kustutamiseks meiliaadress|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="updatemember"></a>updatemember
Liikme teabe värskendamine: konkreetsesse nimekirja liikme teabe värskendamine

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|string|Jah|tee|ükski|Loendi ainuidentifikaator|
|member_email|string|Jah|tee|ükski|Liikme värskendamiseks kordumatu meiliaadress|
|updateMemberInListRequest| |Jah|sisu|ükski|JSON objekti saatmine teabega värskendatud sisu|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="onmembersubscribed"></a>OnMemberSubscribed
Kui loendisse on lisatud liikme: käivitab töövoo, kui loendisse on lisatud uue liikme

```GET: /trigger/lists/{list_id}/members```

| Nimi| Andmetüüp|Nõutav|Asub|Vaikeväärtus|Kirjeldus|
| ---|---|---|---|---|---|
|list_id|string|Jah|tee|ükski|Loendi ainuidentifikaator|

#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|202|Aktsepteeritud|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="oncreatelist"></a>OnCreateList
Uue loendi loomise: käivitab töövoo, kui luuakse uus loend

```GET: /trigger/lists```

Pole selle kõne parameetrid
#### <a name="response"></a>Vastus

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|202|Aktsepteeritud|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="object-definitions"></a>Objekti määratlused

### <a name="newcampaignrequest"></a>NewCampaignRequest


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|tüüp|string|Jah |
|adressaadid|määratletud ei|Jah |
|sätted|määratletud ei|Jah |
|variate_settings|määratletud ei|Ei |
|jälgimine|määratletud ei|Ei |
|rss_opts|määratletud ei|Ei |
|social_card|määratletud ei|Ei |



### <a name="recipient"></a>Adressaadi


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|list_id|string|Jah |
|segment_opts|määratletud ei|Ei |



### <a name="settings"></a>Sätted


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|subject_line|string|Jah |
|pealkiri|string|Ei |
|from_name|string|Jah |
|reply_to|string|Jah |
|use_conversation|kahendmuutuja|Ei |
|to_name|string|Ei |
|folder_id|täisarv|Ei |
|autentida|kahendmuutuja|Ei |
|auto_footer|kahendmuutuja|Ei |
|inline_css|kahendmuutuja|Ei |
|auto_tweet|kahendmuutuja|Ei |
|auto_fb_post|massiiv|Ei |
|fb_comments|kahendmuutuja|Ei |



### <a name="variatesettings"></a>Variate_Settings


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|winner_criteria|string|Ei |
|wait_time|täisarv|Ei |
|test_size|täisarv|Ei |
|subject_lines|massiiv|Ei |
|send_times|massiiv|Ei |
|from_names|massiiv|Ei |
|reply_to_addresses|massiiv|Ei |



### <a name="tracking"></a>Jälgimine


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|avaneb|kahendmuutuja|Ei |
|html_clicks|kahendmuutuja|Ei |
|text_clicks|kahendmuutuja|Ei |
|goal_tracking|kahendmuutuja|Ei |
|ecomm360|kahendmuutuja|Ei |
|google_analytics|string|Ei |
|clicktale|string|Ei |
|salesforce'i|määratletud ei|Ei |
|kõrghoone|määratletud ei|Ei |
|kapsel|määratletud ei|Ei |



### <a name="rssopts"></a>RSS_Opts


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|feed_url|string|Ei |
|Frequency|string|Ei |
|constrain_rss_img|string|Ei |
|ajakava|määratletud ei|Ei |



### <a name="socialcard"></a>Social_Card


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|image_url|string|Ei |
|kirjeldus|string|Ei |
|pealkiri|string|Ei |



### <a name="segmentopts"></a>Segment_Opts


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|saved_segment_id|täisarv|Ei |
|Match|string|Ei |



### <a name="salesforce"></a>Salesforce'i


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|turunduskampaania|kahendmuutuja|Ei |
|märkmete|kahendmuutuja|Ei |



### <a name="highrise"></a>Kõrghoone


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|turunduskampaania|kahendmuutuja|Ei |
|märkmete|kahendmuutuja|Ei |



### <a name="capsule"></a>Kapsel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|märkmete|kahendmuutuja|Ei |



### <a name="schedule"></a>Ajakava


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Hour|täisarv|Ei |
|daily_send|määratletud ei|Ei |
|weekly_send_day|string|Ei |
|monthly_send_date|arv|Ei |



### <a name="dailysend"></a>Daily_Send


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Pühapäev|kahendmuutuja|Ei |
|Esmaspäev|kahendmuutuja|Ei |
|Teisipäev|kahendmuutuja|Ei |
|Kolmapäev|kahendmuutuja|Ei |
|Neljapäev|kahendmuutuja|Ei |
|Reede|kahendmuutuja|Ei |
|Laupäev|kahendmuutuja|Ei |



### <a name="campaignresponsemodel"></a>CampaignResponseModel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|string|Ei |
|tüüp|string|Ei |
|create_time|string|Ei |
|archive_url|string|Ei |
|olek|string|Ei |
|emails_sent|täisarv|Ei |
|send_time|string|Ei |
|content_type|string|Ei |
|adressaadi|massiiv|Ei |
|sätted|määratletud ei|Ei |
|variate_settings|määratletud ei|Ei |
|jälgimine|määratletud ei|Ei |
|rss_opts|määratletud ei|Ei |
|ab_split_opts|määratletud ei|Ei |
|social_card|määratletud ei|Ei |
|report_summary|määratletud ei|Ei |
|delivery_status|määratletud ei|Ei |
|_links|massiiv|Ei |



### <a name="absplitopts"></a>AB_Split_Opts


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|split_test|string|Ei |
|pick_winner|string|Ei |
|wait_units|string|Ei |
|wait_time|täisarv|Ei |
|split_size|täisarv|Ei |
|from_name_a|string|Ei |
|from_name_b|string|Ei |
|reply_email_a|string|Ei |
|reply_email_b|string|Ei |
|subject_a|string|Ei |
|subject_b|string|Ei |
|send_time_a|string|Ei |
|send_time_b|string|Ei |
|send_time_winner|string|Ei |



### <a name="reportsummary"></a>Report_Summary


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|avaneb|täisarv|Ei |
|unique_opens|täisarv|Ei |
|open_rate|arv|Ei |
|hiireklõpsuga|täisarv|Ei |
|subscriber_clicks|arv|Ei |
|click_rate|arv|Ei |



### <a name="deliverystatus"></a>Delivery_Status


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|lubatud|kahendmuutuja|Ei |
|can_cancel|kahendmuutuja|Ei |
|olek|string|Ei |
|emails_sent|täisarv|Ei |
|emails_canceled|täisarv|Ei |



### <a name="link"></a>Link


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|rel|string|Ei |
|href|string|Ei |
|meetod|string|Ei |
|targetSchema|string|Ei |
|skeemi|string|Ei |



### <a name="newlistrequest"></a>NewListRequest


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|Nimi|string|Jah |
|kontakti|määratletud ei|Jah |
|permission_reminder|string|Jah |
|use_archive_bar|kahendmuutuja|Ei |
|campaign_defaults|määratletud ei|Jah |
|notify_on_subscribe|string|Ei |
|notify_on_unsubscribe|string|Ei |
|email_type_option|kahendmuutuja|Jah |
|nähtavuse|string|Ei |



### <a name="contact"></a>Kontakti


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ettevõtte|string|Jah |
|address1|string|Jah |
|Aadress2|string|Ei |
|linna|string|Jah |
|olek|string|Jah |
|ZIP|string|Jah |
|riik|string|Jah |
|telefoni|string|Jah |



### <a name="campaigndefaults"></a>Campaign_Defaults


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|from_name|string|Jah |
|from_email|string|Jah |
|Teema|string|Ei |
|keel|string|Jah |



### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|string|Jah |
|Nimi|string|Jah |
|kontakti|määratletud ei|Jah |
|permission_reminder|string|Jah |
|use_archive_bar|kahendmuutuja|Ei |
|campaign_defaults|määratletud ei|Jah |
|notify_on_subscribe|string|Ei |
|notify_on_unsubscribe|string|Ei |
|date_created|string|Ei |
|list_rating|täisarv|Ei |
|email_type_option|kahendmuutuja|Jah |
|subscribe_url_short|string|Ei |
|subscribe_url_long|string|Ei |
|beamer_address|string|Ei |
|nähtavuse|string|Ei |
|moodulid|massiiv|Ei |
|statistika|määratletud ei|Ei |
|_links|massiiv|Ei |



### <a name="stats"></a>Statistika


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|member_count|täisarv|Ei |
|unsubscribe_count|täisarv|Ei |
|cleaned_count|täisarv|Ei |
|member_count_since_send|täisarv|Ei |
|unsubscribe_count_since_send|täisarv|Ei |
|cleaned_count_since_send|täisarv|Ei |
|campaign_count|täisarv|Ei |
|campaign_last_sent|täisarv|Ei |
|merge_field_count|täisarv|Ei |
|avg_sub_rate|arv|Ei |
|avg_unsub_rate|arv|Ei |
|target_sub_rate|arv|Ei |
|open_rate|arv|Ei |
|click_rate|arv|Ei |
|last_sub_date|string|Ei |
|last_unsub_date|string|Ei |



### <a name="getlistsresponsemodel"></a>GetListsResponseModel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|loendid|massiiv|Ei |
|total_items|täisarv|Ei |



### <a name="newmemberinlistrequest"></a>NewMemberInListRequest


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|email_type|string|Ei |
|olek|string|Jah |
|merge_fields|määratletud ei|Ei |
|huvide|string|Ei |
|keel|string|Ei |
|VIP|kahendmuutuja|Ei |
|asukoht|määratletud ei|Ei |
|email_address|string|Jah |



### <a name="firstandlastname"></a>FirstAndLastName


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|FNAME|string|Ei |
|LNAME|string|Ei |



### <a name="location"></a>Asukoht


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|laiuskraad|arv|Ei |
|laiuskraadide|arv|Ei |



### <a name="memberresponsemodel"></a>MemberResponseModel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|string|Ei |
|email_address|string|Ei |
|unique_email_id|string|Ei |
|email_type|string|Ei |
|olek|string|Ei |
|merge_fields|määratletud ei|Ei |
|huvide|string|Ei |
|statistika|määratletud ei|Ei |
|ip_signup|string|Ei |
|timestamp_signup|string|Ei |
|ip_opt|string|Ei |
|timestamp_opt|string|Ei |
|member_rating|täisarv|Ei |
|last_changed|string|Ei |
|keel|string|Ei |
|VIP|kahendmuutuja|Ei |
|email_client|string|Ei |
|asukoht|määratletud ei|Ei |
|last_note|määratletud ei|Ei |
|list_id|string|Ei |
|_links|massiiv|Ei |



### <a name="lastnote"></a>Last_Note


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|note_id|täisarv|Ei |
|created_at|string|Ei |
|created_by|string|Ei |
|Märkus|string|Ei |



### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|liikmed|massiiv|Ei |
|list_id|string|Ei |
|total_items|täisarv|Ei |



### <a name="object"></a>Objekti






### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|email_address|string|Ei |
|email_type|string|Ei |
|olek|string|Jah |
|merge_fields|määratletud ei|Ei |
|huvide|string|Ei |
|keel|string|Ei |
|VIP|kahendmuutuja|Ei |
|asukoht|määratletud ei|Ei |



### <a name="getmembersresponsemodel"></a>GetMembersResponseModel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|liikmed|massiiv|Ei |
|list_id|string|Ei |
|total_items|täisarv|Ei |



### <a name="adduserresponsemodel"></a>AddUserResponseModel


| Atribuudi nimi | Andmetüüp | Nõutav |
|---|---|---|
|ID|string|Jah |
|email_address|string|Jah |
|unique_email_id|string|Ei |
|email_type|string|Ei |
|olek|string|Ei |
|merge_fields|määratletud ei|Jah |
|huvide|string|Ei |
|statistika|määratletud ei|Ei |
|ip_signup|string|Ei |
|timestamp_signup|string|Ei |
|ip_opt|string|Ei |
|timestamp_opt|string|Ei |
|member_rating|täisarv|Ei |
|last_changed|string|Ei |
|keel|string|Ei |
|VIP|kahendmuutuja|Ei |
|email_client|string|Ei |
|asukoht|määratletud ei|Ei |
|last_note|määratletud ei|Ei |
|list_id|string|Ei |
|_links|massiiv|Ei |


## <a name="next-steps"></a>Järgmised sammud
[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)
