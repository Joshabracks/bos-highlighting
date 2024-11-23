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
```js
// the type name is notated with an @ symbol
@type_name
    // number field is notated with an octothorpe
    #cost
    // string field is notated by a tilde
    ~name
    // boolean field is notated by an exclamaition point
    !is_true
    // arrays are notated by open and closed square brackets  
    [#]number_array
    [~]string_array
    [!]boolean_array
    []any_array
    // set length arrays are preceeded with the length
    2[#]set_length_number_array
    // required fields are preceeded with an asterisk
    * #id
    // default values can be set with an equal sign
    #parent_id = 0
    // defined item types are notated by the type name within enclosed angle brackets
    <defined_type>defined_type
```

```
// example type definition
@item
    * ~name
    * ~description
    * [#]value
    * #weight
    [<discipline>]disciplines
    [<merit>]merits
    [<flaw>]flaws
```

### Enumerables (enum)
Enumerables are noted with the enumerable type followed by a line separated list

```js
<discipline>
    TACTICS     // 0
	COMBAT      // 1
	ESPIONAGE   // 2
	TRICKERY    // 3
```
By default, enumerables are assigned numerical values
however, they can be given other values by properly notating the enumberable

```js
<discipline>
    ~TACTICS    tactics
    ~COMBAT     combat
    ~ESPIONAGE  espionage
    ~TRICKERY   trickery
```

Enumerables can also be declared in tuples, triplets and so on...

```js
<merit flaw>
    BRAVE       COWARDLY    // 0
    CLEVER      CLUELESS    // 1
    STRONG      WEAK        // 2
    MAGE        CURSED      // 3
    SORCERER    HAUNTED     // 4

```

### Object
Objects are notated with a percentage sign and the type of object
The keys and fields are separated with a colon


```yaml
%item
    name: gold coin
    description: small circular currency minted from gold
    value: [100]
    weight: 1
    // recipes do not need to be defined within the type definition and are notated with a colon, followed by the workshop type
    :mint(1 gold billet)1
```

Several objects can be listed through enumerators using the following shorthand.  (using the discipline enum)

```yaml
%item <discipline>
    name: <~discipline>  // gets string name of enum
    id: <discipline>     // gets default of enum
    lowercase_name: <~~discipline>
    capitalized_name: <~~~discipline>
```

In the above case, an enum that looks like the following:
```yaml
<discipline>
    TACTICS
    ESPIONAGE
    LEADERSHIP
```
is the same as writing a list like so..

```yaml
%item
    name: TACTICS
    id: 0
    lowercase_name: tactics
    capitalized_name: Tactics
%item
    name: ESPIONAGE
    id: 1
    lowercase_name: espionage
    capitalized_name: Espionage
%item
    name: LEADERSHIP
    id: 2
    lowercase_name: leadership
    capitalized_name: Leadership
```