helpscout-api-php
=================
PHP Wrapper for the Help Scout API. More information on our developer site: [http://developer.helpscout.net](http://developer.helpscout.net).

Requirements
---------------------
* PHP 5.3.x
* curl

Example Usage
---------------------
<pre><code>include 'HelpScout/ApiClient.php';

use HelpScout\ApiClient;

$hs = ApiClient::getInstance();
$hs->setKey('your-api-key-here');

$mailboxes = $hs->getMailboxes();
if ($mailboxes) {
    // do something
}

$mailbox = $hs->getMailbox(99);
if ($mailbox) {
    $mailboxName = $mailbox->getName();
    $folders = $mailbox->getFolders();
    // do something
}

$conversation = $hs->getConversation(999);
if ($conversation) {
    // do something
    $threads = $conversation->getThreads();
    foreach($threads as $thread) {
        if ($thread instanceof \HelpScout\model\thread\LineItem) {
          // do something else
          continue;
        }
        if ($thread instanceof \HelpScout\model\thread\ConversationThread) {
          // do something again
        }
    }
}

// to get page 2 of a list of conversations:
$list = $hs->getConversationsForMailbox(99, array('page' => 2));

// to get all the closed conversations:
$closed = $hs->getConversationsForMailbox(99, array('page' => 1, 'status' => 'closed'));

// to get page 2 of a list of conversations, 
// while only returning the "id" and "number" attributes on a conversation:
$convos = $hs->getConversationsForMailbox(99, array('page' => 2), array('id', 'number'));

// to get page 2 of conversations from a specific folder:
// 99=MailboxID
// 22=FolderID
$convos = $hs->getConversationsForFolder(99, 22, array('page' => 2));
</code></pre>

Field Selectors
---------------------
Field selectors can be given as a string or an array.

When field selectors are used, a JSON object is returned with the specificed fields. If no fields are given, you will be given the proper object. For example, the following code will return a JSON object with fields for 'name' and 'email'.
<pre><code>$mailbox = ApiClient::getInstance()->getMailbox(99, array('name','email'));</code></pre>
### Returned JSON
<pre><code>{
    "name": "My Mailbox",
    "email": "help@mymailbox.com"	
}
</code></pre>

Built-in Functions
--------------------

### Mailboxes
* getMailboxes($page=1, $fields=null)
* getMailbox($mailboxId, $fields=null)

### Folders
* getFolders($mailboxId, $page=1, $fields=null)

### Conversations
* getConversationsForFolder($mailboxId, $folderId, array $params=array(), $fields=null)
* getConversationsForMailbox($mailboxId, array $params=array(), $fields=null)
* getConversationsForCustomerByMailbox($mailboxId, $customerId, array $params=array(), $fields=null)
* getConversation($conversationId, $fields=null)

### Attachments
* getAttachmentData($attachmentId)

### Customers
* getCustomers($page=1, $fields=null)
* getCustomer($customerId, $fields=null)

### Users
* getUsers($page=1, $fields=null)
* getUsersForMailbox($mailboxId, $page=1, $fields=null)
* getUser($userId, $fields=null)