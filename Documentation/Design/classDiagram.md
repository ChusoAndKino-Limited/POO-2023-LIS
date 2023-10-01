```mermaid

classDiagram
  class DesignPattern {
    -name: String
    -description: String
    -patternType: String
    -purpose: String
    -CodeExample: CodeExample

    +DesignPattern(name: String, description: String, patternType: String, purpose: String, CodeExample: CodeExample)
    +getName(): String
    +setName(name: String): void
    +getDescription(): String
    +setDescription(description: String): void
    +getPatternType(): String
    +setPatternType(patternType: String): void
    +getPurpose(): String
    +setPurpose(purpose: String): void
    +getCodeExample(): CodeExample
    +setCodeExample(CodeExample: CodeExample): void
    +displayPatternInfo(): void
  }

  class CodeExample {
    -title: String
    -description: String
    -code: String

    +CodeExample(title: String, description: String, code: String)
    +getTitle(): String
    +setTitle(title: String): void
    +getDescription(): String
    +setDescription(description: String): void
    +getCode(): String
    +setCode(code: String): void
  }

  class DesignPatternFinder {
    -patternList: Array<DesignPattern>

    +DesignPatternFinder(patternList: Array<DesignPattern>)
    +searchByName(name: String): Array<DesignPattern>
    +searchByCategory(category: String): Array<DesignPattern>
  }

  DesignPattern -- CodeExample : CodeExample
  DesignPatternFinder --> DesignPattern : patternList

