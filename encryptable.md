---
currentMenu: encryptable
---

# Encryptable

The encryptable trait can automatically encrypt and decrypt attributes on your models

- [Usage](#usage)
  - [Unsafe Decryption](#unsafe-decryption)

## Usage

```php
class BankAccount extends Model
{
    protected $casts = [
        'number'    => 'encrypt',
        'sort_code' => 'encrypt',
    ];
}
```

This model will now automatically encrypt and decrypt the `number` and `sort_code` attributes when saving/retrieving from the database.

#### Unsafe Decryption

By default if a `DecryptException` is thrown when attempting to decrypt the attribute, the encryptable trait will catch the exception
and safely return the unaltered contents of the attribute, this is to prevent any unexpected exceptions in your application.
If you wish to change this behaviour and have the `DecryptException` thrown then you may set the `protected $safeDecrypt = false` property on your model. 

```php
class BankAccount extends Model
{
    protected $casts = [
        'number'    => 'encrypt',
        'sort_code' => 'encrypt',
    ];
    
    protected $safeDecrypt = false;
}
```

Now when the `number` or `sort_code` attributes fail to decrypt (e.g because of an invalid hmac) then the `DecryptException` will be thrown and you can handle
it within your application however you need to.