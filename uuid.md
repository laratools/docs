---
currentMenu: uuid
---

# UUID

The uuid trait automatically generates uuids for your Eloquent models.

- [Usage](#usage)
    - [Primary Key](#primary-key)
- [Custom Column](#custom-column)
- [Scope](#scope)
- [String Based UUIDs](#string-based-uuids)

## Usage

Usage is as simple as adding a uuid column to your database and using the trait on your model.

```php
Schema::table('users', function (Blueprint $table)
{
    $table->binaryUuid('uuid');
});
```  

```php
use Laratools\Eloquent\BinaryUuid;

class User extends Model
{
    use BinaryUuid;
}
```

Now, whenever you save a new model, a uuid will be generated. When using the `BinaryUuid` trait, two attributes are provided, `$model->uuid` (The binary form) and `$model->uuid_text` (The string form).
You should use `$model->uuid` whenever you are locating a model from the database, and use `$model->uuid_text` whenever you are display the uuid to your users (e.g API payloads).

#### Primary Key

Using a uuid as the primary key is as simple as setting auto-increments on your model to false.

```php
use Laratools\Eloquent\BinaryUuid;

class User extends Model
{
    use BinaryUuid;
    
    public $incrementing = false;

    protected $primaryKey = 'uuid';
}
```

## Custom Column

By default the trait will assume a column name of `uuid`, however you can customise this on a per-model basis by defining the `UUID_COLUMN` constant on your model.

```php
use Laratools\Eloquent\BinaryUuid;

class User extends Model
{
    use BinaryUuid;
    
    const UUID_COLUMN = 'id';
}
```

## Scope

The trait provides a convenient scope for finding models based on the uuid column.
`Model::uuid('638fa033-2bcf-48dd-896e-6e2b138f7138')->first()`, or you may also supply an array of uuids
`Model::uuid([
    '5d95a9c2-bc2c-4a2a-8e97-1eabf472319e',
    '5785e0eb-155f-4c25-9e1a-c123a29a0cd8',
    '041f681f-9c9b-4ca4-b623-fb9928490e8c',
    '93b1e345-b262-4bc7-ad41-034f755a00c3',
])->get()`

## String Based UUIDs

> ⚠️ String based UUIDs are consistently slower on any size dataset. Use this at your own caution ⚠️

Usage is identical to the `BinaryUuid` trait with two differences.

1. You should use the `uuid` type provided by Laravel. This creates a char column type with a length of 36.
2. There is no `uuid_text` attribute, only `uuid`

```php
Schema::table('users', function (Blueprint $table)
{
    $table->uuid('uuid');
});
```

```php
use Laratools\Eloquent\Uuid;

class User extends Model
{
    use Uuid;
}
```