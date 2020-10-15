# Key management

The key management component allows for querying, modifying, and creating Smartkeys.

## Setup

=== "Java"
    ```java hl_lines="8"
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement;
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagementImpl;
    import com.pkware.smartcrypt.metaclient.MetaClient;

    public static SmartcryptKeyManagement setup(MetaClient metaClient) {
        return new SmartcryptKeyManagementImpl.Builder()
                .metaClient(metaClient)
                // TODO Optionally provide a PersistenceCallback, if you want the SDK to work while offline. You need to implement this class yourself.
                .persistenceCallback(new FileSystemPersistenceCallback())
                .build();
    }
    ```

=== "Kotlin"
    ```kotlin hl_lines="8"
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagementImpl
    import com.pkware.smartcrypt.metaclient.MetaClient

    fun MetaClient.newSmartcryptKeyManagement(): SmartcryptKeyManagement = SmartcryptKeyManagementImpl.Builder()
        .metaClient(this)
        // TODO Optionally provide a PersistenceCallback, if you want the SDK to work while offline. You need to implement this class yourself.
        .persistenceCallback(new FileSystemPersistenceCallback())
        .build()
    ```

### Lifecycle
A single instance of `SmartcryptKeyManagement` should generally be treated as a singleton by your application. You
typically want all operations to use that one instance. Using multiple instances will not cause problems, but will
consume a lot more system resources, as more threads and background processes will get used.

## Login
There are two types of accounts in Smartcrypt: Managed and Unmanaged. A Managed account is one that is tied to an identity system like Active Directory. An Unmanaged account exists only in the PKWARE Enterprise Manager.

For the most part, the two account types work the same way, but during login, different calls have to be made based on the type of the account. Unmanaged accounts need to log in with [loginUnManagedAccount], while Managed accounts can choose between a regular login using a username/password combo, and a single sign-on style login, using Kerberos or Integrated Windows Authentication (IWA).

!!! example "Login unmanaged account"
    === "Java"
        ```java
        import com.pkware.smartcrypt.keymanagement.AccountManagement;
        import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement;

        public static void loginUnmanaged(SmartcryptKeyManagement keyManagement, String emailAddress, String password) {
            AccountManagement accountManagement = keyManagement.getAccountManagement();
            accountManagement.loginUnManagedAccount(emailAddress, password);
        }
        ```

    === "Kotlin"
        ```kotlin
        import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement

        fun loginUnmanaged(keyManagement: SmartcryptKeyManagement, emailAddress: String, password: String) {
            val accountManagement = keyManagement.getAccountManagement()
            accountManagement.loginUnManagedAccount(emailAddress, password)
        }
        ```

### Login managed account
For a managed account, you have to choose between a password-based login and single sign-on. The password based approach is recommended during development as it is significantly easier to set up, but single sign-on typically provides a better deployment process for production scenarios.

!!! example "Login managed account with password"
    === "Java"
        ```java
        import com.pkware.smartcrypt.keymanagement.AccountManagement;
        import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement;

        public static void loginManaged(SmartcryptKeyManagement keyManagement, String emailAddress, String password) {
            AccountManagement accountManagement = keyManagement.getAccountManagement();
            accountManagement.loginManagedAccount(emailAddress, password);
        }
        ```

    === "Kotlin"
        ```kotlin
        import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement

        fun loginManaged(keyManagement: SmartcryptKeyManagement, emailAddress: String, password: String) {
            val accountManagement = keyManagement.getAccountManagement()
            accountManagement.loginManagedAccount(emailAddress, password)
        }
        ```

#### Single sign-on
The Smartcrypt SDK supports single sign-on login flows for managed users on Windows and unix through a mechanism called
Implicit Login.

!!! example "Login managed account with single sign-on"
    === "Java"
        ```java
        import com.pkware.smartcrypt.keymanagement.AccountManagement;
        import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement;

        public static void loginSingleSignOn(SmartcryptKeyManagement keyManagement) {
            AccountManagement accountManagement = keyManagement.getAccountManagement();
            accountManagement.loginImplicitAccount();
        }
        ```

    === "Kotlin"
        ```kotlin
        import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement

        fun loginSingleSignOn(keyManagement: SmartcryptKeyManagement) {
            val accountManagement = keyManagement.getAccountManagement()
            accountManagement.loginImplicitAccount()
        }
        ```

##### Advantages of Implicit Login
Several features make Implicit Login the recommended authentication mechanism.

For back-office applications, Implicit Login provides the convenience of not needing to hard code, manually enter or
place the credentials into a configuration file. This improves security and makes it easier to deploy applications.

For end-user applications, companies typically want the end user to login with domain credentials when running the
application. In this case, Implicit Login affords the ability to skip a login prompt, enabling a seamless user
experience. This is particularly desirable when modifying existing business applications to incorporate Smartcrypt data
protection.

##### How it works
On Windows, Integrated Windows Authentication (IWA) is used. The identity of the Active Directory Domain User who
launches the process using the Smartcrypt SDK is used.

On MacOS and Linux, the Kerberos system is used to identify and authenticate the user. Domain-joined MacOS
installations are automatically configured. On Linux, the [kinit] program is used. When properly configured, Linux
machines will automatically run `kinit` when users perform a password-based system login.

On MacOS and Linux, the JVM must be configured to correctly integrate with the Kerberos system. Which Active Directory
Domain User identity is used depends on the configuration.

##### Configuring the JVM
The simplest way to get started is to set the `java.security.krb5.realm` and `java.security.krb5.kdc` system properties:

=== "Java"
    ```java
    public static void setupKerberos() {
        // Provide the Kerberos realm that your user is in
        System.setProperty("java.security.krb5.realm", "EXAMPLE.COM");
        // Provide the name of a Kerberos ticketing server
        System.setProperty("java.security.krb5.kdc", "dc01.example.com");
    }
    ```

=== "Kotlin"
    ```kotlin
    fun setupKerberos() {
        // Provide the Kerberos realm that your user is in
        System.setProperty("java.security.krb5.realm", "EXAMPLE.COM")
        // Provide the name of a Kerberos ticketing server
        System.setProperty("java.security.krb5.kdc", "dc01.example.com")
    }
    ```

For a production rollout, you should set the `java.security.krb5.conf` system property to point at the `krb5.conf` on
the host system. Consult your Kerberos or Windows Domain Administrator for more information on the details of your
Kerberos environment.

##### Diagnosing JVM problems
To start, ensure that you can successfully acquire a Kerberos TGT with the [kinit] utility. When this is successful,
proceed.

The Smartcrypt SDK has a built-in diagnostic utility. To activate it

1. Set the logging level to ``TRACE``
1. Run the `java` command with the flag
`-Djava.security.debug=net,gssloginconfig,configfile,configparser,logincontext` to enable detailed JVM logging
1. Call [MetaClient#loginImplicitAccount()].
This will cause the utility to write diagnostics to the configured [Logger].

The diagnostic utility will inspect your current configuration for common problems. The additional logging may be
helpful in determining how the JVM is interpreting the current Kerberos setup.

## Create Smartkey

=== "Java"
    ```java hl_lines="10-13 16"
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement;
    import com.pkware.smartcrypt.keymanagement.SmartkeySpec;
    import com.pkware.smartcrypt.keymanagement.Smartkeys;
    import com.pkware.smartcrypt.keymanagement.smartkeys.Smartkey;
    import io.reactivex.Observable;

    public static void createSmartkey(SmartcryptKeyManagement keyManagement) {
        Smartkeys smartkeys = keyManagement.getSmartkeys();

        // To create a new Smartkey, prepare a SmartkeySpec with the information for the new key.
        SmartkeySpec createSmartkeySpec = new SmartkeySpec();
        createSmartkeySpec.name = "Sample";
        createSmartkeySpec.addAccess.add("john.doe@example.com");

        // Key creation is synchronous, but you can be notified of updates to the key by listening to the Observable
        Observable<Smartkey> keyObservable = smartkeys.create(createSmartkeySpec);
        Smartkey smartkey = keyObservable
                .doOnEach(genericSmartkeyNotification -> {
                    Smartkey key = genericSmartkeyNotification.getValue();
                    System.out.println("Got revision " + key.getRevision() + " for created Smartkey " + key.getName());
                })
                .blockingSingle();
    }
    ```

=== "Kotlin"
    ```kotlin hl_lines="7-10 13"
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement
    import com.pkware.smartcrypt.keymanagement.SmartkeySpec

    fun createSmartkey(keyManagement: SmartcryptKeyManagement) {
        val smartkeys = keyManagement.smartkeys

        // To create a new Smartkey, prepare a SmartkeySpec with the information for the new key.
        val createSmartkeySpec = SmartkeySpec()
        createSmartkeySpec.name = "Sample"
        createSmartkeySpec.addAccess.add("john.doe@example.com")

        // Key creation is synchronous, but you can be notified of updates to the key by listening to the Observable
        val keyObservable = smartkeys.create(createSmartkeySpec)
        val smartkey = keyObservable
            .doOnEach { genericSmartkeyNotification ->
                val key = genericSmartkeyNotification.value
                println("Got revision ${key!!.revision} for created Smartkey ${key.name}")
            }
            .blockingSingle()
    }
    ```

## Update Smartkey
To update a key, once again prepare a `SmartkeySpec`. Most types of keys can be updated in some fashion.

=== "Java"
    ```java hl_lines="10-12 15"
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement;
    import com.pkware.smartcrypt.keymanagement.SmartkeySpec;
    import com.pkware.smartcrypt.keymanagement.Smartkeys;
    import com.pkware.smartcrypt.keymanagement.smartkeys.Smartkey;
    import io.reactivex.Observable;

    public static void updateSmartkey(SmartcryptKeyManagement keyManagement, Smartkey smartkey) {
        Smartkeys smartkeys = keyManagement.getSmartkeys();

        SmartkeySpec updateSmartkeySpec = new SmartkeySpec();
        updateSmartkeySpec.name = "Updated Sample";
        updateSmartkeySpec.removeAccess.add("john.doe@example.com");

        // Key updates are synchronous, but you can be notified of updates to the key by listening to the Observable
        Observable<Smartkey> updatedKeyObservable = smartkeys.update(smartkey, updateSmartkeySpec);
        updatedKeyObservable.subscribe(key -> System.out.println("Got updated Smartkey " + key.getName()));
    }
    ```

=== "Kotlin"
    ```kotlin hl_lines="8-10 13"
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement
    import com.pkware.smartcrypt.keymanagement.SmartkeySpec
    import com.pkware.smartcrypt.keymanagement.smartkeys.Smartkey

    fun updateSmartkey(keyManagement: SmartcryptKeyManagement, smartkey: Smartkey) {
        val smartkeys = keyManagement.smartkeys

        val updateSmartkeySpec = SmartkeySpec()
        updateSmartkeySpec.name = "Updated Sample"
        updateSmartkeySpec.removeAccess.add("john.doe@example.com")

        // Key updates are synchronous, but you can be notified of updates to the key by listening to the Observable
        val updatedKeyObservable = smartkeys.update(smartkey, updateSmartkeySpec)
        updatedKeyObservable.subscribe { key -> println("Got updated Smartkey ${key.name}") }
    }
    ```

## Working offline
By default, the SDK stores key and account information in-memory. In this configuration, the application will be able
to use Smartkeys offline only until the process dies. Then, if the device is offline when the application again starts
up, it will not have access to Smartkeys and account data. Often it is desireable for an application to continue
working under these conditions. In that case, it is essential that account data get saved for offline use.

The mechanism in the SDK to provide hooks for saving data is called the [`PersistenceCallback`][PersistenceCallback].
The [`PersistenceCallback`][PersistenceCallback] is invoked by the SDK with the data modifications that need to be
made. You must implement a [`PersistenceCallback`][PersistenceCallback] of your own - the SDK does not come with an
implementation.

!!! note
    Be sure to review the [`PersistenceCallback`][PersistenceCallback] documentation when implementing your own class.

=== "Java"
    ```java hl_lines="16 17 20 21"
    import com.pkware.smartcrypt.keymanagement.PersistenceCallback;
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagementImpl;

    import java.io.BufferedWriter;
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Paths;
    import java.util.Collections;
    import java.util.Map;
    import java.util.Set;

    import static java.util.stream.Collectors.toMap;

    public static SmartcryptKeyManagementImpl.Builder configureDataPersistence(SmartcryptKeyManagementImpl.Builder builder) {

        // It is up to you how you store your data. In files, in a database, on the network, etc.
        return builder.persistenceCallback(new FilePersistenceCallback());
    }

    // Note: This class is not part of the SDK
    class FilePersistenceCallback implements PersistenceCallback {
        private final static String folder = "serializedSmartcryptData";

        @Override
        public boolean onSaveData(Map<String, String> toSave, Set<String> toDelete) {
            try {
                Files.createDirectory(Paths.get(folder));
                for (Map.Entry<String, String> datum : toSave.entrySet()) {
                    try (BufferedWriter writer = Files.newBufferedWriter(Paths.get(folder, datum.getKey()))) {
                        writer.write(datum.getValue());
                    }
                }

                for (String name : toDelete) {
                    Files.deleteIfExists(Paths.get(folder, name));
                }
                return true;
            } catch (IOException e) {
                return false;
            }
        }

        @Override
        public Map<String, String> onLoadData() {
            try (Stream<Path> stream = Files.list(Paths.get(folder))) {
                return stream.collect(toMap(
                        path -> path.getFileName().toString(),                      // Key
                        fileName -> {                                               // Value
                            try {
                                return new String(Files.readAllBytes(fileName));
                            } catch (IOException e) {
                                e.printStackTrace();
                                return "";
                            }
                        }
                ));
            } catch (IOException e) {
                return Collections.emptyMap();
            }
        }
    }
    ```

=== "Kotlin"
    ```java hl_lines="10 11 14 15"
    import com.pkware.smartcrypt.keymanagement.PersistenceCallback
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagementImpl
    import java.io.IOException
    import java.nio.file.Files
    import java.nio.file.Paths
    import java.util.stream.Collectors.toMap

    fun configureDataPersistence(builder: SmartcryptKeyManagementImpl.Builder): SmartcryptKeyManagementImpl.Builder {

        // It is up to you how you store your data. In files, in a database, on the network, etc.
        return builder.persistenceCallback(FilePersistenceCallback())
    }

    // Note: This class is not part of the SDK
    class FilePersistenceCallback : PersistenceCallback {
        private val folder = "serializedSmartcryptData"

        override fun onSaveData(toSave: Map<String, String>, toDelete: Set<String>): Boolean = try {
            Files.createDirectory(Paths.get(folder))
            for ((key, value) in toSave) {
                Files.newBufferedWriter(Paths.get(folder, key)).use { writer ->
                    writer.write(value)
                }
            }
            for (name in toDelete) {
                Files.deleteIfExists(Paths.get(folder, name))
            }
            true
        } catch (e: IOException) {
            false
        }

        override fun onLoadData(): Map<String, String> = try {
            Files.list(Paths.get(folder)).use { stream ->
                stream.collect(toMap(
                    { path -> path.fileName.toString() },             // Key
                    { fileName ->                                     // Value
                        return@toMap try {
                            String(Files.readAllBytes(fileName))
                        } catch (e: IOException) {
                            e.printStackTrace()
                            ""
                        }
                    }
                ))
            }
        } catch (e: IOException) {
            emptyMap()
        }
    }
    ```

Once your `PersistenceCallback` is configured, you must modify your application to restore data into the SDK when it starts up.

=== "Java"
    ```java hl_lines="7-9"
    import com.pkware.smartcrypt.keymanagement.DataStorage;
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement;
    
    public static void restoreSmartcryptData(SmartcryptKeyManagement keyManagement) {
        DataStorage dataStorage = keyManagement.getDataStorage();

        // This causes the SDK to call your PersistenceCallback to load data
        // You control both the threading and timing of how data are restored
        dataStorage.restore();
    }
    ```

=== "Kotlin"
    ```kotlin hl_lines="6-8"
    import com.pkware.smartcrypt.keymanagement.SmartcryptKeyManagement
    
    fun restoreSmartcryptData(keyManagement: SmartcryptKeyManagement) {
        val dataStorage = keyManagement.getDataStorage();

        // This causes the SDK to call your PersistenceCallback to load data.
        // You control both the threading and timing of how data are restored
        dataStorage.restore();
    }
    ```

[MetaClient#loginImplicitAccount()]: /api/com/pkware/smartcrypt/metaclient/MetaClient.html#loginImplicitAccount()
[Logger]: /api/com/pkware/smartcrypt/metaclient/Logger.html
[kinit]: https://web.mit.edu/kerberos/krb5-latest/doc/user/user_commands/kinit.html
[PersistenceCallback]: /api/index.html?com/pkware/smartcrypt/keymanagement/PersistenceCallback.html