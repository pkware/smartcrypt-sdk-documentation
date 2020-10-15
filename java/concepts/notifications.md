# Notifications

Notifications are the mechanism used by the Smartcrypt SDK to provide insight to events that have happened in the background. Often they are useful to a user, but sometimes developers simply want to log certain events. This document describes the notifications available and how to use them.

## Responding to Notifications
Some Notifications require a response. For example, when another user requests access to a Smartkey, a response should be provided to indicate whether or not access should be granted. Notifications can be responded to using [DataStorage#respondToNotification()][respondToNotification].

## Technical details
For technical information about the types of Notifications and their priorities, see subclasses of [Notification] and [NotificationPriority].

[respondToNotification]: ../../api/com/pkware/smartcrypt/keymanagement/DataStorage.html#respondToNotification-com.pkware.smartcrypt.metaclient.Notification-java.lang.String-
[Notification]: ../../api/com/pkware/smartcrypt/metaclient/Notification.html
[NotificationPriority]: ../../api/com/pkware/smartcrypt/metaclient/NotificationPriority.html