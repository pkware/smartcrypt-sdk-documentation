Logging
=============

By default, all log statements will be printed to the console. This works fine for development, but is not recommended for production. Instead, it is recommended that you provide a logging target that suits your application's production environment.

Configuring Logging
----------------------------

.. The text for these sections is, unfortunately, duplicated.
.. only:: csharp

    To customize logging, subclass the `Logger <xmldoc/metaclient/PKWARE.Smartcrypt.MetaClient.Logging.Logger.html>`_ class and provide an instance of your class to the `MetaClient Builder <xmldoc/metaclient/PKWARE.Smartcrypt.MetaClient.MetaClient.Builder.html#PKWARE_Smartcrypt_MetaClient_MetaClient_Builder_Logger_PKWARE_Smartcrypt_MetaClient_Logging_Logger>`_.

    The most common behavior toggle is to change the logging level. Set the `Level <xmldoc/metaclient/PKWARE.Smartcrypt.MetaClient.Logging.Logger.html#PKWARE_Smartcrypt_MetaClient_Logging_Logger_Level>`_ property to the level you desire.

    You can also control the format of the `tag`. The `tag` is useful for differentiating multiple threads or instances and includes the line number, method name, and file information about the logging statement. You can customize the `tag` by overriding the `FormatTag <xmldoc/metaclient/PKWARE.Smartcrypt.MetaClient.Logging.Logger.html#PKWARE_Smartcrypt_MetaClient_Logging_Logger_FormatTag_System_Int32_System_String_System_String>`_ method.

.. only:: java

    To customize logging, subclass the `Logger <javadoc/com/pkware/smartcrypt/metaclient/Logger.html>`_ class and provide an instance of your class to the `MetaClient Builder <javadoc/com/pkware/smartcrypt/metaclient/NativeMetaClient.Builder.html#logger(com.pkware.smartcrypt.metaclient.Logger)>`_.

    The most common behavior toggle is to change the logging level. Use the `setter <javadoc/com/pkware/smartcrypt/metaclient/Logger.html#setLevel(com.pkware.smartcrypt.metaclient.Logger.Level)>`_ to set the level you desire.

    You can also control the format of the `tag`. The `tag` is useful for differentiating multiple threads or instances. You can customize the `tag` by overriding the `formatTag <javadoc/com/pkware/smartcrypt/metaclient/Logger.html#formatTag()>`_ method.

Common Strategies
----------------------

Disable logging
^^^^^^^^^^^^^^^^^^

.. only:: csharp

    There is no toggle to disable logging. Instead, set the `Level <xmldoc/metaclient/PKWARE.Smartcrypt.MetaClient.Logging.Logger.html#PKWARE_Smartcrypt_MetaClient_Logging_Logger_Level>`_ to `Error` and provide an empty implementation of the `Log <xmldoc/metaclient/PKWARE.Smartcrypt.MetaClient.Logging.Logger.html#PKWARE_Smartcrypt_MetaClient_Logging_Logger_Log_PKWARE_Smartcrypt_MetaClient_Logging_Level_System_String_System_String_Exception>`_ method.

.. only:: java

    There is no toggle to disable logging. Instead, set the `level <javadoc/com/pkware/smartcrypt/metaclient/Logger.html#setLevel(com.pkware.smartcrypt.metaclient.Logger.Level)>`_ to `ERROR <javadoc/com/pkware/smartcrypt/metaclient/Logger.Level.html#ERROR>`_ and provide an empty implementation of the `log <javadoc/com/pkware/smartcrypt/metaclient/Logger.html#log(com.pkware.smartcrypt.metaclient.Logger.Level,java.lang.String,java.lang.String,java.lang.Throwable)>`_ method.

Change logging destinations after initialization
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You may want to change where messages are logged during the execution of your program. To do this, customize your `Logger` subclass to maintain the information of where, when, and how to log. This gives you full control over the structure, threading, and lifecycles of your application logging.
