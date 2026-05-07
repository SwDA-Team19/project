# Journal – Shadmehr Rahmati

## Entry 1 – Week of 13 and 20 April

**Activities:**

- Selected Express.js as the project system with Team and registered the team on the CryptPad sheet under the Project tab.
- Read through the `README.md` and `History.md` to get a basic of the project's history, governance model, and ...
- Browsed the full `lib/` directory (6 files) reading each module top to bottom: `express.js`, `application.js`, `request.js`, `response.js`, `utils.js`, and `view.js`.
- Checked the GitHub repository page and `package.json` to record the number of external runtime dependencies (28), GitHub stars, and contributor count.
- Read the OpenJS Foundation governance documentation and the Express contributing guide to understand the structure
- Identified the main stakeholder groups and thought through where their interests align and where they conflict.
- Drafted all three sections of the Overview report: Purpose and Stakeholders, System Description, and Development Activity.

**Effort:** ~5 hours

**Contributions to report:**
- Overview: all sections — Purpose and Stakeholders, System Description, Development Activity.

---
## Entry 2 – Week of 30 April and 4 May

**Activities:**

- Divided the project tasks into two parts: Software Design (Shadmehr and Peyman) and Software Architecture (Andrea and Leonardo).
- Re-read all six files in lib/ with a focus on require() calls to map the full dependency graph, both internal and external.
- Used grep to extract every require statement per module and built the directed dependency graph manually, noting which modules are leaves (utils.js, view.js, request.js) and which have wide fan-out (response.js with 12 external deps).
- Analysed History.md across Express 5.x milestones to identify co-change clusters — which files tend to be modified together — and compared these against the static import graph to find inconsistencies.
- Identified the key inconsistency: request.js and response.js co-change frequently despite having no direct require relationship, driven by protocol-level cohesion rather than code structure.
- Identified four design patterns in the codebase: Factory (createApplication() in express.js), Chain of Responsibility (middleware stack via t he router package), Template Method (app.render() + pluggable engine in application.js / view.js), and Strategy (compileETag / compileQueryParser in utils.js).
- For each pattern: located the exact files and line numbers, identified which classes play which role, wrote up the problem it solves, and evaluated a concrete alternative with pros and cons.
- Drafted the full Design report: Dependencies section (method, code graph, co-change analysis and inconsistencies), all four pattern analyses, and the Summary tying both sections back to Express's two overriding design goals.

**Effort:** ~7 hours

**Contributions to report:**

- Design: all sections — Method and Tools, Code Dependency Graph, Knowledge Dependencies (Co-change), all four pattern analyses (Factory, Chain of Responsibility, Template Method, Strategy), Summary.

