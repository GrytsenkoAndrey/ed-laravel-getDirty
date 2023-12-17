# ed-laravel-getDirty
Power of getDirty()

## Understanding the getDirty() Method:

The getDirty() method, part of the Laravel Eloquent Model class, provides an efficient way to retrieve attributes that have been changed or "dirtied" since the last synchronization with the database. This method returns an array containing the names of attributes that have been modified, along with their new values.
Tracking Attribute Changes

Let’s begin by creating a simple example to demonstrate how getDirty() works:

```
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    // Define the Eloquent model
}

$user = User::find(1); // Retrieve a user from the database

// Update the user's name and email
$user->name = 'John Doe';
$user->email = 'john.doe@example.com';

// Check for dirty attributes
$dirtyAttributes = $user->getDirty();

// $dirtyAttributes will contain ['name' => 'John Doe', 'email' => 'john.doe@example.com']
```

In this example, we fetched a user from the database, modified the name and email attributes, and then used getDirty() to identify the changes. The resulting $dirtyAttributes array contains the names of the attributes that were altered along with their new values.

## Auditing Attribute Changes

One common use case for getDirty() is auditing. Developers often need to keep track of changes made to certain attributes of a model for auditing or versioning purposes. Here's how you can implement a basic attribute change log using Eloquent and getDirty():

```
use Illuminate\Database\Eloquent\Model;

class Article extends Model
{
    // Define the Eloquent model
}

$article = Article::find(1); // Retrieve an article from the database

// Update the article's content
$article->content = 'Updated content';

// Check for dirty attributes
$dirtyAttributes = $article->getDirty();

if (!empty($dirtyAttributes)) {
    // Log the changes to an audit table
    foreach ($dirtyAttributes as $attribute => $newValue) {
        $oldValue = $article->getOriginal($attribute); // Get the original value
        // Log the change with attribute name, old value, and new value
        AuditLog::create([
            'model' => 'Article',
            'model_id' => $article->id,
            'attribute' => $attribute,
            'old_value' => $oldValue,
            'new_value' => $newValue,
        ]);
    }
}
```

In this example, we detect changes in the content attribute of an article, log them to an audit table, and store the old and new values. By using getDirty() in conjunction with getOriginal(), you can efficiently track changes to specific attributes.
