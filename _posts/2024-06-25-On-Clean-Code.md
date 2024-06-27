---
title: On Clean Code
date: 2024-06-20 12:00:00 -0
categories: [Software Development, Principles]
tags: []
---

The more experience I gain in AI model development and deployment, the more I realize how important it is to write high-quality code. This realization led me to explore books on software craftsmanship, and one book that consistently stood out was "Clean Code" by Robert C. Martin. This book goes deeply into the principles and practices of writing clean, efficient, and maintainable code, emphasizing the need for simplicity, readability, and discipline in coding.

"Clean Code" captures the essence of software craftsmanship by addressing the core principles of good coding practices. It highlights the importance of writing code that not only works but is also easy to understand and maintain. Ultimately, "Clean Code" teaches that striving for perfection in the smallest details leads to the development of cleaner, more efficient, and more maintainable code, benefiting both developers and users alike.

In this post, I aim to summarize and reflect on the key insights and lessons from "Clean Code" from my perspective. By doing so, I hope to enhance my skills and continue my journey toward becoming a better AI professional.

## 1. The Importance of Small Actions in Coding

The book begins by emphasizing the significance of small actions and details in coding and software development. One of its core messages is that "honesty in small things is not a small thing." This principle highlights the importance of integrity and meticulousness in every aspect of coding, no matter how minor it may seem. By embedding this principle into their daily practice, developers can cultivate a mindset of excellence and continuous improvement, ultimately leading to the creation of cleaner, more efficient, and more maintainable code.

Small things matter. The book advocates for the idea that paying attention to the small details can profoundly impact the overall quality of the code. Just as small acts of kindness in life can create a ripple effect, in coding, small improvements and attention to detail can lead to more robust and maintainable software. One compelling example given is the Boy Scout Rule: "Leave the campground cleaner than you found it." This concept encourages developers to always strive to improve the codebase, no matter how small the change.

This also introduces the idea that designing code is an ongoing process and that developing it is not a one-time job but involves continuous inspection and improvement. By consistently cleaning up code, refactoring, and eliminating technical debt, developers contribute to a healthier and more sustainable project.

Attention to detail is highlighted as a crucial foundation of professionalism. This idea suggests that the true mark of a professional lies in their dedication to mastering the small, everyday tasks. It is through consistent practice and a focus on the finer points that professionals gain proficiency and expertise.

The book also warns that even the smallest sloppy part can completely undermine the larger whole. This serves as a reminder that neglecting minor aspects can degrade the overall quality and integrity of the software, leading to potential issues and a loss of credibility. A single poorly written function or overlooked bug can create cascading problems that detract from the overall system’s performance and reliability.

The book introduces the concept of Total Productive Maintenance (5S principles), which are essential for maintaining high standards in coding and software development. These principles are:

- **Seiri (Sort)**: Organize and name everything properly. By sorting through your code and eliminating unnecessary elements, you create a cleaner and more efficient workspace.
- **Seiton (Systematize)**: Ensure everything is in its proper place. A tidy codebase where every component is systematically placed makes it easier to understand and navigate.
- **Seiso (Shine)**: Keep your workspace free of clutter. Regular cleaning and maintenance of your code help prevent the buildup of technical debt and clutter.
- **Seiketsu (Standardize)**: Agree as a group on processes and standards. By establishing and adhering to coding standards, teams can ensure consistency and quality across the codebase.
- **Shitsuke (Self-Discipline)**: Practice self-discipline in maintaining these standards. Consistently applying these principles requires discipline and commitment from every team member.

## 2. Key Principles for Quality and Continuous Improvement

The book emphasizes several key principles that are essential for committing to quality and continuous improvement, ultimately leading to better software and more satisfied users. By adhering to these principles, developers can create code that is not only functional and efficient but also a joy to work with. These principles include:

- **Value Substance Over Appearance:** True quality in code comes from its functionality, readability, and maintainability, rather than its superficial appearance. Substance is what makes the code robust and reliable.
- **Excellence Over Competence:** Strive for excellence rather than just competence. Quality is the result of a million selfless acts of care. This means paying attention to every detail and consistently aiming for the best possible outcome. Simple acts, when done with care, lead to excellence. However, simple does not mean simplistic or easy; it requires thoughtful effort and dedication.
- **Promote Usability:** Ensure that code is user-friendly and easy to work with. Usable code is easier to read, understand, and maintain, benefiting everyone who interacts with it.
- **Code Is Not a Static Endpoint:** Code is always evolving and requires continuous refinement and improvement. Viewing code as a dynamic entity that can always be improved helps maintain high standards over time.
- **Be Honest and Humble:** Approach coding with honesty and humility. Recognize the current state of the code and be open about its strengths and weaknesses. This honesty should extend to our colleagues, fostering a culture of transparency and continuous improvement.

## 3. Code Quality

### 3.1. Consequences of Neglecting Code Quality
The book highlights several critical points about the consequences of neglecting code quality and the importance of maintaining high standards. One major consequence is that even the most innovative application will suffer if it is built on poor-quality code. Over time, issues will take longer to resolve, crashes will become more frequent, and the application will ultimately become unusable. Bad code creates obstacles that make it difficult to accomplish tasks efficiently, a situation referred to as "wading" through messy code, which slows down development and hampers productivity.

Having a functional but messy codebase is not a viable solution. A disorganized codebase can lead to more problems than it solves, causing long-term issues that outweigh short-term gains. Failing to follow the rule of leaving the campground cleaner than you found it leads to decreased productivity over time. As technical debt accumulates, the codebase becomes harder to work with, slowing down progress and increasing frustration.

### 3.2. Causes of Neglecting Code Quality

The book explores several potential reasons for declining code quality. Frequent changes in requirements can disrupt development and lead to shortcuts that compromise code quality. Pressures to meet deadlines can tempt developers to cut corners, resulting in subpar code. Poor management decisions and a lack of understanding of the importance of clean code can contribute to declining quality. Demanding customers may push for faster delivery at the expense of quality.

However, the real reason lies with ourselves. As developers, we have the responsibility to uphold standards and resist the urge to rush. Pushing a product to market without ensuring its quality can lead to long-term issues that are costly and time-consuming to fix. Investing time upfront to write clean, maintainable code pays off in the long run. Properly designed and implemented code reduces future headaches and improves overall efficiency. Procrastinating on cleaning up code usually means it will never get done. It's essential to address issues as they arise to prevent technical debt from accumulating.

### 3.3. The Importance of Defending Code Quality

The book stresses the importance of evaluating people's performance based on the quality of their work, not the quantity. High-quality code leads to long-term success and sustainability, while poor-quality code results in ongoing issues and inefficiencies. As a developer, it is your job to defend the integrity of the code. This means maintaining high standards and resisting pressures to compromise on quality.

It is unprofessional to bend to the will of those who do not understand the risks of creating messy code. Defending the code’s quality is a critical part of your role. Making a mess to meet a deadline will slow you down instantly; the immediate gains are quickly overshadowed by the long-term problems it creates. Conversely, keeping the code as clean as possible is the only way to maintain speed and efficiency over time. Clean code reduces technical debt and makes future development faster and easier.

Understanding how to differentiate good code from bad code is essential. However, recognizing quality does not necessarily mean you know how to achieve it. Continuous learning and practice are necessary to master the art of writing clean code. By focusing on the quality of your work and continuously improving your skills, you can contribute to the development of robust, efficient, and maintainable software that stands the test of time.

## 4. Clean Code Characteristics

Clean code embodies several key characteristics that make it robust, efficient, and maintainable:

- **Elegant:** Clean code is aesthetically pleasing and well-structured, reflecting thoughtful design and attention to detail.
- **Efficient:** It performs its functions with minimal resource usage, ensuring optimal performance.
- **Straightforward:** It is easy to understand and free from hidden bugs, making it accessible to any developer who reads it.
- **Low Dependencies:** It has minimal dependencies, which simplifies maintenance and reduces potential conflicts.
- **Simple:** Clean code avoids unnecessary complexity, making it easier to manage and less prone to errors.
- **Crisp Abstractions:** It uses clear and precise abstractions, enhancing readability and reducing cognitive load.
- **Maintainable:** It can be improved and modified by anyone, not just the original author, ensuring long-term sustainability.
- **Explicit Dependencies:** All dependencies are clearly defined and easy to manage, promoting transparency and ease of integration.
- **Clear and Minimal API:** The interfaces are straightforward and minimal, making them easy to use and understand.
- **Care:** It looks like it was written by someone who cares about their work, reflecting a high level of professionalism and pride.
- **Testing:** It includes tests to ensure reliability and correctness, safeguarding against future issues.
- **No Duplication:** It avoids duplicate code, which can lead to inconsistencies and maintenance challenges, ensuring a clean and cohesive codebase.

### 4.1. Naming Conventions

Proper naming conventions are crucial for creating clear, understandable, and maintainable code. The book emphasizes several key principles for effective naming:

- **Avoid Misinformation and Non-information:** Names should accurately describe what they represent, avoiding any confusion or lack of information. Misleading or vague names can cause significant misunderstandings and errors, hindering the development process.

- **Searchable Names:** Names should be easy to search for within the codebase. This helps developers quickly locate and understand variables, functions, and classes, improving efficiency and reducing errors.

- **Clarity is King:** Clear and descriptive names are paramount. They should immediately convey the purpose and usage of the variable, function, or class without requiring additional explanation. Clarity in naming reduces the need for comments and documentation, making the code more self-explanatory.

- **Methods Should Use Verbs or Describe Actions:** Method names should clearly indicate what the method does. Common prefixes include:
  - `delete_xxx`: Clearly indicates the method deletes something.
  - `get_xxx`: Indicates the method retrieves or gets a value or object.
  - `from_xxx`: Suggests the method creates or derives something from another object or value.

- **Avoid Mental Mapping:** Names should be intuitive and self-explanatory, eliminating the need for developers to remember what each variable does. This reduces cognitive load and makes the code easier to understand and maintain.

- **Add Context:** Provide additional context to names to avoid ambiguity. For example, use `address_street` instead of just `street`, and `address_state` instead of just `state`. Adding context helps to differentiate similar elements and provides a clearer understanding of their role within the code.

### 4.2. Functions

Effective function design is essential for creating clean and maintainable code. The book outlines several principles for writing good functions:

- **Refactor Regularly:** Regularly refactor functions to improve readability and maintainability. This involves breaking down complex functions into smaller, more manageable pieces. Regular refactoring ensures that the code remains clean and easy to understand over time.

- **Max Length:** Aim to keep functions no longer than 20 lines. Ideally, functions should be between 5 to 10 lines. Short functions are easier to understand, test, and debug, making them more maintainable in the long run.

- **Use Function Calls Inside Control Structures:** Anything inside an `if` statement or `for` loop should be a function call. This keeps the code small and promotes good naming practices. Using function calls within control structures also enhances readability by abstracting complex logic into named functions.

- **Avoid Nested Structures:** Nested structures can make code difficult to read and understand. Strive to keep indentation levels no greater than one or two. Limiting nesting helps maintain clarity and simplicity in the code, making it easier to follow the flow of logic.

- **Single Responsibility Principle:** Functions should do one thing and do it well. Each function should perform tasks that are one level below its name, maintaining just one level of abstraction per function. Adhering to the single responsibility principle ensures that functions remain focused and easy to manage.

- **Avoid Duplication:** Adhering to the "Don't Repeat Yourself" (DRY) principle is essential. Avoid duplicating code by creating reusable functions and classes. This reduces redundancy, minimizes maintenance efforts, and promotes a more organized codebase.

### 4.3. Function Arguments

Proper handling of function arguments is crucial for creating clear and manageable code. The book provides several guidelines for function arguments:

- **Ideal Number of Arguments:** Functions with no arguments are the easiest to understand and use. They simplify the function's interface and eliminate dependencies, making the function more versatile. Functions with one or two arguments are manageable and usually clear in their purpose. They strike a balance between functionality and simplicity, making the function easy to use and understand. If a function requires three arguments, there should be a clear and compelling reason for it. Functions with three arguments can become difficult to manage and understand, so it's important to evaluate whether all three are truly necessary.

- **Avoid Flags as Arguments:** Using flags as arguments indicates that the function is doing more than one thing, which violates the single responsibility principle. Instead, consider splitting the function into multiple functions, each with a clear and focused purpose.

- **Reduce Function Arguments:** Group related parameters into an object. This not only reduces the argument count but also improves code organization and readability. By encapsulating related data into a single object, the function signature becomes cleaner and more intuitive.

### 4.4. Function Naming

Clear and descriptive function names are crucial for maintaining understandable code. Adhering to a consistent naming convention helps achieve this:

- **Verb + Noun Structure:** Function names should follow a verb + noun structure for clarity. This format clearly indicates the action being performed and the target of that action. For example:
  - Incorrect: `write()`
  - Correct: `writeField()`


### 4.8. Readable Code Flow

The code should be structured so that it can be read and understood from top to bottom. This linear flow makes it easier to follow the logic and understand the program's behavior. Organizing code in this manner enhances readability and aids in debugging.

Think of your code as telling a story. Each function and class should contribute to this narrative, making the code intuitive and engaging. The structure should guide the reader through the logic in a natural and logical sequence, ensuring that the overall flow of the code is coherent and easy to follow.

### 4.11. Comments and Docstring

Docstrings and comments have their place, but they should be used judiciously:

- **Rewrite Bad Code, Don't Comment It:** Instead of writing comments to explain bad code, take the time to rewrite the code to make it clearer and more understandable. Well-written code should not need additional explanations.

- **Comments Are a Necessary Evil:** While comments can be useful, they are often a sign that the code is not as clear as it should be. Aim to write code that is self-explanatory. Clear code reduces the need for comments, making the codebase easier to maintain.

- **Comments Indicate Failure:** Comments often compensate for our failure to express intent and functionality directly in the code. Strive to make the code itself communicate its purpose. If you find yourself needing to add a comment, consider if the code can be improved instead.

- **Comments Can't Fix Bad Code:** Adding comments to bad code doesn't make it better. Focus on improving the code rather than explaining it. Well-structured and self-explanatory code is far more valuable than extensive commenting.

- **Avoid Misleading Comments:** The worst kind of comment is one that misleads the reader. Always ensure that your comments are accurate and helpful. Inaccurate comments can cause confusion and lead to errors, undermining the code's integrity.

- **Not All Functions Need Docstring:** Only include a docstring for functions where the purpose or usage is not immediately clear from the function name and implementation. Overuse of docstring can clutter the code, while strategic use can provide necessary context.

- **Avoid Misinformation:** Every comment or docstring must be accurate and avoid providing misleading information. Misinformation can be more damaging than no information at all, leading to misunderstandings and potential errors.

### 4.13. Formatting

Proper formatting is essential for creating readable and maintainable code. The book outlines several key principles for effective formatting:

- **Agree on a Single Set of Rules:** The team must agree on a single set of simple, consistent formatting rules. Consistency in formatting helps everyone understand and navigate the code more easily. Establishing and adhering to these rules ensures uniformity across the codebase.

- **Consistency Over Time:** While functionality is likely to change, maintaining consistent format and readability is essential. Good formatting ensures that the code remains understandable and maintainable over time, despite changes in functionality.

- **Vertical Distance Matters:** The vertical distance between lines of code or functions is important. Proper spacing helps separate distinct sections of code, making it easier to read and comprehend. Adequate spacing enhances visual clarity and reduces cognitive load.

- **Class Variable Declaration:** Always declare class variables at the top of the class. This practice makes it easy to see the state and properties of the class at a glance. It provides a clear overview of the class structure and its variables.

- **Proximity of Dependent Functions:** Functions that depend on each other should be placed close together. This proximity reduces the cognitive load of jumping around the code to understand dependencies. Grouping related functions improves readability and comprehension.

- **Order of Function Calls:** Place the function call before the function that is being called. This ensures a logical flow when reading from top to bottom, aiding in understanding the sequence of operations. It helps maintain a coherent narrative in the code.

- **Max Line Length:** Set a maximum line length, typically between 80 and 100 characters. This limit helps maintain readability, especially when viewing code on smaller screens or in side-by-side comparisons. Limiting line length prevents horizontal scrolling and ensures that code is easy to read in various environments.

### 4.14. Error Handling

Proper error handling is crucial for creating robust and reliable software. Here are the key principles for effective error handling:

- **Use Exceptions:** Error handling should be isolated in its own function. Encapsulating try/except blocks keeps the main logic clean and focused, while also ensuring that error handling is consistent and reusable. This approach separates error management from business logic, enhancing code clarity.

Use exceptions to handle errors gracefully and ensure that your program can recover or fail predictably. Proper use of exceptions can help maintain the program's stability and provide useful feedback.

  ```python
  def function_xyz():
      try:
          function_abc()
      except TypeError as e:
          print(f"TypeError: {e}")
      except Exception as e:
          print(f"Exception: {e}")
  ```

- **Graceful Degradation:** Using `raise` in conjunction with `try...except` blocks allows your program to degrade gracefully in the face of errors, ensuring it can continue to operate in a reduced capacity if necessary. This approach helps maintain functionality even when some parts of the program encounter issues.

- **Modular Error Handling:** Raise exceptions in low-level functions where specific errors can be detected. Handle these exceptions in higher-level functions, where more informed decisions can be made about how to respond. This separation of concerns helps keep error handling organized and effective.

- **Logging and Monitoring:** Use exceptions to log and monitor errors. This practice helps in diagnosing issues and improving the software over time. Consistent logging provides valuable insights into the program's behavior and can guide future development and debugging efforts.

- **Feedback to Developer:** Provide meaningful feedback to developers through clear and informative error messages. This makes it easier to debug and fix issues. Clear error messages help identify the source and nature of problems quickly, reducing the time and effort required to resolve them.

### 4.15. Boundaries

Establishing clear boundaries in your code is essential for maintaining flexibility, reducing dependencies, and protecting your core logic from external changes. Here are the key principles for managing boundaries effectively:

- **Reduce Dependencies:** Minimize dependencies on third-party code or other teams' work by establishing clear boundaries. This helps integrate foreign code smoothly without tightly coupling it to your own codebase. By isolating dependencies, you make your code more modular and easier to maintain.

  ```python
  class Service:
      def __init__(self, dependency):
          self.dependency = dependency

      def regular_action(self):
          self.dependency.do_complex_stuff()
  ```

- **Accommodate Change:** Design your software to accommodate changes without requiring significant investments of time and resources. By clearly defining boundaries, you make it easier to adapt to changes in third-party code or external dependencies. This approach ensures that your software remains flexible and resilient to modifications.

- **Protect Investment:** When using code outside of your control, such as third-party libraries or APIs, take special care to protect your investment. Isolate third-party code and ensure that your core logic remains unaffected by changes or updates to these external dependencies. This isolation helps maintain the stability and reliability of your software.

- **Minimize Exposure:** Avoid letting too much of your codebase be aware of the specifics of third-party code. By encapsulating interactions with external systems, you create fewer maintenance points, making your software more resilient to changes. Encapsulation reduces the impact of external changes on your code and makes it easier to manage dependencies.

### 4.16. Unit Tests

Effective unit testing is essential for maintaining high-quality software. The book outlines several principles and practices to ensure that unit tests are valuable and maintainable:

- **Test-Driven Development Laws:**
  1. **Fail First:** You may not write production code until you have one failing unit test. This ensures that all production code is tested from the beginning.
  2. **Minimal Test:** You may not write more of a unit test than is sufficient to fail. This encourages writing only necessary tests, keeping them concise and focused.
  3. **Minimal Code:** You may not write more production code than is sufficient to pass the currently failing test. This keeps the code simple and prevents over engineering.

- **Evolving Tests:** As production code evolves, tests must also change. Keeping tests up-to-date with the code ensures they remain relevant and useful, providing accurate feedback about the code's behavior.

- **Avoid Dirty Tests:** Dirty tests are hard to maintain and become a liability. Well-maintained tests are essential for ensuring code quality and reliability. Clean and clear tests make it easier to identify and fix issues.

- **Importance of Tests:** Without tests, it is impossible to verify if changes work as expected and to ensure that changes in one part of the code do not break other parts. Test code is as important as production code and should not be treated as a second-class citizen. Prioritizing test quality ensures robust and reliable software.

- **Benefits of Unit Tests:** Unit tests keep your code flexible, maintainable, and reusable. Good tests allow you to make changes with confidence, knowing that you can quickly detect and fix any issues that arise. They provide a safety net that supports continuous improvement and refactoring.

- **Clean Tests:**
  - **Readability:** Tests should be clear, expressive, and succinct. Simplicity and a high density of expression (achieving a lot with a few expressions) are key. Readable tests make it easier for developers to understand what is being tested and why.
  - **Test Structure:** Tests should be structured in a clear and logical manner, often following the pattern:
    ```python
    def test_something():
        given_abc()
        when_request_issued()
        then_response_should_be()
    ```
  - **Single Assert:** Each test must have only one assert to ensure it tests a single context and provides clear, focused results. This approach reduces ambiguity and simplifies debugging.

- **Rules for Good Tests:**
  - **Fast:** Tests should run quickly to provide immediate feedback. Slow tests can hinder development and reduce the likelihood of frequent test runs.
  - **Independent:** Tests should be able to run in any order without dependencies on other tests. Independence ensures that tests do not influence each other, leading to more reliable results.
  - **Repeatable:** Tests should be repeatable in any environment to ensure consistent results. This consistency is crucial for detecting real issues and not environment-specific anomalies.
  - **Self-Validating:** Tests should produce a boolean output (pass or fail) to clearly indicate success or failure. Self-validating tests make it easy to determine the state of the code.
  - **Timely:** Write tests just before the production code that makes them pass. This ensures that the tests are relevant and aligned with the current development focus, supporting an iterative and test-driven development process.

### 4.17. Insufficient and Wrong Tests

Determining the right number of tests is crucial. Many programmers rely on the metric "That seems like enough," but a test suite should test everything that could possibly break. Tests are insufficient if there are conditions that have not been explored or calculations that have not been validated.

- **Use a Coverage Tool:** Coverage tools report gaps in your testing strategy. They help identify modules, classes, and functions that are insufficiently tested. Most IDEs provide visual indicators, marking lines that are covered in green and those that are uncovered in red, making it quick and easy to find code that hasn't been tested.

- **Don't Skip Trivial Tests:** Trivial tests are easy to write and their documentary value is higher than the cost to produce them. They can provide valuable insights and ensure comprehensive coverage.

- **An Ignored Test Is a Question about an Ambiguity:** Sometimes we are uncertain about a behavioral detail because the requirements are unclear. Express your question about the requirements as a test that is commented out or annotated with @Ignore, depending on whether the ambiguity is about something that would compile or not.

- **Test Boundary Conditions:** Take special care to test boundary conditions. We often get the middle of an algorithm right but misjudge the boundaries, which can lead to errors.

- **Exhaustively Test Near Bugs:** Bugs tend to congregate. When you find a bug in a function, it is wise to do an exhaustive test of that function. You'll probably find that the bug was not alone.

- **Patterns of Failure Are Revealing:** Diagnose problems by finding patterns in the way test cases fail. Complete test cases, ordered in a reasonable way, expose patterns that can help in diagnosing issues.

- **Test Coverage Patterns Can Be Revealing:** Analyzing which parts of the code are or are not executed by passing tests can provide clues to why the failing tests fail. This insight helps in pinpointing problematic areas.

- **Tests Should Be Fast:** A slow test is a test that won't get run. When time is tight, slow tests are the first to be dropped from the suite. Ensure your tests run quickly to encourage regular execution.

### 4.18. Classes

Effective class design is crucial for creating maintainable and understandable software. The book outlines several principles to ensure that classes are well-structured and adhere to best practices:

- **Start with Public and Constant Variables:** Begin any class by declaring public and constant variables. This practice makes it easy to see the state and constants the class exposes, providing a clear overview of its external interface.

- **Keep Classes Small:** Classes should be small, not in terms of lines of code, but in terms of responsibilities. Each class should have a clear, single responsibility, aligning with the Single Responsibility Principle.

- **Measure by Responsibilities:** The length of a class is measured by its responsibilities, not by its lines of code. A class should have only one reason to change and one responsibility, ensuring it remains focused and cohesive.

- **Focus on Organization and Cleanliness:** We are not done when the code works; we need to ensure it is well-organized and clean. Well-organized code is easier to maintain and extend, reducing long-term technical debt.

- **Organization Over Quantity:** More classes do not necessarily mean more complexity. The number of moving parts is the same, but the organization differs. Multiple small, well-labeled, and organized classes are preferable to one big, cluttered class.

- **Small Number of Methods:** Classes should have a small number of methods to ensure they remain focused and manageable. This helps maintain clarity and simplicity in the class design.

- **Cohesion:** 
  - Maximal cohesion is achieved when each variable is used by each method. The more variables a method manipulates, the more cohesive that method is.
  - Keeping functions small and parameter lists short might increase the number of instance variables and methods. When this happens, consider decoupling and creating new classes to maintain cohesion.

- **Why Single Responsibility Principle is Important:**
  - **Easy to Maintain:** Code is easier to maintain when each class has a single responsibility. This reduces the complexity of each class, making it more understandable and easier to manage.
  - **Easy to Test:** Each class can be tested individually with unit tests, ensuring reliability. Focused classes with a single responsibility are simpler to isolate and test.
  - **Reusability:** Single-responsibility classes are more reusable across different parts of the codebase. Clear and well-defined responsibilities make it easier to integrate classes in various contexts.
  - **Robustness:** Code adhering to the Single Responsibility Principle is less fragile and more resistant to changes. This reduces the risk of introducing bugs when modifying the code.

- **Why More Cohesion => More Classes:** Consider a large function A. If you split A into smaller functions A and B, and B uses many arguments created in A, passing many arguments can decrease cohesion. Instead, move these arguments to instance variables. However, this approach might reduce cohesion. To maintain cohesion, create a class that encapsulates both A and B, ensuring that related functionalities are grouped together in a cohesive manner.

### 5. Iterative and Incremental Development

It is a myth that we can get everything perfect the first time. Instead, we should build systems to handle today's needs, then iterate and improve them based on new requirements. This approach, known as iterative and incremental development, allows for continuous improvement and adaptation.

Unlike physical systems, software systems can grow incrementally from simple to complex if we maintain the proper separation of concerns. This means organizing the system so that different parts handle different responsibilities, which allows for easier expansion and maintenance.

- **Incremental Growth:** Software systems should be built to grow and adapt over time. Separate different concerns within the system to make it easier to manage and expand. Use techniques like Aspect-Oriented Programming (AOP) to handle cross-cutting concerns efficiently.

- **Avoiding Big Design Up Front (BDUF):** BDUF involves planning every detail of a system before starting to build it. This approach can be harmful because it makes it hard to adapt to changes. Instead, it's better to start with a simple, decoupled design and iterate as you go. This allows for flexibility and easier adjustments based on new requirements.

- **Optimizing Decision Making:** Good software architecture should be modular and decoupled, allowing for decentralized decision making. This enables different teams or individuals to make decisions about different parts of the system, making the system more adaptable and easier to manage.

- **Emergent Design:** refers to the practice of allowing the design of a system to evolve naturally through the process of writing and refactoring code. This approach focuses on simplicity and adaptability, ensuring that the design meets current needs without unnecessary complexity. By continuously refining and improving the design, developers can create clean, efficient, and maintainable software.

## 6. Concluding Remarks

### 6.1. Simple Rules to Remember

Kent Beck's four rules of Simple Design guide developers in creating high-quality software that is easy to understand, maintain, and extend. These rules are:

1. **Runs all the tests:** Ensuring that the system runs all the tests verifies that it functions as intended. Making a system testable often leads to better design. Testable designs typically feature small, single-purpose classes and follow principles like the Single Responsibility Principle (SRP) and Dependency Inversion Principle (DIP). Tight coupling between classes can make testing difficult, but by writing tests and ensuring testability, developers are pushed toward decoupling classes and improving design cohesion.

2. **Contains no duplication:** Duplication increases complexity, risk, and maintenance efforts. Types of duplication include code duplication, where identical or similar code blocks can be refactored to eliminate redundancy, and implementation duplication, where different methods performing similar tasks can be unified.

3. **Expresses the intent of the programmer:** Code should be clear and expressive to reduce maintenance costs and defects. Use meaningful names and standard nomenclature to make the intent of the code understandable to other developers. Keeping functions and classes small enhances readability and expressiveness. Utilizing standard design patterns like Command or Visitor helps in expressing the design succinctly.

4. **Minimizes the number of classes and methods:** While reducing duplication and being expressive, avoid creating too many tiny classes and methods unnecessarily. Aim to keep the overall system small by balancing the need for expressiveness and simplicity. Adhere to the principle of keeping things simple without becoming dogmatic about class and method counts.

### 6.2. About Incrementalism

One of the best ways to ruin a program is to make massive changes to its structure in the name of improvement. Some programs never recover from such “improvements.” The problem is that it’s very hard to get the program working the same way it worked before the “improvement.” To avoid this, use the discipline of Test-Driven Development. One of the central doctrines of this approach is to keep the system running at all times. In other words, using Test-Driven Development, you are not allowed to make a change to the system that breaks that system. Every change must keep the system working as it worked before.

To achieve this, maintain a suite of automated tests that can be run on a whim and verify that the behavior of the system is unchanged. For example, if you create a suite of unit and acceptance tests while building a complex class, you can run these tests anytime to ensure the system works as specified.

By making a large number of very tiny changes, you can gradually move the structure of the system toward a better design without breaking it. Much of good software design is about partitioning—creating appropriate places to put different kinds of code. This separation of concerns makes the code much simpler to understand and maintain.

### 6.3. General Conclusion

It is not enough for code to work. Code that works is often badly broken. Programmers who satisfy themselves with merely working code are behaving unprofessionally. They may fear that they don’t have time to improve the structure and design of their code, but nothing has a more profound and long-term degrading effect upon a development project than bad code. Bad schedules can be redone, bad requirements can be redeployed, bad team dynamics can be repaired, but bad code rots, becoming an inexorable weight that drags the team down.