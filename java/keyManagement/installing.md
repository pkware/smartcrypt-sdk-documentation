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
      implementation("com.pkware.smartcrypt:key-management:2019.7.1")
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
        <artifactId>key-management</artifactId>
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

### metaclient-2019.7.1.jar

SHA1: `b6d2e79b1a5329ece7fa8c576e77600e5175c2de`

MD5: `36e860184faea2f0c18fe39c6d8db8ed`

### metaclient-jvm-2019.7.1.jar

SHA1: `0d5807beb3c473c5d0efeff20e9a80af13b6757d`

MD5: `584d4d2b9ca62d545c4f7f10f7e617ed`

### key-management-2019.7.1.jar

SHA1: `c0183b0914b749f060e31ed815b4fdb26bc108fa`

MD5: `f88b0c9ebf8947d4ad944faaf841b736`