# Journal – Andrea Bonifacio Scarpitta

## Entry 1 - Week of 27 April to 3 May

**Activities:**

- Divided the project tasks into two parts: Software Design (Shadmehr and Peyman) and Software Architecture (Andrea and Leonardo).
- Started working on the Software Architecture section by exploring the source code of Express.js, its components and their interactions.

## Entry 2 - Week of 4 May to 10 May

**Activities:**

- Collaborated with Leonardo to define the report structure and outline for the Software Architecture section.
- Divided tasks for the Software Architecture report.
- Brainstormed and discussed the first layer of the C4 model (Context) 
- Brainstormed, discussed and designed the diagram for the second layer of the C4 model (Container) identified the interactions between the main containers of Express.js.

## Entry 3 - Week of 11 May to 17 May

**Activities:**

- Shifted the focus of the analysis to the third layer of the C4 model (**Component Level**), concentrating exclusively on the modules located within the `lib/` directory and the `index.js` entry point.
- Identified and mapped with Leonardo the 6 core components of the framework (`express.js`, `application.js`, `request.js`, `response.js`, `utils.js`, and `view.js`), isolating them from external dependencies resolved through npm.
- Worked on the design and layout of the Level 3 Component diagram together with Leonardo.
- Documented the architectural responsibilities of each module in a detailed table, analyzing metrics such as lines of code and interactions with native Node.js APIs (e.g., the extensions of `http.IncomingMessage` and `http.ServerResponse`).

## Entry 4 - Week of 18 May to 24 May

**Activities:**

- Analyzed the key **architectural characteristics** of the framework: extensibility, minimalism, and performance-oriented design decisions (such as lazy Router initialization and the use of the Strategy pattern in `utils.js`).
- Evaluated the degree of internal and external coupling (**Efferent and Afferent Coupling**), studying how Express delegates responsibilities to external npm packages (high Ce) and how the surrounding ecosystem depends rigidly on its public APIs (high Ca).

## Entry 5 - Week of 25 May to 31 May

**Activities:**

- Participated in a **meeting with the professor** to discuss our C4 diagrams; the meeting was essential for clarifying key modeling concepts and resolving our remaining doubts before submission.
- Completed the final part of the architectural analysis by focusing on **module cohesion**, classifying components based on their purpose (from the high functional cohesion of `view.js` to the weaker, facade-style cohesion of `application.js`).
- Drafted the conclusions of the report with Leonardo, summarizing the architectural trade-offs of Express.js: the choice to prioritize pragmatism and developer ergonomics over strict academic purity.
- Conducted a review and proofreading session of the entire "Software Architecture" section with Leonardo, refining the C4 diagrams based on the professor's guidelines.
- Met with Shadmehr and Peyman to integrate our section with the "Software Design" part, ensuring the overall consistency of the final document and formatting cross-references for the design patterns used.

## Entry 6 - Week of 1 June to 7 June

**Activities:**

- Fixed minor formatting issues and improved the clarity of some sections in the report based on internal discussions.
- Made final retouches to the tables
- Conducted a final review of the entire report with the team, ensuring that all sections were cohesive and that the diagrams effectively complemented the written analysis.
