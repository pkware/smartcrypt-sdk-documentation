# Smartkey segregation

The PKWARE Enterprise Manager segregates Smartkeys by the Application they are registered for. This means that an Application will only receive the keys it is expecting and was designed for, not the keys created within the context of another app, which was potentially created by another team.

This is best explain by example. Consider two applications: the Smartcrypt Desktop product created by PKWARE, and a custom application written by your development team. Your application will typically fall into one of two buckets - either it wants to use the exact same keys as the Smartcrypt Desktop application is using, or it wants to use keys managed independently of what occurs in the Smartcrypt Desktop application.

## Share keys with Smartcrypt Desktop and Mobile
In the first case, wanting to use the exact same keys as Smartcrypt Desktop, the setup is straightforward. When you [tell the SDK which application you are][Builder#appName], say you're [Smartcrypt][SiteData#smartcrypt]!

=== "Java"
    ```java hl_lines="5"
    import com.pkware.smartcrypt.metaclient.NativeMetaClient;
    import com.pkware.smartcrypt.protocol.SiteData;

    public static NativeMetaClient.Builder configureForSmartcrypt(NativeMetaClient.Builder builder) {
        return builder.appName(SiteData.APPLICATION_SMARTCRYPT);
    }
    ```

=== "Kotlin"
    ```kotlin hl_lines="5"
    import com.pkware.smartcrypt.metaclient.NativeMetaClient
    import com.pkware.smartcrypt.protocol.SiteData

    fun NativeMetaClient.Builder.configureForSmartcrypt(): NativeMetaClient.Builder =
        appName(SiteData.APPLICATION_SMARTCRYPT)
    ```

By indicating that your application is Smartcrypt, the PKWARE Enterprise Manager will give it all the same keys as the Smartcrypt Desktop app. This is especially useful for back office application that only use Community keys, or user-facing applications that want to delegate key creation/modification policies to Smartcrypt Desktop, but want users to be able to perform unique operations with those keys.

### PKWARE Enterprise Manager configuration
There is no additional Manager configuration required when sharing keys with Smartcrypt Desktop.

## Isolate keys from Smartcrypt Desktop and Mobile
When wanting to use keys that should not be shared with Smartcrypt Desktop, the setup is a bit more involved, and requires some planning.

### Motivation
There are good reasons for wanting to segregate your application's keys from those of Smartcrypt Desktop. For example, consider a scenario where your Archive policy allows users to create new Smartkeys, but in your custom application you want to tightly control which keys get used. In this case, you don't want the newly created keys from Smartcrypt Desktop spilling into your application. Or vice-versa, perhaps your application creates many keys, and you don't want those to get listed as options in the Smartcrypt Desktop UI. A theoretical example of this is a chat app, that creates a new Smartkey for each conversation.

### SDK configuration
To configure the SDK, provide the name of your custom application when you [tell the SDK which application you are][Builder#appName]. For example, `"BackOfficeReportExporter"` or `"CorporateChat"`. The important thing is to **not** call yourself [Smartcrypt][SiteData#smartcrypt].

### PKWARE Enterprise Manager configuration
Manager configuration is required when using this setup. Have a PKWARE Enterprise Manager admin follow the [PKWARE Enterprise Manager SDK Applications][server-sdk-application] instructions to get going. Be sure to provide the same application name as you are providing the SDK.


[Builder#appName]: /api/com/pkware/smartcrypt/metaclient/NativeMetaClient.Builder.html#appName-java.lang.String-
[SiteData#smartcrypt]: /api/com/pkware/smartcrypt/protocol/SiteData.html#APPLICATION_SMARTCRYPT
[server-sdk-application]: https://support.pkware.com/home/smar/latest/sem-reference/8-sdk/applications