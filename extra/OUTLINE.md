# Project Planning #

<i>Please see [THE-GIST](THE-GIST.md) for project description.</i>

___

### Current Stage ###

- Research
- Planning
- ▷ Experimenting ◁
- Implementing
- Testing
- Debugging
- Resolution
- Polishing
- Formatting
- Documentation
- Application
- Presentation

___

### Completed Tasks ###

1. [x] Research project requirements and find related resources.
2. [x] Create local Rust toolchain.
3. [x] Confirm local toolchain is installed and running correctly.
4. [x] Write project summary.
5. [x] Write project outline.

___

### Current Task ###

1. [ ] Reserve test symbol as a restricted character.

___

### Planned Tasks ###

1. [ ] Reserve test symbol as a restricted character.
2. [ ] Reserve `class` symbol as new keyword.
3. [ ] Mirror `trait` functionality through `class` keyword.
4. [ ] Invert `trait` restrictions on `const` for `class`.
5. [ ] Differentiate use of `trait` and `class` in the trait solver.
6. [ ] Use `class` in example application.
7. [ ] Differentiate `trait` and `class` in lints.
8. [ ] Begin formal documentation process for `class`.
9. [ ] Create mock-trial file demonstrating use of `class`.
10. [ ] Release results and seek feedback before further action.
11. [ ] Create additional objectives based on external feedback.

___

### Expected Problems ###

1. Reserving `class` as a keyword will likely break many crates.
    - Just adding a new keyword will likely cause many severe problems.
    - The word `class` could potentially be used heavily across several crates.

2. Mirroring the functionality of `trait` on a different keyword may break the trait solver.
   - It was likely not intended for the trait solver to work with two keywords for a trait.
   - It will likely be very challenging getting both traits and classes to work in unison.

3. Moving the functionality of `const trait` to `class` will likely require significant changes to several critical systems.
   - Changes to the functionality of `const trait` may also prove just as difficult.
   - While much of the task will be focused on mirroring how traits already work, it is likely that there are several new problems introduced by classes.
   - Based on the current situation around `const trait`, it may prove impossible to prevent non-constant operations from being introduced to a class, therefore making classes impossible to be properly constrained.

4. Properly setting up infrastructure for classes will likely require significant effort.
    - The definition of keywords and how to handle them is spread out across `rustc`, so adding the setup for `class` may require a thorough analysis of every single item in that directory.
    - In the possibility that this project requires complete rewrites of multiple files, I'm just ditching the project straight up.

5. Information about how to do any of this seems to be less generally established and more private in nature.
   - If and when problems come up that I cannot solve on my own, I will likely need to communicate with a professional developer.
   - While I'm sure this project is at least somewhat interesting, I have little faith that anyone who understands that much about the compiler would have too much time for a kid trying to mess around with a hobby project.
   - If this project isn't immediately rejected, it will likely end up on a todo list and receive very little assistance for quite some time.

6. I'm not a professional developer.
   - I have never done anything like this in my life.
   - I'm going off of guesstimations and documentation written for people who actually know what they're doing.
   - Every project I've finished up to this point has been either totally useless or completely obsolete.
