---
title: What's New in PHP 8.6
link: https://www.php.net/releases/8.6/en.php
source: hnrss-org
published: 2026-11-19T09:27:15Z
updated: 2026-11-19T09:27:15Z
first_seen: 2026-11-19T17:16:58.860737333Z
authors:
- netol
content: extracted
html: 2026-11-19-what-s-new-in-php-8-6.html
remote_preview:
  url: https://www.php.net/images/php8/php_8_6_released.png
---

## Faster, Safer, More Powerful.

**PHP 8.6 is a major update of the PHP language**, with new features including **Partial Function Application**, a new **Duration** class, a **Polling API**, and a new **clamp()** function

## Key Features in PHP 8.6

**More expressive code**, **stronger types**, and **scalable I/O**.

[Partial Function Application](https://www.php.net/releases/8.6/en.php#partial-function-application)

Allows some of a function's arguments to be provided in advance, returning a Closure that accepts the rest.

[New `\Time\Duration` Class](https://www.php.net/releases/8.6/en.php#duration-class)

The Duration class represents elapsed time as measured by a stopwatch.

[Polling API](https://www.php.net/releases/8.6/en.php#polling-api)

Scalable I/O multiplexing using epoll, kqueue, and other native mechanisms, available to event loop libraries like ReactPHP and AMPHP.

[New `clamp()` Function](https://www.php.net/releases/8.6/en.php#clamp-function)

Ensures that a given value (numeric or otherwise) falls within a specified range.

[New `SortDirection` Enum](https://www.php.net/releases/8.6/en.php#sort-direction-enum)

A standard, type-safe way to express sort order that can be shared across libraries.

[Readonly Property Defaults](https://www.php.net/releases/8.6/en.php#readonly-property-defaults)

Allows readonly properties to declare default values, so fixed values no longer have to be assigned in the constructor.

## Partial Function Application

Allows some of a function's arguments to be provided in advance, using `?` and `...` placeholders for the rest. The result is a Closure, and the function is only called once the remaining arguments are passed to it.

```
$versions = ['8.4', '8.5', '8.6', '8.7', '9.0'];

$supportsPfa = array_filter(
    $versions,
    fn ($version) => version_compare($version, '8.6', '>='),
);
```

```
$versions = ['8.4', '8.5', '8.6', '8.7', '9.0'];

$supportsPfa = array_filter(
    $versions,
    version_compare(?, '8.6', '>='),
);
```

## New `\Time\Duration` Class

The Duration class represents elapsed time as measured by a stopwatch. Its representation is independent of any time and timezone and can represent a range of more than 250 years with nanosecond precision. This makes it well-suited to represent timeouts or benchmark timings in an expressive way.

```
/**
 * All delays are in microseconds.
 */
function backoff(
    int $baseDelay,
    int $step,
    int $attempt,
): void {
    $delay = $baseDelay + ($step * (2 ** $attempt));

    // Wait at most 30 seconds.
    usleep(min($delay, 30_000_000));
}

// 1 second base delay, plus 100 milliseconds
// doubling with each attempt.
backoff(1_000_000, 100_000, $attempt);
```

```
use Time\Duration;

function backoff(
    Duration $baseDelay,
    Duration $step,
    int $attempt,
): void {
    $delay = $baseDelay->add($step->multiplyBy(2 ** $attempt));

    // Durations are comparable, so min() works directly.
    $delay = min($delay, Duration::fromSeconds(30));

    time_nanosleep($delay->seconds, $delay->nanoseconds);
}

backoff(
    Duration::fromSeconds(1),
    Duration::fromMilliseconds(100),
    $attempt,
);

// Preconfigure the delays using Partial Function Application.
$waitBeforeRetry = backoff(
    Duration::fromSeconds(1),
    Duration::fromMilliseconds(100),
    attempt: ?,
);
$waitBeforeRetry($attempt);
```

## Polling API

The new `Io\Poll` API efficiently monitors many streams at once using the best mechanism available on each platform (epoll, kqueue, event ports, or WSAPoll), without the scalability limits of `stream_select()`. It is designed as a common foundation that event loop libraries such as ReactPHP and AMPHP can build on.

```
$server = stream_socket_server('tcp://0.0.0.0:8080');
$clients = [];

while (true) {
    $read = [$server, ...$clients];
    $write = $except = null;

    // Based on select(), which typically can't handle
    // more than 1024 connections
    stream_select($read, $write, $except, null);

    foreach ($read as $stream) {
        if ($stream === $server) {
            $clients[] = stream_socket_accept($server);
            continue;
        }

        $data = fread($stream, 8192);

        if ($data === '' || $data === false) {
            $key = array_search($stream, $clients, true);
            unset($clients[$key]);
            fclose($stream);
        } else {
            fwrite($stream, $data);
        }
    }
}
```

```
use Io\Poll\{Context, Event};

$server = stream_socket_server('tcp://0.0.0.0:8080');

// Automatically uses epoll, kqueue, event ports, or WSAPoll
$poll = new Context();
$poll->add(new StreamPollHandle($server), [Event::Read]);

while (true) {
    foreach ($poll->wait() as $watcher) {
        $stream = $watcher->getHandle()->getStream();

        if ($stream === $server) {
            $client = stream_socket_accept($server);
            $poll->add(
                new StreamPollHandle($client),
                [Event::Read],
            );
            continue;
        }

        $data = fread($stream, 8192);

        if ($data === '' || $data === false) {
            $watcher->remove();
            fclose($stream);
        } else {
            fwrite($stream, $data);
        }
    }
}
```

## Stream Error Handling

Stream operations can now be configured to throw a `StreamException` or to fail silently instead of emitting warnings. Errors are reported as structured `StreamError` objects with a `StreamErrorCode` enum case, the wrapper name, and the affected path or URL, and can also be retrieved later with `stream_last_errors()` or passed to a custom error handler.

```
$config = @file_get_contents('/etc/app/config.json');

if ($config === false) {
    // Only an unstructured message string is available
    $error = error_get_last();
    log_error($error['message']);
}
```

```
$context = stream_context_create([
    'stream' => [
        'error_mode' => StreamErrorMode::Exception,
    ],
]);

try {
    $config = file_get_contents(
        '/etc/app/config.json',
        context: $context,
    );
} catch (StreamException $e) {
    foreach ($e->getErrors() as $error) {
        // $error->code is a StreamErrorCode enum case
        log_error(
            $error->code->name,
            $error->message,
            $error->wrapperName,
        );
    }
}
```

## New `clamp()` Function

Ensures that a given value (numeric or otherwise) falls within a specified range. Otherwise, the nearest boundary value is returned.

```
var_dump(max(0, min(-10, 100)));
// int(0)

var_dump(max(0, min(50, 100)));
// int(50)

var_dump(max(0, min(110, 100)));
// int(100)
```

```
var_dump(clamp(-10, 0, 100));
// int(0)

var_dump(clamp(50, 0, 100));
// int(50)

var_dump(clamp(110, 0, 100));
// int(100)

// Any comparable value works with clamp(). This
// includes the new Duration class.
var_dump(clamp(
    \Time\Duration::fromSeconds(30),
    min: \Time\Duration::fromMinutes(1),
    max: \Time\Duration::fromMinutes(10),
)->seconds);
// int(60)
```

## New `SortDirection` Enum

The `SortDirection` enum provides a shared, type-safe way to express sort order with its `Ascending` and `Descending` cases. Previously, every library had to define its own way of representing sort direction, resulting in a mix of incompatible integers, strings, booleans, and custom enums. Since `SortDirection` is built into PHP, libraries can accept it in their public APIs without needing adapter code.

Learn more about this feature in [The PHP Foundation’s blog](https://thephp.foundation/blog/2026/11/03/stdlib-investment/).

```
class QueryBuilder
{
    private array $orderBy = [];

    public function orderBy(
        string $column,
        int $direction = SORT_ASC,
    ): static {
        $this->orderBy[] = $column . ' ' . match ($direction) {
            SORT_ASC => 'ASC',
            SORT_DESC => 'DESC',
        };

        return $this;
    }
}

$qb->orderBy('name', SORT_DESC);
$qb->orderBy('name', SORT_STRING);
// Accepted by the signature, then fails inside the method:
// UnhandledMatchError: Unhandled match case 2
```

```
class QueryBuilder
{
    private array $orderBy = [];

    public function orderBy(
        string $column,
        SortDirection $direction = SortDirection::Ascending,
    ): static {
        $this->orderBy[] = $column . ' ' . match ($direction) {
            SortDirection::Ascending => 'ASC',
            SortDirection::Descending => 'DESC',
        };

        return $this;
    }
}

$qb->orderBy('name', SortDirection::Descending);
$qb->orderBy('name', SORT_STRING);
// TypeError: QueryBuilder::orderBy(): Argument #2 ($direction)
// must be of type SortDirection, int given
```

Function and method parameters can now have their own DocComments, which are available via the new `ReflectionParameter::getDocComment()` method. This allows each parameter to be documented right next to its declaration, without repeating its name and type in a `@param` tag.

```
class DB
{
    /**
     * Returns up to $limit items matching the $query.
     *
     * @param string $query Terms to search
     * @param int $limit Maximum number of results
     */
    public function search(string $query, int $limit = 10): array
    {
        return [];
    }
}

// Parameter descriptions can only be extracted
// by parsing the function's DocComment.
```

```
class DB
{
    /**
     * Returns up to $limit items matching the $query.
     */
    public function search(
        /** Terms to search */
        string $query,

        /** Maximum number of results */
        int $limit = 10,
    ): array {
        return [];
    }
}

var_dump(
    new ReflectionParameter([DB::class, 'search'], 'query')
        ->getDocComment(),
);
// string(22) "/** Terms to search */"
```

## New `ReflectionProperty::isReadable()` and `ReflectionProperty::isWritable()` Methods

With readonly properties, asymmetric visibility, and property hooks, checking whether a property is public is no longer enough to know whether it can be read or written. The new methods answer this directly for a given scope, and optionally for a specific object, taking into account visibility, initialization state, hooks, and magic methods.

```
class NewsEntry
{
    public string $title = 'PHP 8.6 released!';

    public private(set) string $slug = 'php-86-released';
}

$entry = new NewsEntry();
foreach (['title', 'slug'] as $item) {
    $property = new ReflectionProperty(NewsEntry::class, $item);
    printf(
        "%s: isReadable=%s isWritable=%s\n",
        $item,
        $property->isReadable(null, $entry) ? 'yes' : 'no',
        $property->isWritable(null, $entry) ? 'yes' : 'no',
    );
}
// title: isReadable=yes isWritable=yes
// slug: isReadable=yes isWritable=no
```

## Readonly Property Defaults

Instance readonly properties, including those in readonly classes, can now declare a default value. This makes it simple to implement get-only interface properties with fixed values, which previously required assigning them in a constructor. The default counts as the property's initialization, so the constructor cannot overwrite it.

```
interface Rule
{
    public string $parser { get; }
}

final readonly class SlugRule implements Rule
{
    public string $parser;

    public function __construct()
    {
        $this->parser = SlugParser::class;
    }
}
```

```
interface Rule
{
    public string $parser { get; }
}

final readonly class SlugRule implements Rule
{
    public string $parser = SlugParser::class;
}
```

## Additional Features and Improvements

- The `#[\Override]` attribute can now be applied to class constants and enum cases.
- Enums can now implement the `__debugInfo()` magic method.
- Properties of objects stored in constants can now be modified directly, e.g. `Config::DEFAULTS->timeout = 30`.
- New `Uri\Rfc3986\UriBuilder` and `Uri\WhatWg\UrlBuilder` classes build URIs from their components, along with new methods for detecting URI and host types and a `Uri\WhatWg\url_percent_encode()` function.
- The Sodium extension adds post-quantum key encapsulation (X-Wing and ML-KEM-768), IP address encryption, and the SHAKE and TurboSHAKE extendable-output functions.
- TLS streams now support session resumption, external pre-shared keys, and TLS 1.3 early data (0-RTT).
- New `mysqli::quote_string()` method escapes a string and wraps it in quotes, matching `PDO::quote()`.
- `pack()` and `unpack()` now accept the `<` and `>` endianness modifiers on integer and float format codes.
- JSON error messages now include the location of the error in the input.
- The new `error_include_args` INI setting consistently includes function arguments in error messages.
- New `grapheme_strrev()` function reverses strings by grapheme cluster, and the new `IntlNumberRangeFormatter` class formats ranges of numbers.
- New `ZipArchive::openString()` and `ZipArchive::closeString()` methods work with in-memory archives.
- Performance improvements include compiling `array_map()` with first-class callable or partial application callbacks into loops, faster `printf()` calls, caching of stateless closures, and faster JSON encoding.

## Deprecations and Backward Compatibility Breaks

- Using `return` inside a `finally` block is now deprecated, as it silently discards exceptions and overrides values returned from the `try` or `catch` block.
- Returning a value from `__construct()` or `__destruct()` is now deprecated, as is making them generators. Declaring a nullable return type for `__debugInfo()` is also deprecated.
- Using `let` or `is` as the name of a class, function, or constant is now deprecated. Using `_` as a constant name, `namespace` as a class constant name, or `readonly` as a function name is also deprecated.
- The mbstring regular expression functions such as `mb_ereg()` and `mb_split()` have been deprecated, as the underlying Oniguruma library is no longer maintained. PCRE functions with the `u` modifier can be used instead.
- The `is_double()`, `is_long()`, `is_integer()`, and `doubleval()` aliases have been deprecated. Use `is_float()`, `is_int()`, and `floatval()` instead.
- `spl_object_hash()` has been deprecated in favor of `spl_object_id()`, and `spl_classes()` in favor of `ReflectionExtension::getClassNames()`.
- Passing an object where an array is expected to `array_walk()`, `array_walk_recursive()`, `http_build_query()`, `mb_convert_variables()`, and compression functions and filters is now deprecated. Use `get_object_vars()` to convert the object first.
- `strcoll()` and the `SORT_LOCALE_STRING` sort flag have been deprecated. Use the `Collator` class from the intl extension instead.
- Passing a third argument to `define()` is now deprecated, as is calling `is_a()` or `is_subclass_of()` with a class name string when `$allow_string` is `false`.
- Using more than 16 filters in a `php://filter` URL without setting the `filter.max_filter_count` context option is now deprecated.
- Sessions are now more secure by default: `session.use_strict_mode` and `session.cookie_httponly` are enabled, and `session.cookie_samesite` is set to `Lax`.
- Custom session handlers that do not implement both `create_sid()` and `validateId()` are now deprecated.
- The `metaphone()`, `mysqli_get_charset()`, and `mysqli_stmt_init()` functions are now deprecated, along with the CSV methods of `SplFileObject` and the sorting, flag, and serialization methods of `ArrayIterator`.
- `trim()`, `ltrim()`, and `rtrim()` now also strip form feed (`\f`) characters by default.
- `fclose()`, `file_put_contents()`, and `copy()` now return `false` when flushing or closing the stream fails, instead of silently ignoring the failure.
- Many functions now throw a `ValueError` for out-of-range values, invalid flags, or strings containing NUL bytes, instead of silently truncating or ignoring them.
