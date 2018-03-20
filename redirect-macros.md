---
currentMenu: redirect-macros
---

# Redirect Macros

Laratools includes several macros on the `RedirectResponse` class for easily sharing notice and success messages.
They are used in the exact same way you would redirect with error messages by using `redirect()->withErrors('Failed to create model...')`.

## Success Messages

To enable success messages you'll need to add `Laratools\View\Middleware\ShareSuccessesFromSession` to your middleware stack in `App\Http\Kernel`.
This makes `$successes` available in your views.

```php
protected $middleware = [
    ...
    \Laratools\View\Middleware\ShareSuccessFromSession::class,
    ...
];
```

To redirect with success messages use the `withSuccesses()` method, you may either pass in a string, array of strings, or `Illuminate\Contracts\Support\MessageProvider` instance.

```php
public function store()
{
    // Store a model

    return redirect()->withSuccesses('Successfully created a model');
}
```

Finally to render the messages in your views check if there are any messages using `$successes->any()` and then render each of the messages using `$successes->all()`.

```php
@if ($successes->any())
    <div class="alert alert-success">
        <ul>
            @foreach($successes->all() as $success)
                <li>{{ $success }}</li>
            @endforeach
        </ul>
    </div>
@endif
```

## Notices

To enable notices you'll need to add `Laratools\View\Middleware\ShareNoticesFromSession` to your middleware stack in `App\Http\Kernel`.
This makes `$notices` available in your views.

```php
protected $middleware = [
    ...
    \Laratools\View\Middleware\ShareNoticesFromSession::class,
    ...
];
```

To redirect with notices using the `withNotices()` method, you may either pass in a string, array of strings, or `Illuminate\Contracts\Support\MessageProvider` instance.

```php
public function info()
{
    // Do something

    return redirect()->withNotices('Your import was successful, but we skipped X items...');
```

Finally to render the messages in your views check if there are any messages using `$notices->any()` and then render each of the messages using `$notices->all()`.

```php
@if ($notices->any())
    <div class="alert alert-info">
        <ul>
            @foreach($notices->all() as $notice)
                <li>{{ $notice }}</li>
            @endforeach
        </ul>
    </div>
@endif
```