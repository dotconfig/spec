# dotconfig specification

`dotconfig` is a text format for the serialization of hand-written application configurations.

## Note

* `dotconfig` is NOT an interchange format.

# Grammar

Basically, the grammar is same as JSON except following features:

* allowed comments
* allowed trailing comma
* allowed here documents
* allowed naked keys of map
* allowed hex, binary, octal number literals

```
TODO: BNF goes here
```

# Examples

```
{
    email: "foo@example.com"
    password: "s3cret"
}
```

```
{
    mysql_default_config: { // default configuration
        port: 3306,
        user: "cocoa",
        password: "s3cret",
        database: "users",
        charset:  "utf8mb4",
    },
    mysql: {
        MYSQL_USER_MASTER: { host: "master.users.mysql.local", },
        MYSQL_USER_SLAVE: [ // shards
            { host: /*comment*/ "0.user.products.mysql.local", port: 33060, },
            { host: "1.user.products.mysql.local", port: 33061, },
        ],
        /* /* currently disabled */
        MYSQL_PRODUCT_MASTER: { host: "master.products.mysql.local", },
        MYSQL_PRODUCT_SLAVE: [
            { host: "0.slave.products.mysql.local", port: 33060, database: "products" },
            { host: "1.slave.products.mysql.local", port: 33061, database: "products" },
        ]
        */
    },
    redis: {
        REDIS_PRODUCT_SHARD: [
            { host: "0.shard.products.redis.local", port: 3600, },
            { host: "1.shard.products.redis.local", port: 3601, },
        ]
    }

}
```

See also https://github.com/dotconfig/testcase

# Types

* Number
    * Integer
        * `1`, `100`, `-100`, `0`, `42`
    * Float
        * `1.0`, `100.0`, `-100.0`, `0.0`, `1.2e-3`
    * Hex
        * `0xdeadbeef`
    * Binary
        * `0b0000`, `0b1111`, `0b1001`
    * Octal
        `0o377`
* Boolean
    * `true`, `false`
* Null
    * `null`
* String
    * Quoted
        * `"Foo"`, `"bar"`, `"いろはにほへと"`, `"\u3044\u308d\u306f\u306b\u307b\u3078\u3068"`, `"Hello World\n"`
    * Heredoc
        *
```
        {
            string: <<EOS
Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
EOS,
            string2: <<-EOS
            Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
            Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
            Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
            Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
            EOS,
        }
```
* Array
    * `[]`, `[ "foo", 1, 0xdeadbeef ]`
* Map
    * `{ a: "A", b: "B", c: "C" }`
* Comment
    * Inline
        * `"\u3044\u308d\u306f\u306b\u307b\u3078\u3068" // いろはにほへと`
    * Block
        *
```
        /*
            いろはにほへと
        */
        "\u3044\u308d\u306f\u306b\u307b\u3078\u3068",
```

# Language bindings

* [Perl 5](https://github.com/dotconfig/dotconfig-perl)

# Contribution

## Language bindings

1. Open github issue with a language name that you want to write
2. We will check your public repos and invite you to dotconfig organization
3. Boom

## Documentation improvements

* Creating new pull request to this repo is sufficient.

