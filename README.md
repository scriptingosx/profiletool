# `profiletool`
A tool that helps manage and manipulate configuration profiles.

```
profiletool verb [options] FILE
```

## Actions

### `unsign` - remove a profile's signature

This action will remove the signature from a given profile. The new, unsigned profile will be written to the current working directory with `_unsigned` appended to the file name. (i.e. `Settings.mobileconfig` will turn into `Settings_unsigned.mobileconfig`)

```
$ profiletool unsign Settings.mobileconfig
```

You usually do _not_ have to explicitly unsign profiles when working with `profiletool`. All actions will work with signed and unsigned profiles.


### `sign` - sign a profile

This action will sign a profile with a given identity. This requires a `--identity` argument. If a profile is already signed, the existing signature will be _replaced_.

```
$ profiletool sign --identity "Developer ID Application: Armin Briegel"
```

You can get a list of identities available to use with

```
$ security find-identity -p codesigning -v
```

When you add the `--identify` argument to any action that generates a profile, the new profile will be signed.

### `check-signature` - verify a profile's signature

This action will verify a signature and print some information.

```
$ profiletool check-signature Settings.mobileconfig
```

### `extract` - creates a new profile with a subset of payloads

This action requires at one or more `--type` arguments. This specifies the `PayloadType` of payload items that will be extracted into a new profile. UUIDs will for the profile and payload items will be regenerated. Identifiers for the profile and payload items will also be regenerated (as with `reset-identifiers`).

The new, extracted profile will be written to the current working directory with `_extract` appended to the file name. (i.e. `Settings.mobileconfig` will turn into `Settings_extract.mobileconfig`)

```
$ profiletool extract --type com.apple.preference.security -t com.apple.security.pkcs1 -t com.apple.security.FDERecoveryKeyEscrow -t com.apple.MCX.FileVault2 --identifier "com.scriptingosx.security" Security.mobileconfig
```

### `reset-uuids` - resets all UUIDs

Replaces the `PayloadUUID` of the profile and all payload items with new UUIDs.

The new profile will be written to the current working directory with `_resetuuids` appended to the file name.

### `reset-identifiers` - resets all identifiers

Replaces all `PayloadIdentifiers` with new, unique values. The top-level identifier for the profile can (and should) be given with the `--identifier` option. If no `--identifier` is given, the tool will use `new.<FILENAME>`.

The identifier for each payload item will be generated by concatenating the `--identifier` and the `PayloadType`. When the Payload type starts with `com.apple.` this will be omitted.

The new profile will be written to the current working directory with `_resetidentifiers` appended to the file name.

### `reset-both` - resets all identifiers and uuids

Replaces both identifiers and uuids with new values.

### `info` - prints a summary of the profile's contents

### `read` - prints a value from a payload

This action requires a `--type` argument to define the payload item and a `--key` item to choose the key. When no `--key` is given, all keys and values will be printed for that payload.

```
$ profiletool read --type com.apple.security.pkcs1 --key PayloadUUID Security_extract.mobileconfig
```

### `write` - sets a value in a payload

This action requires a `--type` and `--key` argument, as well as a `--string` argument to define the value.

```
$ profiletool write --type com.scriptingosx.security.security.FDERecoveryKeyEscrow --key EncryptCertPayloadUUID --string 6a030f05-fe1e-11e8-b00f-8c85901d3852 Security_extract.mobileconfig
```