## Laravel & PHP Style Guide

⬆️ [Go to main menu](README.md#laravel-tips)

- [General PHP Rules](#general-php-rules)
- [Naming conventions](#naming-conventions)
- [Typed properties](#typed-properties)
- [Docblocks](#docblocks)
- [Constructor property promotion](#constructor-property-promotion)
- [Traits](#traits)
- [Strings](#strings)
- [If statements](#if-statements)
- [Ternary operators](#ternary-operators)
- [Comments](#comments)
- [Configuration](#configuration)
- [Artisan commands](#artisan-commands)
- [Models](#models)
- [Repositories](#repositories)
- [Services](#services)
- [Controllers](#controllers)
- [Routing](#routing)
- [Validation](#validation)
- [Authorization](#authorization)
- [Translations](#translations)
- [Naming classes](#naming-classes)

## General PHP Rules

Code style must follow [PSR-1](http://www.php-fig.org/psr/psr-1/), [PSR-2](http://www.php-fig.org/psr/psr-2/) and [PSR-12](https://www.php-fig.org/psr/psr-12/). Generally speaking, everything string-like that's not public-facing should use camelCase. Detailed examples on these are spread throughout the guide in their relevant sections.
How to configure your IDE: https://blog.jetbrains.com/phpstorm/2019/11/phpstorm-2019-3-release/

### Void return types

If a method return nothing, it should be indicated with `void`.
This makes it more clear to the users of your code what your intention was when writing it.

```php
// good

// in a Laravel model
public function scopeArchived(Builder $query): void
{
    $query->
        ...
}
```

## Naming conventions

Also, follow naming conventions accepted by Laravel community:

What | How | Good | Bad
------------ | ------------- | ------------- | -------------
Controller | singular | ArticleController | ~~ArticlesController~~
Route | plural | articles/1 | ~~article/1~~
Route | kebab-case | hr-candidates | ~~hr_candidates~~
Named route | snake_case with dot notation | users.show_active | ~~users.show-active, show-active-users~~
Model | singular | User | ~~Users~~
hasOne or belongsTo relationship | singular | articleComment | ~~articleComments, article_comment~~
All other relationships | plural | articleComments | ~~articleComment, article_comments~~
Table | plural | article_comments | ~~article_comment, articleComments~~
Pivot table | singular model names in alphabetical order | article_user | ~~user_article, articles_users~~
Table column | snake_case without model name | meta_title | ~~MetaTitle; article_meta_title~~
Model property | snake_case | $model->created_at | ~~$model->createdAt~~
Foreign key | singular model name with _id suffix | article_id | ~~ArticleId, id_article, articles_id~~
Primary key | - | id | ~~custom_id~~
Migration | - | 2017_01_01_000000_create_articles_table | ~~2017_01_01_000000_articles~~
Method | camelCase | getAll | ~~get_all~~
Method in resource controller | [table](https://laravel.com/docs/master/controllers#resource-controllers) | store | ~~saveArticle~~
Method in test class | camelCase | testGuestCannotSeeArticle | ~~test_guest_cannot_see_article~~
Variable | camelCase | $articlesWithAuthor | ~~$articles_with_author~~
Collection | descriptive, plural | $activeUsers = User::active()->get() | ~~$active, $data~~
Object | descriptive, singular | $activeUser = User::active()->first() | ~~$users, $obj~~
Language files index | snake_case | articles_enabled | ~~ArticlesEnabled; articles-enabled~~
View | kebab-case | show-filtered.blade.php | ~~showFiltered.blade.php, show_filtered.blade.php~~
Config | kebab-case | google-calendar.php | ~~googleCalendar.php, google_calendar.php~~
Contract (interface) | adjective or noun | AuthenticationInterface | ~~Authenticatable, IAuthentication~~
Trait | adjective | Notifiable | ~~NotificationTrait~~


## Typed properties

You should type a property whenever possible. Don't use a docblock.

```php
// good
class Foo
{
    public string $bar;
}

// bad
class Foo
{
    /** @var string */
    public $bar;
}
```

## Docblocks

Don't use docblocks for methods that can be fully type hinted (unless you need a description).

Only add a description when it provides more context than the method signature itself. Use full sentences for descriptions, including a period at the end.

```php
// Good
class Url
{
    public static function fromString(string $url): Url
    {
        // ...
    }
}

// Bad: The description is redundant, and the method is fully type-hinted.
class Url
{
    /**
     * Create a url from a string.
     *
     * @param string $url
     *
     * @return \Inno\Url\Url
     */
    public static function fromString(string $url): Url
    {
        // ...
    }
}
```

Always use fully qualified class names in docblocks.

```php
// Good

/**
 * @param string $url
 *
 * @return \Inno\Url\Url
 */

// Bad

/**
 * @param string $foo
 *
 * @return Url
 */
```

Docblocks for class variables are required, as there's currently no other way to typehint these.

```php
// Good

class Foo
{
    /** @var \Inno\Url\Url */
    private $url;
}

// Bad

class Foo
{
    private $url;
}
```

When possible, docblocks should be written on one line.

```php
// Good

/** @var string */
/** @test */

// Bad

/**
 * @test
 */
```

If a variable has multiple types, the most common occurring type should be first.

```php
// Good

/** @var \Inno\Goo\Bar|null */

// Bad

/** @var null|\Inno\Goo\Bar */
```

## Constructor property promotion

Use constructor property promotion if all properties can be promoted. To make it readable, put each one on a line of its own. Use a comma after the last one.

```php
// Good
class MyClass {
    public function __construct(
        protected string $firstArgument,
        protected string $secondArgument,
    ) {}
}
```
```php
// Bad
class MyClass {
    protected string $secondArgument

    public function __construct(protected string $firstArgument, string $secondArgument)
    {
        $this->secondArgument = $secondArgument;
    }
}
```
## Traits

Each applied trait should go on its own line, and the use keyword should be used for each of them. This will result in clean diffs when traits are added or removed.

```php
// Good

class MyClass {
    use TraitA;
    use TraitB;
}
```
```php
// Bad

class MyClass {
    use TraitA, TraitB;
}
```

## Strings

When possible prefer string interpolation above `sprintf` and the `.` operator.

```php
// Good
$greeting = "Hi, I am {$name}.";
```

```php
// Bad
$greeting = 'Hi, I am ' . $name . '.';
```


## Ternary operators

Every portion of a ternary expression should be on its own line unless it's a really short expression.

```php
// Good
$result = $object instanceof Model
    ? $object->name
    : 'A default value';

$name = $isFoo ? 'foo' : 'bar';

// Bad
$result = $object instanceof Model ?
    $object->name :
   'A default value';
```

## If statements

### Bracket position

Always use curly brackets.

```php
// Good
if ($condition) {
   ...
}

// Bad
if ($condition) ...
```

### Happy path

Generally a function should have its unhappy path first and its happy path last. In most cases this will cause the happy path being in an unindented part of the function which makes it more readable.

```php
// Good

if (! $goodCondition) {
  throw new Exception;
}

// do work
```


```php
// Bad

if ($goodCondition) {
 // do work
}

throw new Exception;
```

### Avoid else

In general, `else` should be avoided because it makes code less readable. In most cases it can be refactored using early returns. This will also cause the happy path to go last, which is desirable.

```php
// Good

if (! $conditionBA) {
   // conditionB A failed
   
   return;
}

if (! $conditionB) {
   // conditionB A passed, B failed
   
   return;
}

// condition A and B passed
```

```php
// Bad

if ($conditionA) {
   if ($conditionB) {
      // condition A and B passed
   }
   else {
     // conditionB A passed, B failed
   }
}
else {
   // conditionB A failed
}
```

Another option to refactor an `else` away is using a ternary
```php
// Good

$condition
    ? $this->doSomething()
    : $this->doSomethingElse();
```


```php
// Bad

if ($condition) {
    $this->doSomething();
} 
else {
    $this->doSomethingElse();
}
```

## Comments

Comments should be avoided as much as possible by writing expressive code. If you do need to use a comment, format it like this:

```php
// There should be a space before a single line comment.

/*
 * If you need to explain a lot you can use a comment block. Notice the
 * single * on the first line. Comment blocks don't need to be three
 * lines long or three characters shorter than the previous line.
 */
```

A possible strategy to refactor away a comment is to create a function with name that describes the comment
```php
// Good
$this->calculateLoans();

// Bad

// Start calculation loads
```


## Whitespace

Statements should have to breathe. In general always add blank lines between statements, unless they're a sequence of single-line equivalent operations. This isn't something enforceable, it's a matter of what looks best in its context.

```php
// Good
public function getPage(string $url)
{
    $page = $this->pages()->where('slug', $url)->first();

    if (! $page) {
        return null;
    }

    if ($page['private'] && ! Auth::check()) {
        return null;
    }

    return $page;
}

// Bad: Everything's cramped together.
public function getPage(string  $url)
{
    $page = $this->pages()->where('slug', $url)->first();
    if (! $page) {
        return null;
    }
    if ($page['private'] && ! Auth::check()) {
        return null;
    }
    return $page;
}
```

```php
// Good: A sequence of single-line equivalent operations.
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->increments('id');
        $table->string('name');
        $table->string('email')->unique();
        $table->string('password');
        $table->rememberToken();
        $table->timestamps();
    });
}
```

Don't add any extra empty lines between `{}` brackets.

```php
// Good
if ($foo) {
    $this->foo = $foo;
}

// Bad
if ($foo) {

    $this->foo = $foo;

}
```

## Configuration

Configuration files must use kebab-case.

```
config/
  pdf-generator.php
```

Configuration keys must use snake_case.

```php
// config/pdf-generator.php
return [
    'chrome_path' => env('CHROME_PATH'),
];
```

Avoid using the `env` helper outside of configuration files. Create a configuration value from the `env` variable like above.

## Artisan commands

The names given to artisan commands should all be kebab-cased.

```bash
# Good
php artisan delete-old-records

# Bad
php artisan deleteOldRecords
```

A command should always give some feedback on what the result is. Minimally you should let the `handle` method spit out a comment at the end indicating that all went well.

```php
// in a Command
public function handle()
{
    // do some work

    $this->comment('All ok!');
}
```

If possible use a descriptive success message eg. `Old records deleted`.

## Models
**Do not use Models in controllers, commands, jobs, services. It can be used only in repository.**

## Repositories
All repositories must extend `App\Repositories\Eloquent\EloquentRepository` class and implement their own dedicated interface.

```php
use App\Repositories\Eloquent\EloquentRepository;

class EloquentTaskRepository extends EloquentRepository implements TaskRepositoryContract 
{

}
```

Repository's custom methods must be define in its interface.

Interface:

```php
interface TaskRepositoryContract 
{
    public function filterTasks(array $params);
}
```

Repository

```php
use App\SomeDomain\Contracts\TaskRepositoryContract;

class EloquentTaskRepository extends EloquentRepository implements TaskRepositoryContract
{
    public function filterTasks(array $params)
    {
        // implementation here...
    }
}
```

Use application container for binding abstraction to concrete implementation:

```php
use App\Contracts\TaskRepositoryContract;
use App\Eloquent\EloquentTaskRepository;

$this->app->bind(TaskRepositoryContract::class, EloquentTaskRepository::class);
```

**Do not use repositories in controllers, migrations.**

**Never concrete implementation for dependency injection.**

## Services
The service layer is a set of service classes that deal with application logic.
It can be used everywhere: in controllers, jobs, artisan commands, etc.

In our projects we use repository & service pattern.

```php
// Good
use App\Contracts\TaskRepositoryContract;

class TaskService 
{   
    private TaskRepositoryContract $tasks;
    
    public function __construct(TaskRepositoryContract $tasks)
    {
        $this->tasks = $tasks;
    }
}
```

Bad:

```php
use App\Models\Task;

class TaskService 
{
    private Task $tasks;
    
    public function __construct(Task $tasks)
    {
        $this->tasks = $tasks;
    }
}
```

Sometimes you need to get Model from repository:
```php
app(ModelRepositoryContract)->getEntity();
```

## Controllers

Controllers that control a resource must use the singular resource name.

```php
class PostController
{
    // ...
}
```

Try to keep controllers simple and stick to the default CRUD keywords (`index`, `create`, `store`, `show`, `edit`, `update`, `destroy`). Extract a new controller if you need other actions.

In the following example, we could have `PostController@favorite`, and `PostController@unfavorite`, or we could extract it to a separate `FavoritePostController`.

```php
class PostController
{
    public function create(): JsonResponse
    {
        // ...
    }

    // ...

    public function favorite(int $postId): JsonResponse
    {
        // ...
    }

    public function unfavorite(int $postId): JsonResponse
    {
        // ...
    }
}
```

Here we fall back to default CRUD words, `store` and `destroy`.

```php
class FavoritePostController
{
    public function store(int $postId): JsonResponse
    {
        // ...
    }

    public function destroy(int $postId): JsonResponse
    {
        // ...
    }
}
```

Always use JsonResource when work with CRUD in controller.
Example how to use service pattern in controllers + JsonResource:

```php
// Good
...
class PostController
{
    private PostServiceContract $service;
    
    public function __construct(PostServiceContract $service) 
    {
        $this->service = $service;
    }

    public function create(StoreRequest $request): JsonResponse
    {
        $post = $this->service->create($request->validated());
        
        return new PostResource($post);
    }
}
```

```php
// Bad (using repository)
...
class PostController
{
    private PostRepositoryContract $repository;
    
    public function __construct(PostRepositoryContract $repository) 
    {
        $this->repository = $repository;
    }

    public function create(StoreRequest $request): JsonResponse
    {
        $post = $this->repository->create($request->validated());
        
        return new PostResource($post);
    }
}
```

```php
// The worst (using Model, skip JsonResource)
...
class PostController
{
    private Post $model;
    
    public function __construct(Post $model) 
    {
        $this->model = $model;
    }

    public function create(StoreRequest $request)
    {
        $post = $this->model->create($request->validated());
        
        return response()->json($post);
    }
}
```

## Routing

Public-facing urls must use kebab-case.

```
https://domain.com/open-sources
https://domain.com/jobs/front-end-developer
```

Prefer to use the route tuple notation when possible.
```php
// Good
Route::get('open-sources', [OpenSourceController::class, 'index']);

// Bad
Route::get('open-sources', 'OpenSourceController@index');
```

Route names must use camelCase.
```php
// Good
Route::get('open-sources', [OpenSourceController::class, 'index'])->name('openSource');

// Bad
Route::get('open-sources', [OpenSourceController::class, 'index'])->name('open-source');
```

All routes have an http verb, that's why we like to put the verb first when defining a route. It makes a group of routes very readable. Any other route options should come after it.

```php
// good: all http verbs come first
Route::get('/', [HomeController::class, 'index'])->name('home');
Route::get('open-sources', [OpenSourceController::class, 'index'])->name('openSource');

// bad: http verbs not easily scannable
Route::name('home')->get('/', [HomeController::class, 'index']);
Route::name('openSources')->get('open-sources', [OpenSourceController::class, 'index']);
```

Route parameters should use camelCase.

```php
Route::get('news/{newsItem}', [NewsItemController::class, 'index']);
```

A route url should not start with `/` unless the url would be an empty string.

```php
// good
Route::get('/', [HomeController::class, 'index']);
Route::get('open-sources', [OpenSourceController::class, 'index']);

//bad
Route::get('', [HomeController::class, 'index']);
Route::get('/open-sources', [OpenSourceController::class, 'index']);
```

This is a loose guideline that doesn't need to be enforced.

## Validation

When using multiple rules for one field in a form request, avoid using `|`, always use array notation. Using an array notation will make it easier to apply custom rule classes to a field.

```php
// good
public function rules()
{
    return [
        'email' => ['required', 'email'],
    ];
}

// bad
public function rules()
{
    return [
        'email' => 'required|email',
    ];
}
```

Move validation from controllers to Request classes.

```php
// Good
public function store(PostRequest $request)
{    
    ....
}

class PostRequest extends Request
{
    public function rules()
    {
        return [
            'title' => ['required', 'unique:posts','max:255'],
            'body' => ['required'],
            'publish_at' => ['nullable', 'date'],
        ];
    }
}
```

```php
// Bad
public function store(Request $request)
{
    $request->validate([
        'title' => ['required', 'unique:posts','max:255'],
        'body' => ['required'],
        'publish_at' => ['nullable', 'date']
    ]);

    ...
}
```

All custom validation rules must use snake_case:

```php
Validator::extend('organisation_type', function ($attribute, $value) {
    return OrganisationType::isValid($value);
});
```

## Authorization

Policies must use camelCase.

```php
Gate::define('editPost', function ($user, $post) {
    return $user->id == $post->user_id;
});
```

## Translations

Translations must be rendered with the `__` function. We prefer using this over `@lang` in Blade views because `__` can be used in both Blade views and regular PHP code. Here's an example:

```php
<h2>{{ __('newsletter.form.title') }}</h2>

{!! __('newsletter.form.description') !!}
```

## Naming Classes

Naming things is often seen as one of the harder things in programming. That's why we've established some high level guidelines for naming classes.

### Controllers

Generally controllers are named by the singular form of their corresponding resource and a `Controller` postfix. This is to avoid naming collisions with models that are often equally named.

e.g. `UserController` or `EventDayController`

When writing non-resourceful controllers you might come across invokable controllers that perform a single action. These can be named by the action they perform again suffixed by `Controller`.

e.g. `PerformCleanupController`

### Resources (and transformers)

Both Eloquent resources and Fractal transformers are singular resources postfixed with `Resource` or `Transformer` accordingly. This is to avoid naming collisions with models.

### Jobs

A job's name should describe its action and a `Job` postfix.

E.g. `CreateUserJob` or `PerformDatabaseCleanupJob`

### Events

Events will often be fired before or after the actual event. This should be very clear by the tense used in their name. Use a `Event` postfix

E.g. `ApprovingLoanEvent` before the action is completed and `LoanApprovedEvent` after the action is completed.

### Listeners

Listeners will perform an action based on an incoming event. Their name should reflect that action with a `Listener` postfix. This might seem strange at first but will avoid naming collisions with jobs.

E.g. `SendInvitationMailListener`

### Commands

To avoid naming collisions we'll postfix commands with `Command`, so they are easiliy distinguisable from jobs.

e.g. `PublishScheduledPostsCommand`

### Mailables

Again to avoid naming collisions we'll postfix mailables with `Mail`, as they're often used to convey an event, action or question.

e.g. `AccountActivatedMail` or `NewEventMail`