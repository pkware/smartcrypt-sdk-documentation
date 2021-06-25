Getting Started
===============

Prerequisites
-------------

* A deployed & :doc:`configured <manager>` Smartcrypt Enterprise Manager
* A good understanding of Smartcrypt, especially :doc:`Smartkeys <faq>` and :doc:`Smartkey strategies <smartkeyStrategies>`

Installation
-------------

The binaries needed for the SDK are hosted by PKWARE. The SDK is split into components so you can pick and choose the components your application needs.

NuGet
^^^^^

Create a ``NuGet.Config`` file at the root of your project, next to your ``.sln`` file, with the following content

.. code-block:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <packageSources>
            <add key="PKWARE" value="https://packages.smartcrypt.com/repository/nuget-public/" />
        </packageSources>
        <packageSourceCredentials>
            <PKWARE>
                <add key="Username" value="your_username" />
                <add key="ClearTextPassword" value="your_password" />
            </PKWARE>
        </packageSourceCredentials>
    </configuration>

Manual
^^^^^^

If not using NuGet, or if unable to reach the PKWARE NuGet server, you can download the required files manually.

#. Navigate to https://packages.smartcrypt.com/#browse/browse:nuget-public
#. Sign in using your credentials
#. Refresh the page. You should now be able to see the artifacts.
#. Expand that target artifact all the way and download the ``nupkg`` files needed
#. Change the downloaded files from ``.nupkg`` to ``.zip``
#. Extract the DLL from the ``lib`` folder in the zip, and check the ``.nuspec`` file for any transitive dependencies needed.

.. note:: The repository is password protected; use the credentials issued by `PKWARE Sales and Support <mailto:sales@pkware.com>`_ for access.

Add the following NuGet packages

* ``PKWARE.Smartcrypt.KeyManagement`` - Required for creating, syncing, and modifying Smartkeys
* ``PKWARE.Smartcrypt.Structured`` - Required for working with structured data encryption
* ``PKWARE.Smartcrypt.Unstructured`` - Required for working with unstructured data encryption

PKArchive.NET Toolkit
^^^^^^^^^^^^^^^^^^^^^

The Unstructured Data component requires the PKArchive.NET Toolkit. This is available as a Windows installer, and can be downloaded from https://packages.smartcrypt.com/repository/binaries-public/sdk/PKWARE_Toolkit_Win_11.00.0066.exe. After installing, the ``PKArchive.NET`` DLLs can be found in ``Documents\Visual Studio 2010\Projects\PKWARE\PAAPI11\Lib``.

Framework compatibility
^^^^^^^^^^^^^^^^^^^^^^^

PKWARE provides support for .NET Framework 4.6.1 and up on Windows, and for .NET Core 2.1 on all platforms, which is the LTS release from Microsoft. The SDK targets .NET Standard 2.0, so other frameworks may work despite being out of support. A complete compatibility matrix is available `from Microsoft <https://docs.microsoft.com/en-us/dotnet/standard/net-standard>`_.

Visual Studio
^^^^^^^^^^^^^
If using Visual Studio, it is recommended you use 2017. If using 2015, you must:

* Install `Nuget VSIX 3.6 or higher <https://www.nuget.org/downloads>`_
* Install `.NET Standard build support for Visual Studio 2015 <https://aka.ms/netstandard-build-support-netfx>`_
* Install `.NET Core 2.1 SDK or higher <https://www.microsoft.com/net/download/>`_
* Modify your ``.csproj`` file by adding ``<ImplicitlyExpandDesignTimeFacades>false</ImplicitlyExpandDesignTimeFacades>`` to the first ``PropertyGroup``. This `StackOverflow answer <https://stackoverflow.com/a/44648397/2502247>`_ gives a good example.
