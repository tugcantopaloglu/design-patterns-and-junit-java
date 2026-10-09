# Fundamentals of Object-Oriented Programming, Design Patterns, and JUnit Usage

## 1. Project Preparation

In this project, we are asked to solve a problem using design patterns and implement the solution using the Java programming language within the NetBeans IDE. The problem involves storing hierarchical employee data (such as salary, name, position, and manager information) and printing this data either collectively or individually (including salaries, totals, and details).

To address this, we selected the **Composite Pattern**, which fits best with the required hierarchical structure, as shown in the diagram below.

In addition, we are required to traverse this hierarchical structure without using `ArrayList`, which led us to implement our own iterator using the **Iterator Pattern**.

Furthermore, data should be read in a structured way from various sources. While we are initially given a `.txt` file as input, the design should support other formats and databases as well. For this, we used the **Strategy Pattern** to allow extensibility.

## 2. Design Patterns Used and Explanation

### 2.1 Composite Pattern

Following the Composite Pattern, the classes `Memur` (Officer) and `Direktor` (Director) serve as the "leaves" of the structure. These are processed within a `Composite` class and unified under the `Calisan` (Employee) interface. This allows us to treat officers and directors as employees, providing a clear hierarchical structure.

![Composite Pattern UML](https://user-images.githubusercontent.com/53763911/148693211-221250ab-76f1-47da-9fcc-ebb4efcb7978.png)

### 2.2 Iterator Pattern

To iterate over the employee data without using `ArrayList`, we implemented a custom iterator based on the Iterator Pattern. We created our own list and interfaces, which were connected to the `Calisan` class holding the actual data.

![Iterator Pattern UML](https://user-images.githubusercontent.com/53763911/148693245-654e4947-7f25-40a1-8a1e-b49a8d052b6a.png)

### 2.3 Strategy Pattern

To support multiple file types for input (beyond just `.txt` files), we used the Strategy Pattern. We created a class to handle `.txt` files and designed the system to select appropriate handlers based on file extensions through a `Register` mechanism.

![Strategy Pattern UML](https://user-images.githubusercontent.com/53763911/148693261-715deee5-5d79-46ed-b55c-da76e5084ac4.png)

### 2.4 Complete UML Design

![Complete UML](https://user-images.githubusercontent.com/53763911/148693272-d80b3465-f3ab-4392-b280-19c3a153799b.png)

## 3. Explanation of Classes

- **Calisan Interface**: The foundation of the design. It unifies `Memur`, `Direktor`, and `Composite`, and connects with the `List` and `Iterator` interfaces.
- **CalisanIterator Class**: Implements our custom iterator, enabling traversal through our custom list.
- **CalisanList Class**: Stores employee objects and interacts with the iterator.
- **Composite Class**: Processes and represents employees (officers and directors) as part of the hierarchy.
- **Direktor Class**: Represents director-level employees, holds their data and behaviors.
- **Memur Class**: Represents officer-level employees, similar to `Direktor`.
- **Deneme Class**: Contains the `main` method and coordinates file reading, employee instantiation, and processing.
- **FileReader Interface**: Shared interface for all file reading classes (`ExcelReader`, `TXTReader`, `XMLReader`).
- **ExcelReader / TXTReader / XMLReader Classes**: Handle reading data from `.xls`, `.txt`, and `.xml` files respectively.
- **Iterator Interface**: Defines methods needed to build a custom iterator.
- **List Interface**: Defines custom list methods used by `CalisanList`.
- **Registry Class**: Manages file type registrations and resolves which reader to use.

## 4. Unit Testing

To test the project, we implemented a JUnit test in the `Deneme` class where the `main` method resides. The test focused on validating input from the user against the data read from the file. If the user input does not match expected values, the test fails, otherwise it passes.

## Build and run the committed source

This is a NetBeans/Ant Java project with `src/`, `test/`, `build.xml`, and the fixture `girdi.txt`. The classes use the default Java package. A plain JDK is sufficient to compile the application from the repository root:

```sh
javac -encoding UTF-8 -d out src/*.java
java -cp out Deneme
```

Run in an interactive terminal. The application reads `girdi.txt` from the working directory, waits for an initial Enter key, then asks for a complete employee name. The current code creates a new scanner for each prompt, so redirected multi-line standard input can be consumed unexpectedly; use the interactive terminal for the sample flow.

The TXT reader is implemented. `ExcelReader` and `XMLReader` are extension placeholders whose read/write methods throw `UnsupportedOperationException`; they do not parse those file formats yet.

## Run the existing JUnit test

The committed test uses JUnit 4. With JUnit 4.13.2 and Hamcrest Core 1.3 on the classpath:

```sh
javac -encoding UTF-8 -cp "out:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar" -d out test/DenemeTest.java
java -cp "out:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar" org.junit.runner.JUnitCore DenemeTest
```

Place the two dependency JARs under `lib/` before these commands. On Windows, use semicolons instead of colons between classpath entries. NetBeans can also supply the JUnit libraries for its existing Ant test target.

All application sources compiled with JDK 17 and the single committed JUnit test passed. That test verifies user-input retrieval; it does not establish salary aggregation, hierarchy traversal, iterator behavior, or unsupported reader correctness.
