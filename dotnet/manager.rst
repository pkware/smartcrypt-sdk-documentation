Smartcrypt Enterprise Manager Configuration
===========================================

The Smartcrypt Enterprise Manager (SEM) is the administrative console used to manage configurations for the Smartcrypt SDK. Some initial configuration is required before using the SDK.

Before proceeding, it is recommended that you have a good understanding of :doc:`Smartkeys <faq>`.

.. _setting-up-an-application:

Setting up an Application
-------------------------

In the SEM, set up an Application to represent the software you're developing.

#. As a SEM administrator, log into your SEM
#. Navigate to the ``SDK`` section
#. On the ``Applications`` page, add an Application
#. You can name the application whatever you want, but this value must exactly match what you provide to ``MetaClient.Builder#AppName(string)``
#. In the ``Kinds`` field, enter all the kinds of Smartkeys that you want your application to support. See :doc:`Smartkey Strategies <smartkeyStrategies>` for advice on populating this field, and the :doc:`FAQ <faq>` for more information about Smartkeys.

Once your Application is configured, you are ready to use the SDK with user-owned Smartkeys! Read on for information about server-owned Smartkeys.

Server-owned Smartkeys
----------------------

Server-owned Smartkeys are useful in scenarios where you wish to distribute a Smartkey to an entire group of people such as an Active Directory group.

Assuming you're already logged into the SEM,

#. Navigate to the ``Application Keys`` tab
#. The ``Name`` field is the primary way for humans to identify the Smartkey. Often this matches the name of the Active Directory group, or is a summary of the purpose of the Smartkey.
#. The ``Kind`` is used to link the Smartkey to an Application. It cannot be changed once the key is created.
#. In the ``Participants`` field, select the users and groups who should have access to this Smartkey
#. In the ``Comments`` field, describe the intended uses case of the key and why certain participants are included. For example, "Used for encrypting credit card numbers in our invoicing system. The customer service group has access for issuing refunds and sales has access for adding new clients."
#. If your application requires a non-rotatable Smartkey, indicate this. Once the key is created, this cannot be changed. See the :doc:`FAQ <faq>` for help on deciding whether or not you need a non-rotatable Smartkey.
#. Save your new Smartkey

Once your Smartkey is created, all of its participants will immediately be able to use it. Be sure that the ``Kind`` you specified for the key is one of the ``Kinds`` mapped for your Application.
