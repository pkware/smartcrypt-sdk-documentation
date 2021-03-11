# File encryption
The file encryption component uses ZIP files to protect sensitive data. ZIP files are an ideal format, as a lot of
software are aware of the format and work well with it. ZIPs also come with a range of desireable properties:

- Easy-to-use compression of data _prior to encryption_.
- Support for encrypting an entire folder hierarchy, thereby keeping related files together and allowing them to be
  easily shared.
- File name encryption, thereby protecting potentially sensitive document file names.
- Embedded support for Smartkeys, including Community keys and Contingency Groups.

## Setup

To get started, you'll need to create a new instance of [`UnstructuredData`][UnstructuredData]. See the
[MetaClient] docs for details on how to obtain the `MetaClient` instance.

=== "Java"

    ```java
    import com.pkware.smartcrypt.metaclient.MetaClient;
    import com.pkware.smartcrypt.unstructured.UnstructuredData;
    import com.pkware.smartcrypt.unstructured.UnstructuredDataImpl;

    public static UnstructuredData setUp(MetaClient metaClient) {
      return new UnstructuredDataImpl.Builder()
        .metaClient(metaClient)
        .license("<Your license here>")
        .build();
    }
    ```

=== "Kotlin"

    ```kotlin
    import com.pkware.smartcrypt.metaclient.MetaClient
    import com.pkware.smartcrypt.unstructured.UnstructuredData
    import com.pkware.smartcrypt.unstructured.UnstructuredDataImpl

    fun setUp(metaClient: MetaClient): UnstructuredData =
      UnstructuredDataImpl.Builder()
        .metaClient(metaClient)
        .license("<Your license here>")
        .build()
    ```

Instances of `UnstructuredData` are thread safe and should be reused throughout your application.

## Create a ZIP
The basic steps of creating a ZIP are the same regardless of which encryption parameters are used.

=== "Java"
    ```java
    import com.pkware.archive.zip.ZipArchiveEntry;
    import com.pkware.archive.zip.ZipArchiveOutputFile;
    import com.pkware.smartcrypt.unstructured.UnstructuredData;

    import java.io.IOException;
    import java.io.InputStream;
    import java.util.Map;

    public static void createZip(
      UnstructuredData unstructured,
      Map<String, InputStream> filesToArchive
    ) throws IOException {
      // A stream variant is also available
      try (ZipArchiveOutputFile zip = unstructured.newZipOutputFile("test.zip")) {
        for (Map.Entry<String, InputStream> dataToArchive : filesToArchive.entrySet()) {
          String entryName = dataToArchive.getKey();
          InputStream entryContent = dataToArchive.getValue();
          ZipArchiveEntry zipEntry = zip.createArchiveEntry(null, entryName);
          zip.addEntry(zipEntry, entryContent);
        }
      }
    }
    ```

=== "Kotlin"
    ```kotlin
    import com.pkware.smartcrypt.unstructured.UnstructuredData
    import java.io.IOException
    import java.io.InputStream

    @Throws(IOException::class)
    fun createZip(
      unstructured: UnstructuredData,
      filesToArchive: Map<String, InputStream>
    ) {
      // A stream variant is also available
      unstructured.newZipOutputFile("test.zip").use { zip ->
        for ((entryName, entryContent) in filesToArchive) {
          val zipEntry = zip.createArchiveEntry(null, entryName)
          zip.addEntry(zipEntry, entryContent)
        }
      }
    }
    ```

### Configuring encryption
By default, the SDK chooses an encryption method that provides strong security and is well suited for most applications.
It may be desirable to change the encryption method (also called the encryption algorithm) used to create new ZIP entries.
The SDK provides a simple of way setting the most common and useful encryption methods.

=== "Java"
    ```java
    import com.pkware.archive.ArchiveEntry;
    import com.pkware.archive.zip.ZipArchiveOutput;

    public static void configureEncryptionMethod(ZipArchiveOutput zip) {
      // Configure encryption to use AE2 (for compatibility with other ZIP programs)
      zip.setCryptAlgorithm(ArchiveEntry.CRYPT_AE2);
      // Configure encryption to use AES-256
      zip.setCryptAlgorithm(ArchiveEntry.CRYPT_AES_256);
    }
    ```

=== "Kotlin"
    ```kotlin
    import com.pkware.archive.ArchiveEntry
    import com.pkware.archive.zip.ZipArchiveOutput

    fun configureEncryptionMethod(zip: ZipArchiveOutput) {
      // Configure encryption to use AE2 (for compatibility with other ZIP programs)
      zip.cryptAlgorithm = ArchiveEntry.CRYPT_AE2.toInt()
      // Configure encryption to use AES-256
      zip.cryptAlgorithm = ArchiveEntry.CRYPT_AES_256.toInt()
    }
    ```

### Configuring compression
By default, the SDK chooses compression setting that balances speed with compressed size. However, it is possible to
change the compression method. One might use a different compression algorithm , such as deflate, store, and LZMA, or
specify a different compression level (higher for smaller ZIPs, lower for faster processing) as the application requires.

=== "Java"
    ```java
    import com.pkware.archive.ArchiveEntry;
    import com.pkware.archive.zip.ZipArchiveOutput;

    public static void compressionMethod(ZipArchiveOutput zip) {
      // Configure compression to use the deflate algorithm at level 6
      zip.setCompressMethod(ArchiveEntry.COMPRESS_METHOD_DEFLATE, 6);
      // Configure compression to use the store algorithm (no compression)
      zip.setCompressMethod(ArchiveEntry.COMPRESS_METHOD_STORE);
    }
    ```

=== "Kotlin"
    ```kotlin
    import com.pkware.archive.ArchiveEntry
    import com.pkware.archive.zip.ZipArchiveOutput

    fun compressionMethod(zip: ZipArchiveOutput) {
      // Configure compression to use the deflate algorithm at level 6
      zip.setCompressMethod(ArchiveEntry.COMPRESS_METHOD_DEFLATE, 6)
      // Configure compression to use the store algorithm (no compression)
      zip.compressMethod = ArchiveEntry.COMPRESS_METHOD_STORE
    }
    ```

## Encryption
Encryption using both passwords and Smartkeys is straightforward. See the
[Key Management docs] for details on how to obtain a Smartkey.

=== "Java"
    ```java
    import com.pkware.archive.zip.ZipArchiveOutput;
    import com.pkware.smartcrypt.keymanagement.smartkeys.Smartkey;
    import com.pkware.smartcrypt.metaclient.MetaClientException;
    import com.pkware.smartcrypt.unstructured.EncryptionSpec;
    import com.pkware.smartcrypt.unstructured.UnstructuredData;

    public static void contingencyGroupWithSmartkey(
      UnstructuredData unstructured,
      Smartkey smartkey,
      // Create the ZIP as you normally would
      ZipArchiveOutput zip
    ) throws MetaClientException {
      EncryptionSpec spec = new EncryptionSpec();
      // Optionally add a password here as well
      spec.smartkey = smartkey;

      unstructured.encrypt(zip, spec);

      // Proceed to add files to the ZIP as you normally would
    }
    ```

=== "Kotlin"
    ```kotlin
    import com.pkware.archive.zip.ZipArchiveOutput
    import com.pkware.smartcrypt.keymanagement.smartkeys.Smartkey
    import com.pkware.smartcrypt.metaclient.MetaClientException
    import com.pkware.smartcrypt.unstructured.EncryptionSpec
    import com.pkware.smartcrypt.unstructured.UnstructuredData

    @Throws(MetaClientException::class)
    fun contingencyGroupWithSmartkey(
      unstructured: UnstructuredData,
      smartkey: Smartkey,
      // Create the ZIP as you normally would
      zip: ZipArchiveOutput
    ) {
      val spec = EncryptionSpec()
      // Optionally add a password here as well
      spec.smartkey = smartkey;

      unstructured.encrypt(zip, spec)

      // Proceed to add files to the ZIP as you normally would
    }
    ```

!!! important
    In order for Smartcrypt Contingency Groups to apply, you must run your application as Smartcrypt.

### Contingency group without Smartkey
The recommended way to gain access to the Contingency Group functionality of Smartcrypt is to use Smartkeys when
encrypting. However, there may be some scenarios in which using a Smartkey is not an option. For these scenarios, we also provide
the option of skipping the Smartkey, but still gaining the benefits of Contingency Groups.

The process is very similar to the encrypting with a Smartkey, but a password __must__ be provided this time.
=== "Java"
    ```java
    import com.pkware.archive.zip.ZipArchiveOutput;
    import com.pkware.smartcrypt.metaclient.MetaClientException;
    import com.pkware.smartcrypt.unstructured.EncryptionSpec;
    import com.pkware.smartcrypt.unstructured.UnstructuredData;

    public static void contingencyGroupWithoutSmartkey(
      UnstructuredData unstructured,
      // Create the ZIP as you normally would
      ZipArchiveOutput zip
    ) throws MetaClientException {
      EncryptionSpec spec = new EncryptionSpec();
      // The password must be provided if not specifying a Smartkey to gain contingency benefits
      spec.password = "secret secret";

      unstructured.encrypt(zip, spec);

      // Proceed to add files to the ZIP as you normally would
    }
    ```

=== "Kotlin"
    ```kotlin
    import com.pkware.archive.zip.ZipArchiveOutput
    import com.pkware.smartcrypt.metaclient.MetaClientException
    import com.pkware.smartcrypt.unstructured.EncryptionSpec
    import com.pkware.smartcrypt.unstructured.UnstructuredData

    @Throws(MetaClientException::class)
    fun contingencyGroupWithoutSmartkey(
      unstructured: UnstructuredData,
      // Create the ZIP as you normally would
      zip: ZipArchiveOutput
    ) {
      val spec = EncryptionSpec()
      // The password must be provided if not specifying a Smartkey to gain contingency benefits
      spec.password = "secret secret";

      unstructured.encrypt(zip, spec)

      // Proceed to add files to the ZIP as you normally would
    }
    ```

!!! important
    In order for Smartcrypt Contingency Groups to apply, you must run your application as Smartcrypt.

## Decryption
Once you have an `UnstructuredData` instance, decrypting ZIP archives encrypted using a Smartkey is easy! If you do not
have access to the Smartkey, but are part of the Contingency Group, this snippet will also grant you access to the files.

=== "Java"
    ```java
    import com.pkware.archive.zip.ZipArchiveEntry;
    import com.pkware.archive.zip.ZipFile;
    import com.pkware.smartcrypt.unstructured.UnstructuredData;
    import java.io.File;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.InputStream;
    import java.io.OutputStream;

    public static void decrypt(UnstructuredData unstructuredData) throws IOException {
      try (ZipFile encrypted = unstructuredData.newZipFile(new File("encrypted.zip"))) {
        ZipArchiveEntry entry = encrypted.getEntry("example.txt");
        try (OutputStream fileStream = new FileOutputStream("example.txt")) {
          try (InputStream decryptStream = encrypted.getInputStream(entry)) {
            decryptStream.transferTo(fileStream);
          }
        }
      }
    }
    ```

=== "Kotlin"
    ```kotlin
    import com.pkware.archive.zip.ZipArchiveEntry
    import com.pkware.archive.zip.ZipFile
    import com.pkware.smartcrypt.unstructured.UnstructuredData
    import java.io.File
    import java.io.FileOutputStream
    import java.io.IOException
    import java.io.InputStream
    import java.io.OutputStream

    @Throws(IOException::class)
    fun decrypt(unstructuredData: UnstructuredData) {
      unstructuredData.newZipFile(File("encrypted.zip")).use { encrypted ->
        val entry = encrypted.getEntry("example.txt")
        FileOutputStream("example.txt").use { fileStream ->
          encrypted.getInputStream(entry).use { decryptStream ->
            decryptStream.transferTo(fileStream);
          }
        }
      }
    }
    ```

[UnstructuredData]: ..//api/index.html?com/pkware/smartcrypt/unstructured/UnstructuredData.html
[MetaClient]: ../keyManagement/metaClient
[Key Management docs]: ../keyManagement/#create-smartkey
