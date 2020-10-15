# Change Log

## Version 2019.7.1
*2019-07-01*

- Add `MetaClient#loginImplicitAccount()` for logging into SMDS using a system account.
- Remove `OkHttpNetworkRequestProcessor` and the OkHttp dependency.
- Add `AccountManagement#loginImplicitAccount()` for logging into SMDS using a system account.

## Version 2019.3.1
*2019-03-01*

- Bug fixes

## Version 2019.2.1
*2019-01-31*

- Bug fixes

## Version 2018.12.1
*2019-01-02*

- Format preserving encryption range checking
- Build and test improvements

## Version 2018.11.1
*2018-11-30*

- `SmartkeyFeature` and it's implementation has been removed. Instead, use `com.pkware.smartcrypt.metaclient.Feature`.
Previously, to identify the type of feature, an `instanceof` check was used. Now, compare the value of `Feature#getName()` against constants in the `Feature` class.

## Version 2018.10.1
*2018-11-01*

- Bug fixes

## Version 2018.9.3
*2018-10-22*

- Fix segfault that occurred on some Oracle Linux systems

## Version 2018.9.2
*2018-10-12*

- Reduce system requirements in native code on Linux

## Version 2018.9.1
*2018-09-24*

- Bug fixes

## Version 2018.8.1
*2018-08-23*

- Javadocs now link against external documentation

## Version 2018.7.2
*2018-08-17*

- Fix issue when using `MetaClientPasswordListener` with Smartkeys that have multiple Asset Keys

## Version 2018.7.1
*2018-07-25*

- New Unstructured Data API
- Minor changes to the MetaClient API
- Javadocs and source as part of Maven artifact

## Version 2018.6.2
*2018-07-02*

- Fix POM

## Version 2018.6.1
*2018-06-29*

- Initial release