# First Iterarion of classes
        As this is the first version of the classes, they are currently few and somewhat general. With each iteration, they will be further developed and become more complex in their relationships
**1. `DesignPattern` Class:**

**Attributes:**
- `name` (Type: String): Stores the name of the design pattern.
- `description` (Type: String): Holds a detailed description of the pattern.
- `patternType` (Type: String): Indicates the category to which the pattern belongs.
- `purpose` (Type: String): Describes the purpose of the design pattern.
- `CodeExample` (Type: Code Example): Possibly a reference to an object of the `CodeExample` class related to the pattern.

**Methods:**
- Constructor to initialize the attributes.
- Methods to get and set each attribute.
- Method to display the complete information of the pattern.

**2. `CodeExample` Class:**

**Attributes:**
- `title` (Type: String): A descriptive title for the example.
- `description` (Type: String): A brief description of the example.
- `code` (Type: String): The example code related to the pattern.

**3. `DesignPatternFinder` Class:**

**Attributes:**
- `patternList` (Type: Array of references to `DesignPattern`): Stores a list of objects related to design patterns.

**Methods:**
- `searchByName(name: String)`: List of `DesignPattern`: Allows searching for design patterns by name.
- `searchByCategory(category: String)`: List of `DesignPattern`: Facilitates the search for design patterns by category (e.g., creational, structural, behavioral, etc.).



