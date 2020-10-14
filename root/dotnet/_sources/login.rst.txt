Implicit Login
==================

The Smartcrypt SDK supports implicit login, a form of login in which managed users are logged in
not by providing a username and password, but by the system user who owns the process using the
SDK.

Advantages of Implicit Login
-------------------------------
Several features make implicit login the recommended authentication mechanism.

For back-office applications, implicit login provides the convenience of not needing to hard code,
manually enter or place the credentials into a configuration file. This improves security and
makes it easier to deploy applications.

For end-user applications, companies typically want the end user to login with domain
credentials when running the applications. In this case, implicit login affords the ability to skip
a login prompt, enabling a seamless user experience. This is particularly desirable when modifying
existing business applications to incorporate Smartcrypt data protection.

How it works
----------------

On Windows, Integrated Windows Authentication is used. The identity of the Active Directory Domain
User who launches the process using the Smartcrypt SDK is used.

On MacOS and Linux, the Kerberos system is used to identify and authenticate the user.
Domain-joined MacOS installations are automatically configured. On Linux, the
`kinit <https://web.mit.edu/kerberos/krb5-latest/doc/user/user_commands/kinit.html>`_ program is
used. When properly configured, Linux machines will automatically run ``kinit`` when users perform
a password-based login.

.. only :: csharp

    The identity of the Active Directory Domain User who launches the process using the Smartcrypt
    SDK is used. Setting the `KRB5_CONFIG` environment variable to the path to a custom `krb5.conf`
    file allows you to configure the underlying Kerberos setup.

.. only:: java

    On MacOS and Linux, the JVM must be configured to correctly integrate with the Kerberos system.
    Which Active Directory Domain User identity is used depends on the configuration.

.. only:: java

    Configuring the JVM
    ----------------------------

    The simplest way to get started is to set the ``java.security.krb5.realm`` and
      ``java.security.krb5.kdc`` system properties:

    .. code-block:: java

        public static void setupKerberos() {
            // Provide the Kerberos realm that your user is in
            System.setProperty("java.security.krb5.realm", "EXAMPLE.COM");
            // Provide the name of a Kerberos ticketing server
            System.setProperty("java.security.krb5.kdc", "dc01.example.com");
        }

    For a production rollout, you should set the ``java.security.krb5.conf`` system property to
    point at the ``krb5.conf`` on the host system. Consult your Kerberos or Windows Domain
    Administrator for more information on the details of your Kerberos environment.

    Diagnosing Problems
    ----------------------

    To start, ensure that you can successfully acquire a Kerberos TGT with the
    `kinit <https://web.mit.edu/kerberos/krb5-latest/doc/user/user_commands/kinit.html>`_ utility.
    When this is successful, proceed.

    The Smartcrypt SDK has a built-in diagnostic utility. To activate it

    1. set the logging level to ``TRACE``
    2. run the ``java`` command with the flag
       ``-Djava.security.debug=net,gssloginconfig,configfile,configparser,logincontext``
       to enable detailed JVM logging
    3. call `MetaClient#loginImplicitAccount() <javadoc/com/pkware/smartcrypt/metaclient/MetaClient.html#loginImplicitAccount()>`_.
       This will cause the utility to write diagnostics to the configured
       `Logger <javadoc/com/pkware/smartcrypt/metaclient/Logger.html>`_.

    The diagnostic utility will inspect your current configuration for common problems. The
    additional logging may be helpful in determining how the JVM is interpreting the current
    Kerberos setup.
