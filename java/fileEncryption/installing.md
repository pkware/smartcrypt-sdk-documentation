The binaries needed for the SDK are hosted in a Maven repository by PKWARE.

!!! info
    The repository is password protected; contact [PKWARE Sales and Support](mailto:sales@pkware.com) for access.

## Getting the packages

!!! info
    We highly recommend involving your DevOps/Infra group and having them proxy PKWARE's repository through your
    corporate Maven repository, rather than adding PKWARE's repository to each of your software projects. Point them at
    this page.

=== "Gradle"
    Add the following to your `repositories` block

    ```groovy
    maven {
        url 'https://packages.smartcrypt.com/repository/maven-public/'
        credentials {
            username 'Username issued by PKWARE to your company'
            password 'Password issued by PKWARE to your company'
        }
    }
    ```

    Then add the dependency

    ```groovy
    dependencies {
      implementation("com.pkware.smartcrypt:unstructured:2019.7.1")
    }
    ```

=== "Maven"
    Integrate the following into your `pom.xml`

    ```xml
    <repositories>
      <repository>
        <id>pkware</id>
        <name>PKWARE</name>
        <url>https://packages.smartcrypt.com/repository/maven-public/</url>
      </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.pkware.smartcrypt</groupId>
        <artifactId>unstructured</artifactId>
        <version>2019.7.1</version>
      </dependency>
    </dependencies>
    ```

    Integrate the following into your `~/.m2/settings.xml`

    ```xml
    <servers>
        <server>
            <id>pkware</id>
            <username>company_username</username>
            <password>company_password</password>
        </server>
    </servers>
    ```

=== "Manual"
    If not using a maven-package compatible build system, or if unable to reach the PKWARE repository server via your build tool, you can download the JAR files manually.

    1. Navigate to https://packages.smartcrypt.com/#browse/browse:maven-public
    1. Sign in using your company's credentials
    1. Refresh the page. You should now be able to see the artifacts.
    1. Expand that target artifact all the way and download the JAR files needed
    1. Be sure to check the POM files for transitive dependencies and include those in your build as well

## Compatibility

A Java 8 or higher JRE is required. The SDK includes native code, and therefore has system dependencies. PKWARE has confirmed that it works on the following platforms:

- MacOS
- Red Hat Enterprise Linux 6+ 64-bit
- SUSE Linux Enterprise Server 11+
- Ubuntu 16.04+ 64-bit
- Windows 7 / Windows Server 2008 R2 64-bit

The SDK is likely to run on other 64-bit GNU-based linux systems with `libc` version `2.12` or higher.

## Package Integrity
The following checksums can be used if wanting to verify the integrity of the package and transitive dependencies published by PKWARE.

### unstructured-2019.7.1.jar

SHA1: `27b4073b32564e3a471b602e8b02213d10095ac9`

MD5: `51348df5a888ff0cee2ac1f8040d7e03`