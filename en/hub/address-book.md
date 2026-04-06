<style>
.page__rnb .lst_rnb_item .rnb_item:first-of-type a {
    display: inline !important;
}
</style>
<h1>Address Book</h1>

**Notification > Notification Hub > Console Guide > Address Book**



## Address Book

You can register and manage recipients' contact information.

### Contacts

* Click **+ Add Contact**.
* Contacts can be registered by direct input or by uploading a file.
* The recipient alias is a value that can identify the recipient. It is generally used as an identifier for recipients (members) managed by customers integrated with Notification Hub.
    * This is different from the recipient ID managed internally in the address book.
    * Example: If the identifier for user `recipient@example.com` in the customer service member system is `638f11af-5e34-4803-9682-49265b690f69`, you can use `638f11af-5e34-4803-9682-49265b690f69` as the recipient alias in the address book.
* Up to 6 tokens can be registered.
* Up to 16 groups can be registered.

#### Download Contacts
You can download all stored contacts as a file.

* Click **Request Contact Download** to request extraction of contact delivery result data.
* Click **Download Request List** to check the requested list and download the completed file after extraction is finished.

### Groups

You can create groups to add recipients and group recipients together.

* Click **+ Add Group**.
* Enter a group name and click **Confirm** to create the group.
* Click the created group and click **+ Add Group Contact** in the **Manage Group Contacts** tab to add recipients to the group.

#### Download Group Contacts
You can download contacts belonging to a group as a file.

* Click **Request Group Contact Download** to request extraction of contact delivery result data.
* Click **Download Request List** to check the requested list and download the completed file after extraction is finished.


### Manage Unsubscriptions

You can query and manage unsubscribed mobile phone numbers, email addresses, and tokens.

#### Mobile Phone Numbers

* Click the dropdown list below the **+ Add Unsubscription Number** button to select an 080 unsubscription number, and query mobile phone numbers that have unsubscribed from the selected 080 unsubscription number.
* Click **+ Add Unsubscription Number** to manually add to the unsubscription list.

#### Email

* Click the dropdown list of the **+ Add Unsubscription Email** button to select an email domain, and query email addresses that have unsubscribed from the selected email domain.
* Click **+ Add Unsubscription Email** to manually add to the unsubscription list.


#### Tokens

* You can directly query the list of tokens that have unsubscribed.
* Click the dropdown list below the **+ Add Unsubscription Token** button to select an email domain, and query email addresses that have unsubscribed from the selected email domain.
* Click **+ Add Unsubscription Token** to manually add to the unsubscription list.

#### Download Unsubscription Numbers/Emails/Tokens
You can download unsubscribed contacts as a file.

* Click **Request Unsubscription Number/Email/Token Download** to request extraction of contact delivery result data.
* Click **Download Request List** to check the requested list and download the completed file after extraction is finished.