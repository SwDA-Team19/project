# Journal - Leonardo Farfan Rodriguez

## Entry 1 - Week of 27 April to 3 May

**Activities:**

- Divided the project tasks into two parts: Software Design (Shadmehr and Peyman) and Software Architecture (Andrea and Leonardo).
- Started working on the Software Architecture section by exploring the source code of Express.js, its components and their interactions.

## Entry 2 - Week of 4 May to 10 May

**Activities:**

- Collaborated with Andrea to define the report structure and outline for the Software Architecture section.
- Divided tasks for the Software Architecture report.
- Brainstormed, discussed and wrote the first layer of the C4 model (Context) and identified the main stakeholders and their interactions with the system.

## Entry 3 - Week of 11 May to 17 May

**Activities:**

- Shifted the focus of the analysis to the third layer of the C4 model (**Component Level**), concentrating exclusively on the modules located within the `lib/` directory and the `index.js` entry point.
- Identified and mapped with Andrea the 6 core components of the framework (`express.js`, `application.js`, `request.js`, `response.js`, `utils.js`, and `view.js`), isolating them from external dependencies resolved through npm.
- Worked on the design and layout of the Level 3 Component diagram together with Andrea.

## Entry 4 - Week of 18 May to 24 May

**Activities:**

- Conducted an analysis of the Express.js codebase based on **SOLID principles**, highlighting both its strengths (the exemplary implementation of OCP through the middleware pipeline) and violations of the Single Responsibility Principle (SRP) found in `application.js` and `response.js`.
- Analyzed the key **architectural characteristics** of the framework: extensibility, minimalism, and performance-oriented design decisions (such as lazy Router initialization and the use of the Strategy pattern in `utils.js`).

## Entry 5 - Week of 25 May to 31 May

**Activities:**

- Participated in a **meeting with the professor** to discuss our C4 diagrams; the meeting was essential for clarifying key modeling concepts and resolving our remaining doubts about the proper level of abstraction for our diagrams before submission.
- Fixed the diagrams based on the professor's feedback, simplifying the Level 2 Container diagram.
- Drafted the conclusions of the report with Andrea, summarizing the architectural trade-offs of Express.js: the choice to prioritize pragmatism and developer ergonomics over strict academic purity.
- Conducted a review and proofreading session of the entire "Software Architecture" section with Andrea, refining the C4 diagrams based on the professor's guidelines.
- Met with Shadmehr and Peyman to integrate our section with the "Software Design" part, ensuring the overall consistency of the final document and formatting cross-references for the design patterns used.

## Entry 6 - Week of 1 June to 7 June

**Activities:**

- Fixed minor formatting issues and improved the clarity of some sections in the report based on internal discussions.
- Changed the language of the diagrams from PlantUML to Structurizr, which resulted in more consistent and visually appealing diagrams, especially for the Level 3 Component diagram.
- Made final retouches to the diagrams, to ensure they were clear, consistent between each other, and aligned with the checklist provided in the slides.
- Added hyperlinks in the reports to better connect the Software Design and Software Architecture sections, ensuring readers can easily navigate between related concepts and patterns discussed in both sections.
- Conducted a final review of the entire report with the team, ensuring that all sections were cohesive and that the diagrams effectively complemented the written analysis.
