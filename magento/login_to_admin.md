```php
<?php
$username = isset($_GET['username']) ? trim($_GET['username']) : null;

if (!$username) {
    echo 'Username is required'; 
}

require_once 'app/Mage.php';
umask(0);
$app = Mage::app();

Mage::getSingleton('core/session', array('name' => 'adminhtml'));

$user = Mage::getModel('admin/user')->loadByUsername($username);

if (Mage::getSingleton('adminhtml/url')->useSecretKey()) {
  Mage::getSingleton('adminhtml/url')->renewSecretUrls();
}

$session = Mage::getSingleton('admin/session');
$session->setIsFirstVisit(true);
$session->setUser($user);
$session->setAcl(Mage::getResourceModel('admin/acl')->loadAcl());
Mage::dispatchEvent('admin_session_user_login_success',array('user'=>$user));

if ($session->isLoggedIn()) {
  echo 'logged in';
}
```