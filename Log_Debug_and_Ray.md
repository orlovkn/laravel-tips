## Log, Debug and Ray

⬆️ [Go to main menu](README.md#laravel-tips)

* [https://spatie.be/docs/ray/v1/configuration/laravel](Config for Laravel)
* [https://spatie.be/docs/ray/v1/usage/laravel](The whole info for Laravel)

### Showing the table with the times in Ray
```php
ray()->clearAll();
ray()->measure()->label('First call');

$users = User::withCount('networks')->select('id')->get();

ray()->measure->label('Before processing all users');
$data = $service->getData($users);
ray()->measure->label('After processing all users');
```