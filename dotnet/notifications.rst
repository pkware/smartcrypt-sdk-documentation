Notifications
=============

Notifications are the mechanism used by the Smartcrypt SDK to provide insight to events that have happened in the background. Often they are useful to a user, but sometimes developers simply want to log certain events. This document describes the notifications available and how to use them.

Responding to Notifications
----------------------------

.. The text for these sections is, unfortunately, duplicated.
.. only:: csharp

    Some Notifications require a response. For example, when another user requests access to a Smartkey, a response should be provided to indicate whether or not access should be granted. Notifications can be responded to using `IDataStorage#RespondToNotification() <xmldoc/keyManagement/PKWARE.Smartcrypt.KeyManagement.IDataStorage.html>`_.

.. only:: java

    Some Notifications require a response. For example, when another user requests access to a Smartkey, a response should be provided to indicate whether or not access should be granted. Notifications can be responded to using `DataStorage#respondToNotification() <javadoc/com/pkware/smartcrypt/keymanagement/DataStorage.html#respondToNotification(com.pkware.smartcrypt.metaclient.Notification,java.lang.String)>`_.

Technical details
-------------------

.. The text for these sections is, unfortunately, duplicated.
.. only:: csharp

    For technical information about the types of Notifications and their priorities, see subclasses of `Notification <xmldoc/metaclient/PKWARE.Smartcrypt.MetaClient.Notification.html>`_ and `NotificationPriority <xmldoc/metaclient/PKWARE.Smartcrypt.MetaClient.NotificationPriority.html>`_.

.. only:: java

    For technical information about the types of Notifications and their priorities, see subclasses of `Notification <javadoc/com/pkware/smartcrypt/metaclient/Notification.html>`_ and `NotificationPriority <javadoc/com/pkware/smartcrypt/metaclient/NotificationPriority.html>`_.
