---
currentMenu: uuid
---

# UUID

The uuid trait automatically generates uuids for your Eloquent models.

- [Usage](#usage)
    - [Primary Key](#primary-key)
- [Custom Column](#custom-column)
- [Scope](#scope)

## Usage

Usage is as simple as adding a uuid column to your database and using the trait on your model.

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

Now, whenever you save a new model, a uuid will be generated.

#### Primary Key

Using a uuid as the primary key is as simple as setting auto-increments on your model to false.

```php
use Laratools\Eloquent\Uuid;

class User extends Model
{
    use Uuid;
    
    public $incrementing = false;

    protected $primaryKey = 'uuid';
}
```

## Custom Column

By default the trait will assume a column name of `uuid`, however you can customise this on a per-model basis by defining the `UUID_COLUMN` constant on your model.

```php
class User extends Model
{
    use Uuid;
    
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