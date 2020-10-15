# Troubleshooting

## Self-signed SSL certificate
When using a self-signed SSL certificate on your Smartcrypt Enterprise Manager, you'll likely encounter an error like
those below. Self-signed SSL certificates are not supported.

```java
com.pkware.smartcrypt.metaclient.MetaClientException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
Details: GET https://demoserver.smartcrypt.com/mds/api/v1.0/clusterInfo 0

at com.pkware.smartcrypt.metaclient.NativeMetaClient.loginManagedAccount(Native Method)
at com.pkware.smartcrypt.keymanagement.MetaClientAccountManagement.loginManagedAccount(MetaClientAccountManagement.java:26)
```

## Unable to connect to server
If unable to connect to the server, you will encounter error messages with varying degrees of clarity. The SDK relies on
the underlying platform (runtime & OS) to resolve the server and generate error messages. The following are error you
might encounter.

```java
com.pkware.smartcrypt.metaclient.MetaClientException: demoserver.smartcrypt.com: nodename nor servname provided, or not known
Details: GET https://demoserver.smartcrypt.com/mds/api/v1.0/clusterInfo 0

at com.pkware.smartcrypt.metaclient.NativeMetaClient.loginManagedAccount(Native Method)
at com.pkware.smartcrypt.keymanagement.MetaClientAccountManagement.loginManagedAccount(MetaClientAccountManagement.java:26)
```

To start troubleshooting, try to contact the server from code without the SDK. The following snippets demonstrate how
to do this. Be sure to replace the URL in the snippets with that of your server.

=== "Java"
    ```java hl_lines="13"
    import org.apache.http.client.methods.CloseableHttpResponse;
    import org.apache.http.client.methods.HttpUriRequest;
    import org.apache.http.client.methods.RequestBuilder;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClients;

    import java.io.IOException;

    public class Program {
      public static void main(String[] args) throws IOException {
        try (CloseableHttpClient client = HttpClients.createDefault()) {
          HttpUriRequest request = RequestBuilder
              .get("https://demoserver.smartcrypt.com/mds/api/v1.0/clusterInfo")
              .build();
          try (CloseableHttpResponse response = client.execute(request)) {
            System.out.println(response.getStatusLine());
          }
        }
      }
    }
    ```

=== "Kotlin"
    ```kotlin hl_lines="9"
    import org.apache.http.client.methods.RequestBuilder
    import org.apache.http.impl.client.HttpClients
    import java.io.IOException

    @Throws(IOException::class)
    fun main() {
      HttpClients.createDefault().use { client ->
        val request = RequestBuilder
            .get("https://demoserver.smartcrypt.com/mds/api/v1.0/clusterInfo")
            .build()
        client.execute(request).use { response -> println(response.statusLine) }
      }
    }
    ```