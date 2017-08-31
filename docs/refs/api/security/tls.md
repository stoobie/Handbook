#### Arm Mbed TLS

Arm Mbed TLS provides a comprehensive SSL/TLS solution and makes it easy for developers to include cryptographic and SSL/TLS capabilities in their software and embedded products. As an SSL library, it provides an intuitive API, readable source code and a minimal and highly configurable code footprint.

<span class="notes">_**Note:** Mbed TLS needs a secure source of random numbers; make sure that your target board has one and that it is fully ported to Arm Mbed OS. You can read more about this in our [porting guide](https://docs.mbed.com/docs/mbed-os-handbook/en/latest/advanced/porting_guide/)._</span>

##### Differences between the standalone and Arm Mbed OS editions

Mbed TLS has a standalone edition (available from the [Mbed TLS website](https://tls.mbed.org/download)) for devices that are not running Mbed OS. However, this guide focuses on the integration of Mbed TLS into Mbed OS. Although the two editions share a codebase, there are a number of differences, mainly in configuration and the platform integration. You should keep those differences in mind when reading articles in our [knowledge base](https://tls.mbed.org/kb), as currently all articles reference the standalone edition.

The key differences are:

- To reduce its footprint, the Mbed OS edition enables a smaller set of features by default in the build configuration file `config.h`. Although the default configuration of the standalone edition puts more emphasis on maintaining interoperability with a wide variety of platforms, the Mbed OS edition only enables the most modern ciphers and most recent version of TLS and DTLS.
- The following components of Mbed TLS are disabled in the Mbed OS edition: `net.c` and `timing.c`. This is because equivalent platform code is available in Mbed OS.

##### Configuring Mbed TLS features

Mbed TLS simplifies enabling and disabling features to meet the needs of a particular project, through compilation options. The default configuration:

- Enables all modern and widely used features, to meet the needs of new projects.
- Disables all features that are older or less common, to minimize the code footprint.

The list of compilation flags is available in the fully documented configuration file, [config.h](https://github.com/ARMmbed/mbedtls/blob/development/include/mbedtls/config.h).

For example, in an application called `myapp`, if you want to enable the EC J-PAKE key exchange and disable the CBC cipher mode, you can create a file named  `mbedtls-config-changes.h` in the `myapp` directory containing the following lines:

```
#define MBEDTLS_ECJPAKE_C
#define MBEDTLS_KEY_EXCHANGE_ECJPAKE_ENABLED

#undef MBEDTLS_CIPHER_MODE_CBC
```

Then create a file named `mbed_app.json` at the root of your application with the following contents:

```
{
    "macros": ["MBEDTLS_USER_CONFIG_FILE=\"mbedtls-config-changes.h\""]
}
```

<span class="notes">_**Note:** You need to provide the exact name that you use in the `#include` directive, including the `<>` or quotes around the name_.

##### Getting Mbed TLS from GitHub

Like most components of Mbed OS, Mbed TLS is an open source project, and its source can be found on GitHub: [ARMmbed/mbedtls](https://github.com/ARMmbed/mbedtls). Unlike most other Mbed OS components, however, you cannot just add the repository with `mbed add`. This is because Mbed TLS also exists as an independent software library for other platforms, so its repository includes files that are not relevant for Mbed OS.

If you want to use Mbed TLS from the GitHub repository, and potentially to edit it:

1. Fork the [Mbed OS](https://github.com/ARMmbed/mbed-os) repository.

2. Edit or set the value of `MBED_TLS_RELEASE` to the desired tag branch or revision in the [Makefile](https://github.com/ARMmbed/mbed-os/blob/master/features/mbedtls/importer/Makefile) used for importing Mbed TLS.

3. Open a shell, go to the Mbed TLS importer [directory](https://github.com/ARMmbed/mbed-os/tree/master/features/mbedtls/importer) and run the command:
    ``
    make update && make
    ``

4. Commit, and push the changes before you add your fork with `mbed add` to your project.

##### Sample programs

This release includes the following examples:

1. [TLS client](https://github.com/ARMmbed/mbed-os-example-tls/tree/master/tls-client): Downloads a file from an HTTPS server (developer.mbed.org) and looks for a specific string in that file.

1. [Benchmark](https://github.com/ARMmbed/mbed-os-example-tls/tree/master/benchmark): Measures the time taken to perform basic cryptographic functions used in the library.

1. [Hashing](https://github.com/ARMmbed/mbed-os-example-tls/tree/master/hashing): Demonstrates the various APIs for computing hashes of data (also known as message digests) with SHA-256.

1. [Authenticated encryption](https://github.com/ARMmbed/mbed-os-example-tls/tree/master/authcrypt): Demonstrates using the Cipher API for encrypting and authenticating data with AES-CCM.

Each of them comes with complete usage instructions as a readme file in the repository.

##### Other resources

The [Mbed TLS website](https://tls.mbed.org) contains many other useful resources for developers, such as [developer documentation](https://tls.mbed.org/dev-corner), [knowledge base articles](https://tls.mbed.org/kb) and a [support forum](https://tls.mbed.org/discussions).

##### Contributing

We gratefully accept bug reports and contributions from the community.

To be able to accept contributions of code, we require the author of the work or whoever has the authority to do so to sign a Contributor’s License Agreement (CLA).

The easiest way to do this is to create an Mbed account and [approve the agreement here with a click through](https://developer.mbed.org/contributor_agreement/) if this is a personal contribution.

If this is a contribution from a company, or if you do not want to create an Mbed account, you can [find an alternative agreement here](https://www.mbed.com/en/about-mbed/contributor-license-agreements/), which you can sign and return to Arm.

We require a CLA for all contributions, whether large or small.

If you would like to contribute, please:

- Check for [open issues](https://github.com/ARMmbed/mbedtls/issues) or start a [discussion](https://tls.mbed.org/discussions) around a feature idea or a bug.

- Fork the [Mbed TLS repository](https://github.com/ARMmbed/mbedtls) on GitHub to start making your changes. As a general rule, you should use the "development" branch as a basis.

- Write a test that shows that the bug was fixed or that the feature works as expected.

- Send a pull request and nag us until it gets merged and published. We will include your name in the ChangeLog.