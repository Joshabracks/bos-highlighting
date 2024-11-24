# BOS (Branching Object System)
BOS is a type specific data interchange format written specifically for the "Guild Master" project.
Author: Joshua James Almanza Bracks

## Purpose
Create game objects with branching relationships to other objects using flags, triggers and recipe groups

## Syntax

### Comments

```js
// single line comment

/* 
multi
line
comment
*/
```

### Type Definition
```c#
// the type name is notated with an @ symbol
@type_name
    // float, int, string and boolean primatives are supported
    int cost
    string name
    boolean is_true
    // arrays are notated by open and closed square brackets  
    [float]float_array
    [string]string_array
    [boolean]boolean_array
    []any_array
    2[int]set_length_integer_array
    // required fields are preceeded with an asterisk
    * int id
    // default values can be set with an equal sign
    int parent_id = 0
    // defined item types are notated by the type name within enclosed angle brackets
    <defined_type> defined_type
    [<defined_type>] defined_type
```

```c
// example type definition
@item
    *string name
    *string description
    *[int] value
    *float weight
    [<discipline>] disciplines
    [<merit>] merits
    [<flaw>] flaws
```

### Enumerables (enum)
Enumerables are noted with the enum keyword and type followed by a line separated list

```
enum discipline
    TACTICS     // 0
	COMBAT      // 1
	ESPIONAGE   // 2
	TRICKERY    // 3
```
By default, enumerables are assigned numerical values
however, they can be given other values by properly notating the enumberable

Enumerables can also be declared in tuples.  In a single line, 0th entry of the enum is the acutal enum, whereas the 1st can be used as a secondary enum or a descriptor.  Because of this, the 0th must be unique, but the 1st does not.

```
enum    merit       flaw
        BRAVE       COWARDLY    // 0
        CLEVER      CLUELESS    // 1
        STRONG      WEAK        // 2
        MAGE        CURSED      // 3
        SORCERER    HAUNTED     // 4

enum    animal  class
        DOG     MAMMAL  // 0, 0
        CAT     MAMMAL  // 1, 0
        PEACOCK BIRD    // 2, 1
        SHARK   FISH    // 3, 2
```

### Object
Object types are declared without indent at the first line of the object.


```yaml
item
    name: gold coin
    description: small circular currency minted from gold
    value: [100]
    weight: 1
    // recipes do not need to be defined within the type definition and are notated with a colon, followed by the workshop type
    :mint(1 gold billet)1
```

Several objects can be listed through enumerators using the following shorthand.  (using the discipline enum)

```yaml
item discipline
    name: <~discipline>  // gets string name of enum
    id: <discipline>     // gets default of enum
    lowercase_name: <~~discipline>
    capitalized_name: <~~~discipline>
```

In the above case, an enum that looks like the following:
```yaml
discipline
    TACTICS
    ESPIONAGE
    LEADERSHIP
```
is the same as writing a list like so..

```yaml
item
    name: TACTICS
    id: 0
    lowercase_name: tactics
    capitalized_name: Tactics
item
    name: ESPIONAGE
    id: 1
    lowercase_name: espionage
    capitalized_name: Espionage
item
    name: LEADERSHIP
    id: 2
    lowercase_name: leadership
    capitalized_name: Leadership
```

Objects that make use of tuple enums, can also reference both parts of the tuple
```
leash animal class
    name: <~~~animal> leash
    alternate_name: <~~~class> leash
```

using the animal enum listed just above this section, you'd get the following list

```
leash
    name: Dog leash
    alternate_name: Mammal leash

leash
    name: Cat leash
    alternate_name: Mammal leash

leash
    name: Peacock leash
    alternate_name: bird leash

leash
    name: Shark leash
    alternate_name: Fish leash
```