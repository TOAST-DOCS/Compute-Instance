<style>
.page__rnb .lst_rnb_item .rnb_item:first-of-type a { 
    display: inline !important;
}
</style>
<h1>Getting Started with Notification Hub</h1>

**Notification > Notification Hub > Console Guide > Getting Started with Notification Hub**

<span id="identity-verification"></span>

## Identity Verification

After Notification Hub is enabled, you must complete identity verification before you can use the service. For more details on identity verification, see **Service Policy and Precondition Guide > Identity Verification**.

* [Go to Identity Verification Guide](../service-policy-and-precondition/identity-verification)


<span id="manage-sender-info"></span>

## Sender Information Management

<span id="manage-sender-phone-number"></span>

### Sender Number Management

To send SMS, LMS, and MMS messages, you must register a sender number. After requesting sender number registration review and approval, the sender number is registered.

1. Click **+ Register Sender Number** and agree to the **Personal Information Collection and Use Agreement**.
2. Select the sender number type to register and enter the sender number.
3. Attach the required documents for the sender number type.

For more details on the sender number pre-registration system, see **Service Policy and Precondition Guide > SMS > Sender Number Pre-registration System**.

* [Go to Sender Number Pre-registration System](../service-policy-and-precondition/sms#sender-phone-number-pre-registration)

<span id="manage-sender-brand"></span>

### Brand Management

To send RCS messages, you must complete brand linkage. If the pre-registration requirements have been completed (brand approved) in RCS Biz Center, proceed with integration in the NHN Cloud console. For creating brands in RCS Biz Center, see **Service Policy and Precondition Guide** > **RCS**.

* [Go to Service Policy and Precondition Guide > RCS](../service-policy-and-precondition/rcs)
* [Go to RCS Biz Center](https://www.rcsbizcenter.com/main)

Once brand creation, agency settings, chatroom (sender number) registration, and template registration are completed (approved) in RCS Biz Center, link the brand in the console.

* Click **+ Link Brand** to complete synchronization.

<span id="manage-sender-domain"></span>

### Domain Management

To send emails, you need your own domain, SPF authentication, DKIM authentication, and DMARC authentication.

For more details on sending domains and SPF, DKIM, DMARC, see **Service Policy and Precondition Guide > Email**.

* [Go to Service Policy and Precondition Guide > Email](../service-policy-and-precondition/email)

#### Email Domain Registration and Ownership Authentication

You must register a domain and verify domain ownership. Register the value provided by Notification Hub in the email domain DNS TXT record. Ownership is authenticated by matching the provided value with the TXT record of the registered domain.

1. Click **+ Register Domain**.
2. Enter the root domain to register and click **Verify**.
3. If verification is successful, click **Register** to complete registration.
4. In the domain list, click **Authenticate** in the domain ownership authentication status.

Once domain ownership authentication is successful, the domain authentication status changes to 'Completed'.

#### SPF Authentication

SPF (Sender Policy Framework) is a mechanism to verify the reliability of email senders and sending servers. The email receiving server checks whether emails sent from a specific domain actually come from authorized email sending servers. The mail receiving server checks the SPF record registered in the sender's email domain DNS and treats mail sent from unregistered IP addresses as spam.

**SPF**
```
v=spf1 include:_spfblocka.toast.com ~all
```

1. Register the SPF record from the **SPF** item above in the domain TXT record.
2. Select the domain from the list.
3. Click **Check Status** in the **SPF Record Authentication Status** item to complete SPF authentication.


!!! danger "Caution"
    * Only one SPF record should be registered in the domain TXT record. If two or more SPF records are registered in the domain TXT record, SPF authentication may fail and the email receiving server may refuse to receive emails.
    * When checking SPF records, the use of mechanisms (include) and modifiers (redirect) that cause DNS lookups is limited to a maximum of 10, and exceeding this may result in refusal by email receiving servers.
    
For more details on SPF, see the documents below.

* [Go to Email Security Enhancement Feature Introduction (SPF)](https://meetup.nhncloud.com/posts/244)
* [Go to RFC 4408 - 4.5 Selecting Records](https://datatracker.ietf.org/doc/html/rfc4408#section-4.5)
* [Go to RFC 4408 - 10.1 Processing Limits](https://datatracker.ietf.org/doc/html/rfc4408#section-10.1)

#### DKIM Authentication

DKIM (DomainKeys Identified Mail) is an email verification method where the email sending server digitally signs emails and the email receiving server verifies sender authenticity to confirm that messages were not forged or altered during transmission. DKIM can prevent spam senders and other malicious attackers from forging or altering emails.


1. After completing domain ownership authentication, check the domain in the list and click **DKIM Settings**.
2. Set the TXT record value for the DNS host name provided for DKIM authentication and click **Authenticate** below.
    * If the registered domain is `example.com`, you must set the value in the `toast._domainkey.example.com` TXT record.
3. After authentication is complete, enable the setting and click **Save** to complete DKIM authentication.

For more details on DKIM, see the document below.

* [Go to Email Security Enhancement Feature Introduction - Domain Protection, DKIM, DMARC](https://meetup.nhncloud.com/posts/248)


#### DMARC Authentication

DMARC (Domain-based Message Authentication Reporting and Conformance) is the final step in email security enhancement. It is a domain-based message authentication reporting and compliance policy to prevent phishing and fraud using email spoofing. The email receiving server queries the DMARC record from the DNS of the sender address (From) domain. According to the policy defined in the DMARC record, the receiving server authenticates received emails.

**DMARC**
```
"v=DMARC1;p=none;sp=quarantine;pct=100;rua=mailto:${email_address_to_receive_reports}"
```

1. Complete the DMARC TXT record by adding the email address to receive DMARC reports to the value in the **DMARC** item above.
2. Register it in the subdomain TXT record with `_dmarc.` added.
    * Example: If the domain is `example.com`, register it in the TXT record of `_dmarc.example.com`.
3. Click **Check Status** in the **DMARC Authentication Status** item to complete DMARC authentication.

For more details on DMARC, see the document below.

* [Go to Email Security Enhancement Feature Introduction - Domain Protection, DKIM, DMARC](https://meetup.nhncloud.com/posts/248)

##### Domain Protection

Domains with domain protection enabled cannot be used in other projects. To use a protected domain in another project, the same domain registration and ownership authentication must be completed.

!!! danger "Caution"
    If domain protection is disabled, the domain can be used arbitrarily in other projects. For domains that have completed all authentication, emails sent from other projects will also be received normally by email receiving servers. If such emails are spam or phishing, it may cause damage to recipients and the domain's reputation may decline, causing receiving email servers to refuse reception.

<span id="manage-sender-push-authorization"></span>

### Push Authentication Management

For how to issue Push authentication information, see **Service Policy and Precondition Guide > Push**.

* [Go to Service Policy and Precondition Guide > Push](../service-policy-and-precondition/push)

#### FCM Authentication Settings
1. Enable **Register Service Account Key**.
2. Copy and paste the contents of the issued FCM Service Account Credential file into Service Account Key (JSON).
3. Click **Verify > Save** to complete the settings.

#### APNS Authentication Settings
1. Enable **Register APNS JWT Certificate**.
2. Enter the **Team ID** and **Key ID**.
3. Enter the **Topic**. The topic is the app's Bundle ID.
4. Copy and paste the contents of the **Private Key** file.
5. Click **Verify > Save** to complete the settings.

#### ADM Authentication Settings
1. Enable **Register Credentials**.
2. Enter the **Client ID** and **Client Key**.
3. Click **Verify > Save** to complete the settings.

<span id="manage-sender-profile"></span>

### Sender Profile Management

To send KakaoTalk notifications, you need to create and register a sender profile.

Sender profile creation can be done through KakaoTalk Business.

* [Go to Sender Profile Creation Guide](../service-policy-and-precondition/alimtalk-and-friendtalk)


Once sender profile creation is completed in KakaoTalk Business, register it following these steps:

1. Click **+ Register Sender Profile**, set the sender profile ID, administrator mobile number, and category, then click **Request Token**.
2. Enter the token sent to the administrator's mobile phone and click **Confirm > Register** to complete sender profile registration.


<span id="manage-080-unsubscription-number"></span>

### 080 Unsubscription Number Management

The 080 unsubscription number is a service that provides unsubscription options to recipients when sending advertising messages. When transmitting advertising information, you must include a free unsubscription method so recipients can unsubscribe or withdraw consent for free.

#### Apply for Subscription

* Click **+ Apply for 080 Unsubscription Number** and enter the company name. The company name you enter is the business name announced when calling the 080 unsubscription number.
* Once the application is completed, the status changes to registration pending. The 080 unsubscription service activation takes 3-4 business days, and once activation is complete, it can be used.
* Once activation is complete, you can check the service start date and status. You cannot terminate SMS service usage while the 080 unsubscription service is in registration pending or active status. Service termination is possible after cancellation. To cancel, click **Cancel**.

#### Setting 080 Unsubscription Number When Sending Advertising Messages

* Advertising messages can only be sent when the 080 unsubscription number is activated.
* In the **Send > SMS** tab, when you select advertising as the sending purpose, the 080 unsubscription number selection window appears.
* Click **Apply Selection** to add mandatory advertising phrases.
* When sending advertising messages, the message body must include mandatory advertising phrases, and the rules are as follows:
    * Opening phrase: (광고)
    * Closing phrase: 무료수신 거부 {080 unsubscription number} or 무료거부 {080 unsubscription number} (These phrases may include spaces.)

##### Examples
```
(광고)

[무료 수신 거부]080XXXXXXX
```
```
(광고)

무료거부 080XXXXXXX
```