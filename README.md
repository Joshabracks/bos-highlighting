# BOS (Batching Object System)
BOS is a type specific data interchange format written specifically for the "Guild Master" project.
Author: Joshua James Almanza Bracks

## Purpose
Increase iteration speed of asset creation by enforcing good data practices and allowing for grouped and filtered data design.
Incorporate simple functional shorthand within strings and (eventually) numerical values.

## Roadmap
- Comments
- Inline Mathmatical operations

## Syntax

### Type Definition
```bash
@type_name
    int cost
    string name
    boolean is_true
    [float]float_array
    [string]string_array
    [boolean]boolean_array
    []any_array
    2[int]set_length_integer_array
    :int id
    int parent_id
    brackets
    <defined_type> defined_type
    [<defined_type>] defined_type
```

```bash
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

```bash
enum discipline
    TACTICS  
	COMBAT   
	ESPIONAGE
	TRICKERY 
```
By default, enumerables are assigned numerical values
however, they can be given other values by properly notating the enumberable

Enumerables can also be declared with meta_data.  In a single line, 0th entry of the enum is the acutal enum, whereas the 1st, 2nd, ect can be used as a secondary enum or a descriptor.  Because of this, the 0th must be unique, but others do not

```bash
enum    merit       flaw
        BRAVE       COWARDLY
        CLEVER      CLUELESS
        STRONG      WEAK    
        MAGE        CURSED  
        SORCERER    HAUNTED 

enum    animal  class   diet
        DOG     MAMMAL  OMNIVORE
        CAT     MAMMAL  CARNIVORE
        PEACOCK BIRD    OMNIVORE
        SHARK   FISH    CARNIVORE
```

### Object
Object types are declared without indent at the first line of the object.


```bash
item
    name: gold coin
    description: small circular currency minted from gold
    value: [100]
    weight: 1
    :mint(1 gold billet)1
```

Several objects can be listed through enumerators using the following shorthand.  (using the discipline enum)

```bash
item discipline
    name: <~discipline>  
    id: <discipline>     
    lowercase_name: <~~discipline>
    capitalized_name: <~~~discipline>
```

In the above case, an enum that looks like the following:
```bash
discipline
    TACTICS
    ESPIONAGE
    LEADERSHIP
```
is the same as writing a list like so..

```bash
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

Objects that make use of meta_data enums, can also reference all parts of the meta_data
```bash
leash animal class
    name: <~~~animal> leash
    alternate_name: <~~~class> leash
```

using the animal enum listed just above this section, you'd get the following list

```bash
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