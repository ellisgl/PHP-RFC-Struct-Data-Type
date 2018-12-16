# PHP RFC: Addition of the 'struct' data type.

## Introduction:
PHP has many data types, but does not offer something that is the equivalent to the C struct data type.

The purpose of this RFC is to propose a data type of 'struct', which would be a strictly typed, immutable data structure that resembles a mix of a class and an array.

    Pros:
    * Provides a data type which would immutable and self-validating by mandatory type hinting.

    Cons:
    * ?

    Possible future considerations:
    * Advanced Type Validators.
        
## Use cases:
Configurations, DTOs, anything that needs a strict schema.

## Proposed usage / syntax:
    struct MyStuct 
    {
        boolean|bool              'prop01';
        integer|int               'prop02';
        double                    'prop03';
        string                    'prop04';
        array                     'prop05';
        object                    'prop06';
        resource                  'prop07';
        callable                  'prop08';
        iterable                  'prop09';
        Acme\MyClass              'prop10';
        Acme\ConfStruct           'prop11';
        array Acme\AnotherStruct  'prop12';
        ?boolean|bool             'prop13';
        ?integer|int              'prop14';
        ?double                   'prop15';
        ?string                   'prop16';
        ?array                    'prop17';
        ?object                   'prop18';
        ?resource                 'prop19';
        ?callable                 'prop20';
        ?iterable                 'prop21';
        ?Acme\MyClass             'prop22';
        ?Acme\ConfStruct          'prop23';
        ?array Acme\AnotherStruct 'prop24';
    }

    - OR? -
    
    struct MyStuct 
    {
        'prop01': boolean|bool;
        'prop02': integer|int;
        'prop03': float;
        'prop04': string;
        'prop05': array;
        'prop06': object;
        'prop07': resource;
        'prop08': callable;
        'prop09': iterable;
        'prop10': Acme\MyClass;
        'prop11': Acme\ConfStruct;
        'prop12': array Acme\AnotherStruct;
        'prop13': ?boolean|bool;
        'prop14': ?integer|int;
        'prop15': ?float;
        'prop16': ?string;
        'prop17': ?array;
        'prop18': ?object;
        'prop19': ?resource;
        'prop20': ?callable;
        'prop21': ?iterable;
        'prop22': ?Acme\MyClass;
        'prop23': ?Acme\ConfStruct;
        'prop24': ?array Acme\AnotherStruct;
    }
    
    struct Acme\ConfStruct
    {
        string 'host';
        ?int 'port';
    }

    struct Acme\AnotherStruct
    {
        string 'firstName';
        string 'lastName';
        ?integer 'age';
    }

    Acme\ConfStruct $a = {
        'host' => '127.0.0.1',
        'port' => 8088
    }

    Acme\MyStruct $myStruct = {
        'prop01' => true,
        'prop02' => 1,
        'prop03' => 1.01,
        'prop04' => 'Hello',
        'prop05' => ['a', 'b', 'c'],
        'prop06' => new stdClass(),
        'prop07' => fopen('xyz.txt', 'wb'),
        'prop08' => function() {
                    return 1 + 1;
                  },
        'prop09' => [
            [1, 2, 3],
            [4, 5, 6],
            [7, 8, 9]
        ],
        'prop10' => new Acme\MyClass,
        'prop11' => $a,
        'prop12' => [
            Acme\AnotherStruct [
                'firstName' => 'Bob',
                'lastName'  => 'Walker'
            ],
            Acme\AnotherStruct [
                'firstName' => 'Little',
                'lastName'  => 'Johnny',
                'age'       => 5
            ],
        ],
        'prop13' => null,
        'prop14' => null,
        'prop15' => null,
        'prop16' => null,
        'prop17' => null,
        'prop18' => null,
        'prop19' => null,
        'prop20' => null,
        'prop21' => null,
        'prop22' => null,
        'prop23' => null,
        'prop24' => null
    };

    $x = new Some\Other\Class($myStruct);
    
    echo $myStruct['prop02'];
    
    - OR? -
    
    echo $myStruct->prop02;

## Warning / Errors / Exceptions:
    Acme\MyStruct $a = {
        'prop01' => 5,
        ...
    }
    // Should: throw new TypeError("'prop01' should be set as a boolean type, integer type was attempted.")
    
    echo $a['xyz']; // or echo $a->xyz (which ever is the preferred syntax after comments).
    // Should: trigger_error("<b>Notice</b>: Undefined property: Acme\MyStruct::xyz");
