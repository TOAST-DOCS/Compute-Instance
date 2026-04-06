<style>
.page__rnb .lst_rnb_item .rnb_item:first-of-type a {
    display: inline !important;
}
</style>
<h1>Templates</h1>

**Notification > Notification Hub > Console Guide > Templates**


<span id="template"></span>

## Templates

You can save frequently used messages or messages that require a specific format as templates and send messages by configuring the saved templates when sending. For example, if you create templates for frequently used message formats such as customer support, announcements, notifications, or marketing messages, you can send messages by modifying only some information without having to write the same content every time.

### Category
* First, select the root category and click **+ Add Category** to create a category.
* Categories are created under the selected category.

### Templates
1. Select the category that the template will belong to and click **+ Register Template**. You will be taken to the template creation page, where additional settings for the selected message channel are displayed.
2. Complete the title, content, and settings required by each message channel, then click **Register**.

#### KakaoTalk Notification Templates

KakaoTalk notification templates can only be used after receiving approval from Kakao's review after submitting a registration request.

* Available message types include Channel Addition, Basic, Additional Information, and Composite types.
* Available emphasis types include Emphasis, Image, and Item List types.
* Select the desired type to create a template.

* Kakao Notification Guide
    * [[Notification Creation Guide]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/content-guide), [[Notification Review Guide]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/audit), [[Notification Whitelist]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/audit/white-list), [[Notification Blacklist]](https://kakaobusiness.gitbook.io/main/ad/bizmessage/notice-friend/audit/black-list)
* Sender Profile/Group
    * Select the sender profile or sender profile group to register the template to. When registering to a group, all sender profiles included in the group can use the template.
    * If the same template code exists in a sender profile/group, the template registered to the sender profile takes priority for sending.
    * For sender profile groups, templates cannot be registered with 'Channel Addition' and 'Composite' message types.
* Template Code/Template Name
    * The same template code and template name cannot be duplicated within a single sender profile/group.

* Template Content
    * KakaoTalk notifications can be written up to 1,000 characters including variables, URLs, spaces, and button names regardless of Korean/English. When registering with variables, consider the content to be replaced when creating the template.<br/> For detailed character count guidelines, see [KakaoTalk Notification Template Precautions](https://docs.nhncloud.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-overview/#_3).
    * Write variables in the form of #{variable}. (Example: #{Hong Gil-dong}'s package is scheduled to be delivered today at (#{09:50}).)
    * When registering buttons, variables cannot be entered in button names, but variables can be entered in button URLs. (Example: http://kakao.com/#{variable})
    * When registering button URLs, url_mobile and url_pc links must include 'http://' or 'https://', and scheme_ios and scheme_android links must be registered according to the scheme format. Otherwise, template registration is not possible.
* Security Template
    * When the template is secured, message content is not exposed on devices other than mobile.('Please check on mobile' message is displayed)
    * For general messages, setting values may be changed during review, and security must be checked for OTP, verification number, password, and credit information/grade change notification templates.

#### KakaoTalk Notification Template Buttons
* Up to 5 buttons can be registered per template.
* Quick Connect
    * Delivery tracking and plugin types cannot be used, but other types can be used the same as buttons.
    * Up to 10 can be used per template, and when using quick connect, the number of buttons is limited to 2.

| Button Type | Description |
| --- | --- |
| Delivery Tracking | - Navigate to the courier company's delivery tracking page.<br/> - Check courier companies that support delivery tracking buttons and invoice number patterns for each courier company. [[KakaoTalk Notification Delivery Tracking Invoice Number Pattern Guide]](https://www.nhncloud.com/kr/support/notice/detail/1455)|
| Web Link | - Navigate to mobile or PC web pages.<br/> - URL links can be set as variables. |
| App Link | - Launch the app with a custom scheme.<br/> - Custom schemes to run on Android and iOS must be set separately. |
| Bot Keyword | - The button name is delivered to the counselor.<br/> - If a channel that does not support consultation chat adds this button, KakaoTalk notification sending is not possible. |
| Message Forward | - The button name and message body are delivered to the counselor.<br/> - If a channel that does not support consultation chat adds this button, KakaoTalk notification sending is not possible. |
| Consultation Chat Transfer | - Connect to consultation chat.<br/> - If a channel that does not support consultation chat adds this button, KakaoTalk notification sending is not possible. |
| Bot Transfer | - Chatbot is called.<br/> - If a channel that does not support consultation chat adds this button, KakaoTalk notification sending is not possible. |
| Channel Addition | - Add the channel that sent the KakaoTalk notification. The display position can only be used as the first button.<br/> - If the channel is already added, it is not displayed to the recipient. |
| Image Security Transmission Plugin | - Encrypt and transmit images containing sensitive information within the chat window.<br/> - Biz plugin creation is required. [[Biz Plugin Guide]](https://business.kakao.com/info/talkbizplugin/) |
| Personal Information Use Plugin | - Receive consent for collecting personal information necessary for service provision without membership registration within the chat window.<br/> - Biz plugin creation is required. [[Biz Plugin Guide]](https://business.kakao.com/info/talkbizplugin/) |
| One-Click Payment Plugin | - Users can pay for products without screen transitions within the chat window.<br/> - Payment plugins are not directly supported for registration on the platform, so please contact [Kakao Customer Center](https://cs.kakao.com/helps?service=127&category=572&locale=ko). | 
| Business Form | - If you create a business form and connect it to the current channel, the configured business form is called when the button is clicked.<br/> - Business form creation is required. [[Business Form Guide]](https://business.kakao.com/info/talkbizform/) |

#### Template Review
The review and examination of KakaoTalk notification templates are conducted directly by Kakao and processed sequentially within 2 business days after the review request.

* Template Inquiry Registration
    * If there are comments to be delivered to the Kakao review officer before requesting a review, enter the content in the inquiry registration field. Templates in request/approval status cannot be inquired. (**Only templates in review/rejection status can register inquiries.**)
    * Registered inquiries are added to the review results and confirmed by the Kakao review officer.
    * Inquiries about template usage and rejection reasons are added to the review results.
    * When a template is rejected, you can click **Register Inquiry** and **Modify** to re-review.

#### Template Status
* When registering a template, it is updated in the order of **Request > Under Review > Approval/Rejection** status.
* After template registration, if it remains in the same status for 1 year or there are no additional sends, it is converted to **Dormant** status. For related guidelines, see [KakaoTalk Notification Template Precautions](https://docs.nhncloud.com/en/Notification/KakaoTalk%20Bizmessage/en/alimtalk-overview/#_3).

#### Template Modification
* Only templates in **Approval/Rejection** status can be modified.
* When an approved template is modified and the review is completed, the existing template content is replaced with the modified content.
* Sender profile/group and template code cannot be modified.
* Modified templates undergo review again starting from **Under Review** status.

#### Template Deletion
* Only templates in request/rejection status can be deleted.
* Rejected templates can be re-registered after **deletion**.
* Deleted template codes can be reused.