---
currentMenu: uuid
---

# UUID

The uuid trait automatically generates uuids for your Eloquent models.

- [Usage](#usage)
    - [Primary Key](#primary-key)
- [Custom Column](#custom-column)

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