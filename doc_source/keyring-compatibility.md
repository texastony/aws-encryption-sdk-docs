# Keyring compatibility<a name="keyring-compatibility"></a>

Although the different language implementations of the AWS Encryption SDK have some architectural differences, they are fully compatible, subject to language constraints\. You can encrypt your data using one language implementation and decrypt it with any other language implementation\. However, you must use the same or corresponding wrapping keys to encrypt and decrypt your data keys\. For information about language constraints, see the topic about each language implementation, such as [Compatibility of the AWS Encryption SDK for JavaScript](javascript-compatibility.md) in the AWS Encryption SDK for JavaScript topic\.

## Varying requirements for encryption keyrings<a name="encrypt-keyring-requirements"></a>

In AWS Encryption SDK language implementations other than the AWS Encryption SDK for C, all wrapping keys in an encryption keyring \(or multi\-keyring\) or master key provider must be able to encrypt the data key\. If any wrapping key fails to encrypt, the encrypt method fails\. As a result, the caller must have the [required permissions](use-kms-keyring.md#kms-keyring-permissions) for all keys in the keyring\. If you use a discovery keyring to encrypt data, alone or in a multi\-keyring, the encrypt operation fails\. 

The exception is the AWS Encryption SDK for C, where the encrypt operation ignores a standard discovery keyring, but fails if you specify a multi\-Region discovery keyring, alone or in a multi\-keyring\.

## Compatible Keyrings and Master Key Providers<a name="keyring-compat-table"></a>

The following table shows which master keys and master key providers are compatible with the keyrings that the AWS Encryption SDK supplies\. Any minor incompatibility due to language constraints is explained in the topic about the language implementation\.


| Keyring: | Master Key Provider: | 
| --- | --- | 
| [AWS KMS keyring](use-kms-keyring.md) |  [KMSMasterKey \(Java\)](https://aws.github.io/aws-encryption-sdk-java/com/amazonaws/encryptionsdk/kms/KmsMasterKey.html) [KMSMasterKeyProvider \(Java\)](https://aws.github.io/aws-encryption-sdk-java/com/amazonaws/encryptionsdk/kms/KmsMasterKeyProvider.html) [KMSMasterKey \(Python\)](https://aws-encryption-sdk-python.readthedocs.io/en/latest/generated/aws_encryption_sdk.key_providers.kms.html) [KMSMasterKeyProvider \(Python\)](https://aws-encryption-sdk-python.readthedocs.io/en/latest/generated/aws_encryption_sdk.key_providers.kms.html#aws_encryption_sdk.key_providers.kms.KMSMasterKeyProvider)  The AWS Encryption SDK for Python and AWS Encryption SDK for Java don't include a master key or master key provider that is equivalent to the [AWS KMS regional discovery keyring](use-kms-keyring.md#kms-keyring-regional)\.    | 
| [Raw AES keyring](use-raw-aes-keyring.md) | When they are used with symmetric encryption keys:[JceMasterKey](https://aws.github.io/aws-encryption-sdk-java/com/amazonaws/encryptionsdk/jce/JceMasterKey.html) \(Java\)[RawMasterKey](https://aws-encryption-sdk-python.readthedocs.io/en/latest/generated/aws_encryption_sdk.key_providers.raw.html#aws_encryption_sdk.key_providers.raw.RawMasterKey) \(Python\) | 
| [Raw RSA keyring](use-raw-rsa-keyring.md) | When they are used with asymmetric encryption keys:[JceMasterKey](https://aws.github.io/aws-encryption-sdk-java/com/amazonaws/encryptionsdk/jce/JceMasterKey.html) \(Java\)[RawMasterKey](https://aws-encryption-sdk-python.readthedocs.io/en/latest/generated/aws_encryption_sdk.key_providers.raw.html#aws_encryption_sdk.key_providers.raw.RawMasterKey) \(Python\) The AWS Encryption SDK does not support asymmetric AWS KMS keys\.  | 