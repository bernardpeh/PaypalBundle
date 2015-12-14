LpWeb's PaymentBundle
=====================

`LpWebPaymentBundle` provides easy integration with payments gateway for Symfony2 and Twig

Installation
============

### Step 1: Download the LpWebPaymentBundle

***Using the vendors script***

Add the following lines to your `deps` file:

```
    [LpWebPaymentBundle]
        git=https://github.com/lpwebit/PaymentBundle.git
        target=/bundles/LpWeb/PaymentBundle
```

Now, run the vendors script to download the bundle:

``` bash
$ php bin/vendors install
```

***Using submodules***

If you prefer instead to use git submodules, then run the following:

``` bash
$ git submodule add git://github.com/lpwebit/PaymentBundle.git vendor/bundles/LpWeb/PaymentBundle
$ git submodule update --init
```

***Using Composer***

Add the following to the "require" section of your `composer.json` file:

```
    "lpwebit/payment-bundle": "dev-master"
```

You can also choose a version number, (tag, commit ...)

And update your dependencies

```
    php composer.phar update
```

### Step 2: Configure the Autoloader

If you use composer, you can skip this step.

Add it to your `autoload.pp` :

```php
<?php
...
'LpWeb' => __DIR__.'/../vendor/bundles',
```

### Step 3: Enable the bundle

Registers the bundle in your `app/AppKernel.php`:

```php
<?php
...
public function registerBundles()
{
    $bundles = array(
        ...
        new LpWeb\PaymentBundle\LpWebPaymentBundle(),
        ...
    );
...
```

### Step 4: Configure the bundle and set up the directories

Adds the following configuration to your `app/config/config.yml`:

    - { resource: @LpWebPaymentBundle/Resources/config/config.yml }

    lp_web_payment: ~

If you want to use Paypal as your payment gateway you must configure CLIENT_ID and CLIENT_SECRET,
to generate these please refer to Paypal documentation:

    lp_web_payment:
        paypal:
            client_id: YOUR_KEY
            client_secret: YOUR_SECRET
        
Complete configuration:

    lp_web_payment:
        paypal: # for information on mode and loggingLevel please refer to https://github.com/paypal/PayPal-PHP-SDK/wiki/Logging
            client_id: YOUR_KEY
            client_secret: YOUR_SECRET
            mode: sandbox|live
            logLevel: DEBUG|INFO|WARN|ERROR

Usage
=====

Basics
------

To make a request for payment you simply have to set a few information,
and then redirect the user to the generated approval url:

```php
<?php
...
// Get the Paypal Express Checkout Service
$expressCheckout = $this->get('lpweb_payment.paypal.express_checkout');

// Set the Invoice Number or OrderId
$expressCheckout->setInvoiceNumber(uniqid());

// Set the transaction description
$expressCheckout->setDescription("NearToAll - Quota Associativa");

// Set the total amount of the order
$expressCheckout->setAmout(10.00);

// Get the redirect url for payment approval
$approvalUrl = $expressCheckout->createPayment();

// Now you have to redirect the user to the paypal confirmation page
```


Requirements
============

`LpWebPaymentBundle` no further requirements are needed

License
=======

This bundle is under MIT license