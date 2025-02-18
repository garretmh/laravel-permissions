# Laravel Example

This is a example for Laravel implementation. This requires [Laravel Permissions](https://github.com/spatie/laravel-permission) installed and [configured](https://github.com/spatie/laravel-permission#usage).

## Steps

### Step 1: Add function to PermissionController

The frontend needs to know the permissions assigned, for this purpose one endpoint are created that return this information.

> In this case we will use the magic method `__invoke` to send the function call in the routes

```php
/**
 * Display a listing of permissions from current logged user.
 *
 * @return \Illuminate\Http\JsonResponse
 */
public function __invoke()
{
    return auth()->user()->getAllPermissions()->pluck('name');
}
```

### Step 2: Add function to RoleController

The frontend needs to know the roles assigned, for this purpose one endpoint are created that return this information.

> In this case we will use the magic method `__invoke` to send the function call in the routes

```php
/**
 * Display a listing of roles from current logged user.
 *
 * @return \Illuminate\Http\JsonResponse
 */
public function __invoke()
{
    return auth()->user()->getRoleNames();
}
```

### Step 3: Define routes

These two routes should only be accessed when you are authenticated.

```php
Route::namespace('Auth')->group(function () {
  Route::get('permissions', 'PermissionController');
  Route::get('roles', 'RoleController');
});
```

### Step 4: Use plugin

The plugin creates an instance of Laravel accessible globally, and at the same time it registers all the directives.

```js
import Vue from 'vue';
import LaravelPermissions from 'laravel-permissions';

Vue.use(LaravelPermissions);
```

### Step 5: Set permissions and roles

```js
const permissionsRequest = axios.get('/api/permissions');
const rolesRequest = axios.get('/api/roles');

const { data: permissions } = await permissionsRequest;
const { data: roles } = await rolesRequest;

this.$laravel.setPermissions(permissions);
this.$laravel.setRoles(roles);
```

### Finish!

That is all. Now you can start using the directives.
