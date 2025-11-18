# Variables, Attributes, and Properties in Python

## Summary

### 1. **Variables**
- Local to a function/method scope
- Exist only while the function is executing
- Not accessible outside their scope

### 2. **Attributes**
- Variables that belong to a class or instance
- **Class attributes**: Shared by all instances (e.g., `Dog.species`)
- **Instance attributes**: Unique to each instance (e.g., `self._name`)

### 3. **Properties**
- Attributes controlled by getter/setter methods
- Enable validation, transformation, and controlled access
- Created using the `property()` function

---

## Examples from Our Code

### Class Attributes
```python
class Dog:
    species = "Canis familiaris"  # Class attribute - shared by all dogs
```

```python
# All dogs share the same species
fido = Dog("Fido", "Pug")
buddy = Dog("Buddy", "Beagle")
print(Dog.species)    # => "Canis familiaris"
print(fido.species)   # => "Canis familiaris"
print(buddy.species)  # => "Canis familiaris"
```

### Instance Attributes
```python
class Dog:
    def __init__(self, name="Fido", breed="Mastiff"):
        self._name = name    # Instance attribute (private by convention)
        self._breed = breed  # Instance attribute (private by convention)
```

The underscore prefix (`_name`) indicates this is meant to be private.

### Properties with Validation
```python
class Dog:
    def get_name(self):
        return self._name
    
    def set_name(self, name):
        if isinstance(name, str) and 1 <= len(name) <= 25:
            self._name = name
        else:
            print("Name must be string between 1 and 25 characters.")
    
    # Property - combines getter and setter
    name = property(get_name, set_name)
```

---

## The property() Function

The `property()` function takes up to 4 arguments:
```python
property(fget, fset, fdel, doc)
```

- `fget`: getter method (retrieves value)
- `fset`: setter method (sets/validates value)
- `fdel`: deleter method (optional)
- `doc`: docstring (optional)

### Why Use Properties?

✅ **Validation**: Ensure data meets criteria before storing
```python
dog = Dog("Fido", "Pug")
dog.name = ""  # Prints error message instead of setting invalid value
```

✅ **Data Transformation**: Process data before storing
```python
person = Person("guido van rossum", "Sales")
print(person.name)  # => "Guido Van Rossum" (automatically title-cased)
```

✅ **Controlled Access**: Prevent direct manipulation of internal data
```python
# Users interact with 'name', while '_name' holds the actual value
dog.name = "Buddy"  # Goes through validation
# dog._name = ""    # Bypasses validation (but shouldn't be used!)
```

---

## When to Use Properties vs Regular Attributes

### Use Regular Attributes when:
- No validation is needed
- Data can be any value
- Simple storage is sufficient

### Use Properties when:
- Input validation is required
- Data transformation is needed
- You need to control how attributes are accessed/modified
- You want to maintain data integrity

---

## Complete Working Example

```python
from dog import Dog
from person import Person

# Creating instances
fido = Dog("Fido", "Pug")
guido = Person("guido van rossum", "Sales")

# Accessing class attributes
print(Dog.species)      # => "Canis familiaris"
print(Person.species)   # => "Homo sapiens"

# Accessing properties (calls getters)
print(fido.name)        # => "Fido"
print(guido.name)       # => "Guido Van Rossum" (title-cased!)

# Setting properties (calls setters with validation)
fido.name = "Buddy"     # Valid - sets successfully
fido.name = ""          # Invalid - prints error message
fido.breed = "Poodle"   # Invalid - prints error message

guido.job = "Marketing" # Valid - sets successfully
guido.job = "CEO"       # Invalid - prints error message
```

---

## Key Takeaways

1. **Variables** are local to functions and temporary
2. **Attributes** belong to classes or instances and persist
3. **Properties** are attributes with getter/setter control
4. Use the `property()` function to validate and transform data
5. Prefix "private" attributes with `_` by convention
6. Properties provide clean syntax while maintaining data integrity
