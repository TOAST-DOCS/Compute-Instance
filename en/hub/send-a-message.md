<style>
.page__rnb .lst_rnb_item .rnb_item:first-of-type a {
    display: inline !important;
}
</style>
<h1>Send</h1>

**Notification > Notification Hub > Console Guide > Send**

<span id="message"></span>

!!! danger "Caution"
    Before sending, sender information must be registered for the message channel you want to send to. For more details on sender information, see **Notification** > **Notification Hub** > **Console Guide** > **Getting Started** > **Sender Information Management**.


<span id="send-flow-message"></span>

## Send Flow Messages

To send flow messages, you must have a registered flow.


1. Select a flow.
2. Configure recipients. There are three ways to configure recipients: direct input, selecting from address book, or file upload.
    * For direct input and selecting from address book, enter template substitution variables when configuring recipients.
    * For file upload, you must enter recipient information and template substitution variables in the file.
    * Recipient configuration is covered in detail below.
3. Configure statistics key.
4. Configure send time. For scheduled sending, set the send date and time.
    * Scheduled send time can be set up to 30 days from the send time.
5. Click **Send** to send the message.

You can use the **Copy Input Values (JSON)** button to copy the send configuration in JSON format.


## Send Individual Message Channels

1. Select whether to use a template, and if using a template, select the template.
    * For KakaoTalk notifications, select a sender profile and choose a template registered to the sender profile.
2. Configure recipients. There are three ways to configure recipients: direct input, selecting from address book, or file upload.
    * For direct input and selecting from address book, enter template substitution variables when configuring recipients.
    * For file upload, you must enter recipient information and template substitution variables in the file.
    * Recipient configuration is covered in detail below.
3. If not using a template, write the message title and content.
    * How to write titles and content for each message channel is covered in detail below.
4. Configure send time. For scheduled sending, set the send date and time.
    * Scheduled send time can be set up to 30 days from the send time.
5. Click **Send** to send the message.

You can use the **Copy Input Values (JSON)** button to copy the send configuration in JSON format.

### How to Configure Recipients

#### Direct Input and Selecting Recipients from Address Book

* For flow sending, all contact information for message channels configured in the flow must be filled in for sending to be possible.
* For direct input in flow sending, enter all recipient contact information for message channels configured in the flow.
* For selecting recipients from address book in flow sending, you can only select recipients who have all contact information configured for the message channels set in the flow.
* For individual message channel sending, enter contact information corresponding to the message channel.
* For push tokens, enter the push type and token generated from the device.

#### File Upload

* Download the recipient contact list file template.
* Rows represent recipients and columns represent recipient contact information.
* Push token columns are JSON objects composed of contact type and token. Up to 6 can be entered per recipient.


The structure of the recipient contact list file template is as follows:

| contactPhoneNumber | contactEmailAddress | contactTokenJson1..6 | 
| - | - | - | 
| Recipient phone number | Recipient email address | {"contactType": "contact_type", "contact": "push_token" } |

### How to Write Message Titles and Content

#### SMS
* Select sender number and sending purpose. If the sending purpose is advertising, select an 080 unsubscription number.
* Select sending type. Sending types are SMS (short message), LMS (long message), MMS (multimedia long message).
* The character set that can be input for SMS, LMS, and MMS is EUC-KR.
    * [Wikipedia EUC-KR Link](https://ko.wikipedia.org/wiki/EUC-KR)
* SMS allows up to 90 bytes, with 45 Korean characters or 90 English characters.
* LMS and MMS allow up to 2,000 bytes, with 1,000 Korean characters or 2,000 English characters.
* MMS can attach images.
* If text transmission fails due to sender number blocking, check the 'Phone Number Scam Blocking Service'.
    * [Phone Number Scam Blocking Service Guide Link](../service-policy-and-precondition/sms#about-phone-scam-blocking-services)
* If the transmission result is successful but you don't receive the text, check the 'Carrier Spam Blocking Service'.
    * [Carrier Spam Blocking Service Guide Link](../service-policy-and-precondition/sms#about-carrier-spam-text-blocking-services)
* For authentication SMS messages, authentication phrases must be included.
      * Authentication phrases: auth, password, verif, にんしょう, 認証, 비밀번호, 인증

##### MMS Attachable Image Specifications

* MMS maximum size: 1000×1000 or smaller file
* MMS supported specifications: 300KB or less per file, and if there are 3 images, combined 800KB or less. .jpg, .jpeg files


#### International SMS
International SMS is sent as Concatenated Messages depending on encoding and character count.

* Concatenated Messages is a service that overcomes character count limitations in international SMS sending by connecting them to display as one long text on the device, similar to domestic LMS.
  Concatenated Message functionality is provided or limited depending on message body character count, encoding, and support from overseas carriers.
* When Concatenated Messages are not supported, they may be received on the device as multiple short messages.
* Headers added during the Concatenated Message creation process occupy message body space (approximately 6 bytes) for message connection, reducing the number of characters that can be entered.
* Charges are billed according to the number of Concatenated Messages.

| Encoding | 1 message charge | 2 message charge | 3 message charge | 4 message charge | 5 message charge |
| --- | --- | --- | --- | --- | --- |
| UCS-2<br>(Unicode) | 70 characters | 134 characters<br>(=67*2) | 201 characters<br>(=67*3) | 268 characters<br>(=67*4) | 335 characters<br>(=67*5) |
| GSM-7bit | 160 characters | 306 characters<br>(=153*2) | 459 characters<br>(=153*3) | 612 characters<br>(=153*4) | 765 characters<br>(=153*5) |


#### RCS

1. Select sender brand and chat room (sender number).
2. Select sending purpose. For advertising, select an 080 unsubscription number.
3. Select sending type. To use RCS Biz Center templates, you must select template usage as **Use**. Otherwise, only SMS, LMS, MMS can be selected.
    * SMS allows up to 100 Korean/English characters and can set 1 button.
    * For authentication SMS messages, authentication phrases must be included.
      * Authentication phrases: auth, password, verif, にんしょう, 認証, 비밀번호, 인증
    * For authentication SMS messages, buttons cannot be added.
    * LMS Standard type allows up to 30 characters for body title and up to 1,300 characters for body content regardless of Korean/English, and can set up to 3 buttons.
    * For LMS Format type Basic and Title Emphasis types, main title allows up to 17 characters, body title up to 30 characters, body content up to 1,300 characters, and can set up to 2 buttons.
    * For LMS Format type Paragraph type, main title allows up to 17 characters, body title up to 30 characters, body content up to 1,300 characters, and can set up to 2 buttons within the body. 
        * Additionally, up to 3 bodies can be added, and all body titles and body content combined can be up to 1,300 characters.
    * MMS allows up to 30 characters for title and up to 1,300 characters for content per card regardless of Korean/English, 1 image, and up to 2 buttons.
        * MMS can select horizontal, vertical, or slide types in detailed settings.
        * When selecting slide type, you can add a minimum of 3 to a maximum of 6 slides.
    * When selecting free templates from RCS Biz Center templates, messages can be entered up to 90 characters.
    * RCS Biz Center templates require prior registration in RCS Biz Center.
    * When sending integrated message types for advertising purposes, you must include "(Advertisement)" text and free unsubscription text.
        * For integrated SMS cards, since there is no title, the "(Advertisement)" text must be included at the beginning of the body, and free unsubscription text and 080 number must be included at the end of the body.
        * For integrated LMS cards and integrated MMS cards, the "(Advertisement)" text must be included at the beginning of the title, and free unsubscription text and 080 number must be included at the end of the body.

!!! danger "Caution"
    * If integrated message types (integrated SMS cards, integrated LMS cards, integrated MMS cards) include copy buttons, they cannot be received on iOS devices.
    * If GIF images are attached to integrated MMS cards, they cannot be received on iOS devices.

##### RCS Group ID
* Message statistics can be checked in RCS Biz Center only for messages with added group IDs.
* It is recommended to set group IDs for each message you want to analyze.
* Data is aggregated for 4 days (D+3) based on the same group ID send date.
* Data is aggregated only when approximately 500 successfully sent messages (over 100 successful sends per carrier) have the same group ID and message format.
* Searches are available for up to 31 days within the most recent 1 year and 6 months. When searching, please include the campaign send start date.
* Button clicks by the same customer within 1 day are excluded.

##### RCS Button Types
* Open Chat Room
    * Sends the configured message to the configured phone number.
    * After entering the button name, enter the phone number to send the message to.
    * Enter the message content to send.
* Copy
    * The configured value is copied.
    * After entering the button name, enter the value to be copied when the button is clicked.
* Make Call
    * Makes a call to the configured phone number.
    * After entering the button name, enter the phone number to call when the button is clicked.
* Show Map/Search Map
    * Shows the configured location in a map app.
    * After entering the button name, enter the latitude and longitude of the location.
    * Enter the location name and map URL (URL including <span>https://</span>).
* Share Current Location
    * The recipient sends their current location to the sender as a message.
    * Enter the button name.
* URL Connection
    * Connects to a web link.
    * After entering the button name, enter the link to connect to when the button is clicked.
    * When entering links, 'http://' or 'https://' must be included.
* Register Schedule
    * Registers a schedule in the recipient's calendar app.
    * After entering the button name, select the schedule start and end dates.
    * Enter the schedule title and schedule content.


#### KakaoTalk Notification

* Select a sender profile and a template registered to the sender profile.
* KakaoTalk notifications only support template sending, so no content input is required.

#### Email

1. Select sending purpose.
    * If you select advertising as the sending purpose, additional input is required.
        * The "(Advertisement)" text must be included at the beginning of the subject.
        * The sender's name, email address, phone number, and address must be displayed in the body.
        * Unsubscription links must be included in Korean/English format, and technical measures must be taken to allow recipients to unsubscribe.
        * Users registered for unsubscription will not receive advertising emails.
        * When you select advertising as the sending purpose, a notification popup appears and you configure unsubscription guidance text.
2. Enter the sender address. Writing in name format allows you to enter both sender name and email address.
    * Sending in the format "Sender Name<sender email>" displays the sender name and email address format to the email recipient.
3. Attach files by uploading directly or selecting registered files.
    * Up to 10 attachments can be uploaded, and only files 30MB or smaller are allowed.
    * The total size of attachments cannot exceed 30MB.
    * While up to 30MB can be attached, it is recommended to keep attachments under 10MB as they may be rejected as `limit exceeded` or have higher spam determination rates according to receiving email system (gmail.com, naver.com, etc.) attachment policies.


##### Precautions When Sending Advertising Emails

When sending commercial advertising emails or company promotional emails according to the Information and Communications Network Act, the following must be observed. ([Check related content from Korea Internet & Security Agency](https://spam.kisa.or.kr/spam/cm/cntnts/cntntsView.do?mi=1061&cntntsId=1086)) <br>


1. Advertising emails must only be sent to recipients who have explicitly consented to receive them. If disputes arise due to violations, responsibility lies with the advertising email sender.
2. The "(Advertisement)" text must be included at the beginning of the subject.
3. Sender information including the sender's name, email address, phone number, and address must be displayed in the body.
4. Guidance text must be specified in the body to enable recipients to easily express their intention to unsubscribe or withdraw consent.
5. Technical measures must be taken to allow recipients to easily select unsubscription or consent withdrawal by clicking [Unsubscribe] etc. within the body, and in this case, the guidance text and technical measures must be specified in Korean and English.

```
If you do not want to receive emails, click [Unsubscribe].
If you do not want to receive it, please click a [Unsubscription].
```

NHN Cloud provides the following technical measures for 'advertising emails' to comply with the Information and Communications Network Act.

- Inserts "(Advertisement)" text in the subject.
- Provides unsubscription functionality in Korean and English format to allow recipients to choose unsubscription.
- Does not send advertising emails to email addresses designated for unsubscription.

##### Keys Provided as Unsubscription Links

| Key | Text | Usage Example |
|-------------------------| - |-------------------------------------------------------------------------------------------------------------------------------|
| BLOCK_RECEIVER_LINK | [Unsubscribe](#) | If you do not want to receive emails, click ##BLOCK_RECEIVER_LINK##. |
| EN_BLOCK_RECEIVER_LINK | [Unsubscription](#) | If you no longer wish to receive these emails, please click the ##EN_BLOCK_RECEIVER_LINK##. |
| JA_BLOCK_RECEIVER_LINK | [受信拒否](#) | メールの受信を希望しない場合、##JA_BLOCK_RECEIVER_LINK##をクリックしてください。 |
| BLOCK_RECEIVER_LINK_URL | - | If you no longer wish to receive these emails, please `<a href='##BLOCK_RECEIVER_LINK_URL##' target='_blank'>click here</a>`. |


### Push

1. Select sending purpose.
2. If you select advertising as the sending purpose, additional input is required.
    * Enter the representative number in the sending contact information.
    * In the consent withdrawal guide, enter how to unsubscribe from push messages in the app.
        * Example: Unsubscribe: Settings > Notification Settings
3. Select input type. If you select JSON as the input type, you must enter the entire content according to JSON format.
4. Select whether to use HTML style.
    * Using HTML style allows HTML to be displayed on Android devices.
    * iPhone (iOS) does not support HTML style.
    * When sending messages using HTML style, you must write and send Android and iPhone messages separately.
5. You can send push messages in various forms by including buttons, images, etc.
    * To properly display buttons and images in push messages received on devices, SDK implementation in the app is required.
        * [Android SDK Link](https://docs.nhncloud.com/en/nhncloud/en/nhncloud-sdk/push-android/)
        * [iOS SDK Link](https://docs.nhncloud.com/en/nhncloud/en/nhncloud-sdk/push-ios/)


#### Button

| Name | Content |
| --- | --- |
| Name | Button name                                                           |
| Type | Button type: Reply (REPLY), Open App (OPEN_APP), Open URL (OPEN_URL), Dismiss (DISMISS) |
| Send button name | If button type is reply button, you can set the send button name on iOS.                       |
| Link | Link to navigate to or execute when the button is pressed. Applies when button type is Open URL.                |
| Hint | Description of the button.                                                    |

#### Button Types
- Reply
    - Executes direct reply functionality.
    - When the user touches the send button, user input text is delivered to the action listener.
- Open App
    - The app is executed.
    - The full message is delivered through the action listener. You can implement functionality such as navigating to specific pages by entering information in the message.
- Open URL
    - Executes the URL (https://...) or Scheme (scheme://...) entered in the link item.
    - Entering a URL executes the web browser and loads that URL.
    - Entering a scheme executes a scheme predefined in the app.
- Dismiss
    - Closes the notification.

#### Media

| Name | Content |
| --- | --- |
| Location | Where media is located: 'REMOTE' or 'LOCAL'                   |
| Address | Address where media is located; can be URL, URI, etc.                |
| Type | You can select image, GIF, video, or sound (Android supports images only) |
| Extension | Media extension such as .png, .avi, etc. |
| Expand | Media expansion feature, available on Android only.                      |

#### Media File Specification
- External
    - Downloads and uses media files from the entered URL.
    - Android
        - To use HTTP on Android Pie or later, you must configure <a href="http://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#android-p">network-security-config</a>.
    - iOS
        - To use HTTP on iOS 9 or later, you must configure ATS (app transport security) in the Info.plist file.
        - You must enter actual media file extension information in the extension field. (e.g.: jpg, png, mp4, wav, ...)
- Internal
    - Uses resources included in the app.
    - Android
        - Files must be pre-added to 'res > drawable'.
        - Since access is through resource identifiers, enter the filename excluding extension in 'richMessage.media.source' when writing messages.
        - In Android, since filenames are used as resource identifiers, the same filename cannot be used even if extensions differ.
          Supported image formats are png, jpg, gif. (Video and audio format media are currently not supported.)
    - iOS
        - Resources must be pre-added to the <a href="http://docs.toast.com/en/TOAST/en/toast-sdk/push-ios/#notification-service-extension">Notification Service Extension</a> project that creates rich messages.
        - Add files or directories to the 'NotificationServiceExtension' project in XCode.
        - Check that files are properly added in 'Build Phases > TARGETS'.
        - Since access is through bundle resources, the full filename including extension is required.
        - Enter the added filename in 'richMessage.media.source' when writing messages.

#### Media Types
- Image

| | Android | iOS |
| - | - | - |
| Supported formats | JPEG, PNG, GIF | JPEG, PNG, GIF |
| GIF animation | Not supported | Supported |
| File size | No limit | 10MB |
| Recommendations | Horizontal image with 2:1 ratio recommended<br>Small: 512x256<br>Medium: 1024x512<br>Large: 2048x1024 | Horizontal image recommended<br>Maximum size: 1038x1038 |

- Video

| | Android | iOS |
| - | - | - |
| Supported formats | Not supported | MPEG, MPEG3Video, MPEG4, AVIMovie |
| File size | Not supported | 50MB |

- Sound

| | Android | iOS |
| - | - | - |
| Supported formats | Not supported | WaveAudio, MP3, MPEG4Audio |
| File size | Not supported | 5MB |

#### Large Icon
A feature provided only on Android. Specifies a large icon for notifications. File specification method is the same as media file specification.

| Name | Content |
| --- | --- |
| Location | Location: 'REMOTE' or 'LOCAL'         |
| Address | Address where image is located; can be URL, URI, etc. |

#### Group
A feature provided only on Android. Sets groups for notifications and displays notifications with the same group key together.

| Name | Content |
| --- | --- |
| Key | Group key     |
| Description | Group description |

#### Notification Sound
| | Android | iOS |
| - | - | - |
| Supported formats | MP3, PCM/WAVE, Vorbis | Linear PCM, MP4(IMA/ADPCM), μ-law, aLaw |
| Extensions | .mp3, .wav, .ogg | .aiff, .wav, .caf |
| Play time | No limit | 30 seconds |

- Only resources included in the app can be specified. (External URLs cannot be used)
- Android
    - Resources must be pre-added to the 'res > raw' folder.
    - Since access is through resource identifiers, file extensions are ignored.
    - Works only on versions below Android Oreo.
- iOS
    - Resources must be pre-added as bundle resources in the app project.
    - Since access is through bundle resources, the full filename including extension is required.