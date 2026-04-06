<style>
.page__rnb .lst_rnb_item .rnb_item:first-of-type a {
    display: inline !important;
}
</style>
<h1>Detailed Settings</h1>

**Notification > Notification Hub > Console Guide > Detailed Settings**

Manage settings and attachments for each message channel. It may take approximately several minutes for settings to take effect after configuration.

## SMS

### International SMS Message Delivery Settings
* Be sure to check [[International SMS Service Policy]](../service-policy-and-precondition/international-sms) before using the international SMS delivery feature.
* If you do not want to use the international SMS delivery feature, set it to **Disabled**. When set to **Enabled**, additional charges may be incurred due to accidents caused by international SMS volume pumping.
* Monthly international SMS delivery volume can be set from a minimum of 1 to a maximum of 10,000 messages. If you need to send more than 10,000 messages, please contact [[Customer Center]](https://www.nhncloud.com/kr/support/inquiry).
* Allowed delivery countries management
    * When first enabled, it is configured to allow delivery only to designated major countries. Click the **Allowed delivery countries** input field to select countries for delivery.
* Monthly delivery volume and threshold notifications
    * **Monthly delivery volume** defaults to 1,000 messages per month and can be set up to a maximum of 10,000 messages.
    * **Monthly delivery volume** is a supplementary feature and threshold exceed detection may not be reflected in real time. NHN Cloud is not responsible for partial errors in supplementary features.
    * When **International SMS delivery** is set to **Enabled**, notification emails are sent to all project members when 70% and 100% of the **monthly delivery volume** threshold are reached.

!!! danger "Caution"
    International SMS abuse cases are increasing worldwide.
    It is recommended to set monthly limits and delivery countries only to the extent necessary.
    NHN Cloud takes no responsibility for international SMS sent through abusive acts.


### Message Settings

#### Message Duplicate Delivery Block Time Settings
* You can set to prevent messages with the same content from being sent for a specified time period.
* When duplicate delivery blocking is configured, identical requests will fail to send during the set time period (in minutes).
* The maximum blockable time is 1 hour.
* Duplicate determination criteria are as follows:
    * Message type (SMS/LMS/MMS/AUTH), sender number, recipient number, subject, body, attachments

#### Alternative Character Settings
* When the body/subject of a delivery request contains characters that are not included in the EUC-KR character set and cannot be sent, you can configure conversion to sendable characters.
    * Emoji characters are typically not included in the EUC-KR character set.
* You can select question mark '?' and space ' ' as alternative characters.
* When alternative character settings are enabled, unsendable characters are converted to and displayed as the configured alternative characters.

### Advertising Message Settings
#### Advertising Message Delivery Time Restriction Settings
* You can restrict the delivery time for advertising messages.
* Advertising delivery will not proceed during the configured advertising delivery restriction time.
    * Advertising delivery restriction start time settings: 18:00~21:00
    * Advertising delivery restriction end time settings: 08:00~12:00
* You can configure the handling method for unsent messages.
    * Process as failure
    * Resend after restriction time is lifted
* SMS and RCS advertising message delivery time restriction settings require individual configuration.

## RCS

### Advertising Message Settings
#### Advertising Message Delivery Time Restriction Settings
* You can restrict the delivery time for advertising messages.
* Advertising delivery will not proceed during the configured advertising delivery restriction time.
    * Advertising delivery restriction start time settings: 18:00~21:00
    * Advertising delivery restriction end time settings: 08:00~12:00
* You can configure the handling method for unsent messages.
    * Process as failure
* SMS and RCS advertising message delivery time restriction settings require individual configuration.

## Push

### Token Settings
#### Token Expiration Period Settings
* Tokens without registration requests for the configured period are deleted from the address book.
* The possibility of messages being received by tokens that have been inactive for long periods is extremely low.
    * Causes of inactive tokens
        * App has actually been deleted but the token remains
        * App has been deactivated on the device so messages cannot be received but the token remains
        * App has registered a new token but the old token remains
* The default value is 12 months.
* Inactive tokens are excluded from delivery targets, saving delivery costs.
* Cleaning up inactive tokens can improve delivery rate and reception rate accuracy.

#### App Type Settings
* Manage tokens according to the type of integrated app.
* Multiple tokens
    * This is the default setting. Multiple tokens can be registered for one recipient alias.
    - An app where users can simultaneously use apps installed on multiple devices, and one user can have multiple tokens.
    - For example, if a user uses both a phone and tablet, they can have two tokens, and push messages are sent to both the phone and tablet.
* Single token
    - Users can only use the app on one device at a time. One user can have only one token.
    - For example, if a user uses both a phone and tablet, they can have only one token, and push messages are sent to only one of them.

<span id="webhook"></span>

## Webhook

You can receive webhook events by specifying a URL when designated events occur.

1. Click the Add Webhook button.
2. Select the event type to register.
3. Enter the URL address that can receive data to be sent via webhook.
4. Enter the webhook signature to register (optional).
5. After verification, click Add to register the webhook.
 
Registered webhooks can be viewed in the webhook registration list.

<span id="backup"></span>

## Backup 

You can back up message delivery history that is older than 180 days according to the message retention period policy.

* It is set to disabled by default, and when set to enabled by entering storage information, backups are performed daily.
* Supported storage types are [NHN Cloud Object Storage](../../../../Storage/Object%20Storage/en/Overview/) and AWS S3.
    * **Access Key** and **Secret Key** can be verified through [AWS S3 API](../../../../Storage/Object%20Storage/en/s3-api-guide/#_1) EC2 credential registration and retrieval.
    * **Bucket Name** is the name of the Object Storage container where logs will be stored.
    * **Endpoint** and **Region** are information for managing Object Storage where logs will be stored, and can be found in [Amazon S3 Compatible API Guide - AWS SDK](../../../../Storage/Object%20Storage/en/s3-api-guide/#aws-sdk).
* You can specify up to 5 storage locations, and identical files are backed up to each storage location.
* Message bodies are backed up up to a maximum of 10,000 characters.
* The storage permissions required for backup are as follows:
    1. ListBucket - Determine bucket validity (check if upload is possible)
    2. deleteObject - Create and delete temporary files (check if upload is possible)
    3. putObject - Upload backup files