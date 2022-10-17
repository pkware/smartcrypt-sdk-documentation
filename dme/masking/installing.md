The binaries needed for the SDK are hosted in a Maven repository by PKWARE.

!!! info
    The repository is password protected; contact [PKWARE Sales and Support](mailto:sales@pkware.com) for access.

## Getting the packages

!!! info
We highly recommend involving your DevOps/Infra group and having them proxy PKWARE's repository through your
corporate Maven repository, rather than adding PKWARE's repository to each of your software projects. Point them at
this page.

=== "Gradle Catalog"
    The [Gradle Catalog] for DME contains entries for all the most essential libraries.
    Add the following to your `settings.gradle` or `settings.gradle.kts`

    ```kotlin
    dependencyResolutionManagement {
      repositories {
        maven {
          url = uri("https://packages.smartcrypt.com/repository/maven-public/")
          credentials {
            username = "Username issued by PKWARE to your company"
            password = "Password issued by PKWARE to your company"
          }
        }
      }
    
      versionCatalogs {
        create("pkwareDmeLibs") {
          from("com.pkware.dme:catalog:1.15.0")
        }
      }
    }
    ```

    Add the following to your `dependencies` block in `build.gradle` or `build.gradle.kts`

    ```kotlin
    dependencies {
      implementation(pkwareDmeLibs.masking)
    }
    ```

=== "Gradle"
    Add the following to your `repositories` block in `build.gradle` or `build.gradle.kts`

    ```kotlin
    maven {
      url = uri("https://packages.smartcrypt.com/repository/maven-public/")
      credentials {
        username = "Username issued by PKWARE to your company"
        password = "Password issued by PKWARE to your company"
      }
    }
    ```

    Then add the dependency

    ```kotlin
    dependencies {
      implementation("com.pkware.dme:masking:1.15.0")
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
        <groupId>com.pkware.dme</groupId>
        <artifactId>masking</artifactId>
        <version>1.15.0</version>
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

A Java 8 or higher JRE is required.

[Gradle Catalog]: https://docs.gradle.org/current/userguide/platforms.html#sec:importing-published-catalog