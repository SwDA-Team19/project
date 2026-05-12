# Journal - Peyman Rivandi

## Entry 1 - Week of 13 and 20 April

We picked Express.js as our system together with Shadmehr, Andrea, and Leonardo, and registered on the CryptPad sheet. After that, Shadmehr and I started going through the codebase. We read the README and History.md first just to get a feel for how the project evolved, then went through all six files in lib/ to understand what each one does.

I also spent some time on the GitHub page and package.json to get a sense of the project's health, things like how many contributors there are, how active the releases are, and what the 28 dependencies are actually for. Shadmehr and I talked through the stakeholders and where their interests might clash, especially around keeping things backward compatible while the framework also needs to move forward.

For the Overview report I mainly worked on the Development Activity section and the part about stakeholder conflicts.

Effort: ~5 hours

Contributions to report:
Overview: Development Activity; stakeholder conflict analysis in Purpose and Stakeholders.

---

## Entry 2 - Week of 30 April and 4 May

We split the work with the team at the start of this week, Shadmehr and I took Software Design, Andrea and Leonardo took Software Architecture. Shadmehr and I then split our part further: he focused on mapping the dependencies while I took the lead on the design patterns, though we talked through everything together as we went.

I went back through the lib/ files looking for patterns this time, and we ended up identifying four: Factory in express.js (how createApplication() wires everything up), Chain of Responsibility in the middleware stack, Template Method in how app.render() delegates to whatever engine you plug in, and Strategy in the compileETag and compileQueryParser functions in utils.js. For each one we found the exact lines, figured out which parts play which role, and thought through what an alternative approach would look like and what the trade-offs would be.

I also looked over Shadmehr's dependency analysis and checked his co-change observations against History.md myself. We wrote the Summary section together at the end to tie both parts of the report into a coherent picture.

Effort: ~6 hours

Contributions to report:
Design: pattern analyses (Factory, Chain of Responsibility, Template Method, Strategy); Summary (jointly with Shadmehr).
