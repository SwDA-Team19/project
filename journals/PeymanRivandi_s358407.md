# Journal - Peyman Rivandi

## Entry 1 - Week of 13 and 20 April

We picked Express.js as our system together with Shadmehr, Andrea, and Leonardo, and registered on the CryptPad sheet. After that, Shadmehr and I started going through the codebase. We read the README and History.md first just to get a feel for how the project evolved, then went through all six files in lib/ to understand what each one does.

I also spent some time on the GitHub page and package.json to get a sense of the project's health, things like how many contributors there are, how active the releases are, and what the 28 dependencies are actually for. Shadmehr and I talked through the stakeholders and where their interests might clash, especially around keeping things backward compatible while the framework also needs to move forward.

For the Overview report I mainly worked on the Development Activity section and the part about stakeholder conflicts.

Effort: ~5 hours

Contributions to report:
Overview: Development Activity; stakeholder conflict analysis in Purpose and Stakeholders.

---

## Entry 2 - Week of 27 April and 4 May

We split the work with the team at the start of this week, Shadmehr and I took Software Design, Andrea and Leonardo took Software Architecture. Shadmehr and I then split our part further: he focused on mapping the dependencies while I took the lead on the design patterns, though we talked through everything together as we went.

I went back through the lib/ files looking for patterns this time, and we ended up identifying four: Factory in express.js (how createApplication() wires everything up), Chain of Responsibility in the middleware stack, Template Method in how app.render() delegates to whatever engine you plug in, and Strategy in the compileETag and compileQueryParser functions in utils.js. For each one we found the exact lines, figured out which parts play which role, and thought through what an alternative approach would look like and what the trade-offs would be.

I also looked over Shadmehr's dependency analysis and checked his co-change observations against History.md myself. We wrote the Summary section together at the end to tie both parts of the report into a coherent picture.

Effort: ~6 hours

Contributions to report:
Design: pattern analyses (Factory, Chain of Responsibility, Template Method, Strategy); Summary (jointly with Shadmehr).

---

## Entry 3 - Week of 11 and 18 May

These two weeks I made the diagrams for the Design report. I drew one for the code dependency graph and one for each of the four patterns: Factory, Chain of Responsibility, Template Method, and Strategy, and put them into Design.md. It took a while to get them looking right but I think they make the report a lot clearer.

After that, Shadmehr and I went through the Design report together one more time to check everything was consistent, the dependency section lined up with the pattern analyses, and nothing was missing or off. That was the last thing we needed to do on the Design side.

I also read through the Architecture file that Andrea and Leonardo put together, just to see how the two reports fit together and make sure there was nothing conflicting between what we wrote and what they wrote.

Effort: ~9 hours

Contributions to report:
Design: diagrams (dependency graph, Factory, Chain of Responsibility, Template Method, Strategy); final review (with Shadmehr).

---

## Entry 4 - Week of 25 May and 1 June

The four of us met to bring the Design and Architecture sections together into one report. Andrea and Leonardo had been working on the C4 diagrams and the architecture analysis separately, so this was the first time we sat down and compared the two parts. We went through the cross-references, made sure the pattern descriptions and architecture diagrams were saying consistent things about the same modules, and fixed a few spots where the wording didn't match.

I also went back through the Design file on my own and read it against the project requirements more carefully. There were a few small things to fix, some wording and minor formatting issues, nothing major but worth sorting before the final submission.

Effort: ~3 hours

Contributions to report:
Design: consistency review and minor fixes; integration with Architecture section (with full team).

---

## Entry 5 - Week of 8 June

This was mostly a final check. I read through the whole report one more time to catch anything still off, formatting, phrasing, things that didn't read well. We did a last pass as a team to make sure everything was in order and the diagrams and text were all aligned before submitting.

Effort: ~2 hours

Contributions to report:
Design: final proofread and formatting fixes.
