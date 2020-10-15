# Logging

By default, all log statements will be printed to the console. This works fine for development, but is not recommended for production. Instead, it is recommended that you provide a logging target that suits your application's production environment.

## Configuring Logging

To customize logging, subclass the [Logger] class and provide an instance of your class to the [MetaClient Builder].

The most common behavior toggle is to change the logging level. Use the [setter] to set the level you desire.

You can also control the format of the `tag`. The `tag` is useful for differentiating multiple threads or instances. You can customize the `tag` by overriding the [formatTag] method.

## Common Strategies

### Enable trace logging
Trace logging is particularly useful when trying to debug connection issues and when contacting PKWARE support.

=== "Java"

    ```java hl_lines="8 9 12"
    import com.pkware.smartcrypt.metaclient.Logger;
    import com.pkware.smartcrypt.metaclient.MetaClient;
    import com.pkware.smartcrypt.metaclient.NativeMetaClient;

    // Use the ConsoleLogger from the SDK, or a custom implementation you write
    Logger logger = new ConsoleLogger();

    // Set the custom log level
    logger.setLevel(Logger.Level.TRACE);

    MetaClient metaClient = new NativeMetaClient.Builder()
        .logger(logger)
        // Set other properties
        .build();
    ```

=== "Kotlin"

    ```kotlin hl_lines="8 9 12"
    import com.pkware.smartcrypt.metaclient.Logger
    import com.pkware.smartcrypt.metaclient.MetaClient
    import com.pkware.smartcrypt.metaclient.NativeMetaClient

    // Use the ConsoleLogger from the SDK, or a custom implementation you write
    val logger = ConsoleLogger()

    // Set the custom log level
    logger.level = Logger.Level.TRACE

    val metaClient = NativeMetaClient.Builder()
        .logger(logger)
        // Set other properties
        .build()
    ```

### Disable logging
There is no toggle to disable logging. Instead, set the [level][setter] to [ERROR] and provide an empty implementation of the [log] method.

### Change logging destinations after initialization

You may want to change where messages are logged during the execution of your program. To do this, customize your `Logger` subclass to maintain the information of where, when, and how to log. This gives you full control over the structure, threading, and lifecycles of your application logging.

[Logger]: ../../api/com/pkware/smartcrypt/metaclient/Logger.html
[MetaClient Builder]: ../../api/com/pkware/smartcrypt/metaclient/NativeMetaClient.Builder.html#logger(com.pkware.smartcrypt.metaclient.Logger)
[setter]: ../../api/com/pkware/smartcrypt/metaclient/Logger.html#setLevel-com.pkware.smartcrypt.metaclient.Logger.Level-
[formatTag]: ../../api/com/pkware/smartcrypt/metaclient/Logger.html#formatTag--
[ERROR]: ../../api/com/pkware/smartcrypt/metaclient/Logger.Level.html#ERROR
[log]: ../../api/com/pkware/smartcrypt/metaclient/Logger.html#log-com.pkware.smartcrypt.metaclient.Logger.Level-java.lang.String-java.lang.String-java.lang.Throwable-