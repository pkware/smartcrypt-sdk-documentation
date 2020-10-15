# MetaClient

MetaClient is a low-level component in the Smartcrypt SDK, responsible for communicating with the PKWARE Enterprise Manager,
ensuring integrity of those communications, enforcing policies and enabling use of the SDK offline. MetaClient is very
powerful, but comes with a lot of sharp edges. SDK users are encouraged to interact with the SDK through other APIs, like
those of the [`SmartcryptKeyManagement`][SmartcryptKeyManagement] type.

## Setup
The most basic setup is relatively straightforward.

=== "Java"
    ```java hl_lines="10"
    import com.pkware.smartcrypt.metaclient.MetaClient;
    import com.pkware.smartcrypt.metaclient.NativeMetaClient;
    import com.pkware.smartcrypt.metaclient.Platform;
    import com.pkware.smartcrypt.protocol.SiteData;
    import java.util.UUID;

    public static MetaClient setUp() {
      return new NativeMetaClient.Builder()
                .appName(SiteData.APPLICATION_SMARTCRYPT)
                // TODO Fill in your application version
                .appVersion("9.99.9999")
                .deviceName(UUID.randomUUID().toString())
                .platform(Platform.LINUX)
                .build();
    }
    ```

=== "Kotlin"
    ```kotlin hl_lines="9"
    import com.pkware.smartcrypt.metaclient.MetaClient
    import com.pkware.smartcrypt.metaclient.NativeMetaClient
    import com.pkware.smartcrypt.metaclient.Platform
    import com.pkware.smartcrypt.protocol.SiteData
    import java.util.UUID

    fun setUp(): MetaClient = NativeMetaClient.Builder()
        .appName(SiteData.APPLICATION_SMARTCRYPT)
        // TODO Fill in your application version
        .appVersion("9.99.9999")
        .deviceName(UUID.randomUUID().toString())
        .platform(Platform.LINUX)
        .build()
    ```

### Lifecycle
A single instance of `MetaClient` should generally be treated as a singleton by your application. You typically want
all operations to use that one instance. Using multiple instances will not cause problems, but will results in a lot
more network traffic, since the data won't be cached locally.


[SmartcryptKeyManagement]: /api/index.html?com/pkware/smartcrypt/keymanagement/SmartcryptKeyManagement.html