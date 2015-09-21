# Transpiling Recomponsed

## Who am I

Podio Frontend Lead

CopenhagenJS Organizer

Assetgraph Author

## What i believe

I have spent a lot of time thinking about build tools.

This year I have spent more time thinking about how to make tools simpler for the next generation. Partially because I'm mentoring more, but also for more personal reasons.

Given this focus, teaching newcomers, let me start out with a bold statement.

Transpiler tooling is a problem (example of transpilers). Transpilers themselves are awesome unixy doing-one-thing-well tools. But the way we wrap them is problematic.

Lets me explain


## The problem

I believe that task runners, as a concept, are a fundamentally wrong abstration to transpiling.

I also believe that task runners were a very important foundation for tools to evolve around.

I think we are caught in a local optimum and that we need new ideas to improve.

## The solution


## Demo


## Details about Fusile


## Thanks


---------------------

## Ideal setup

- No configuration of integration
- On demand transpiling
- Doesn't require a full ecosystem
- Plays well with other tools
-




------------------

# What are transpilers

- Sass / Less / Stylus / Autoprefixer
- Babel / TypeScript / CoffeeScript
- Jade / Markdown

Simple tools. Unix 1-to-1 comman line. They enable us you use the abstraction we like, give us access to powerful new features immediately

# The Problem

But there are some problems we face when using transpilers. The main problem is how to deal with the build artifact.

Lets set up a project with a transpiler as an example of this.

Figure:
  - Source code --> build artifact in same directory. Simple and easy, but gets you in trouble with version control with clashing file extensions or handling ignores.
  - So we move the build artifacts to a different directory. Version control problems solved. But now your HTML, JS and CSS are no longer pointing to the correct path on disk
  - So we build a web server that looks for files first in the build artifact folder, then in the source folder
  - Running the transpiling manually gets tedious, so we set up a file watcher to trigger transpiling on each source file increment
  - And you probably also need a task runner to orchestrate all of this
  - When you set up a build system, this has to also know where to find the build artifacts, or if not, it has to redo compilation in its own temporary directory

Does that look about right?

Not really. This only covers the CSS flow. For JS you might also need:
  - Transpilers plugins for your test runner
  - Transpiler plugins for your module loader
  - Transpiler plugins for your linter
  - Any other types of tools

Lets take a step back and look at this again. Does it look simple? (No)

So what we do is wrap a bow around it to make it easy: "Yo webapp"

But easy is not simple. And when a newcomer tries to learn frontend development and something goes wrong with this setup, they hit a wall of massive complexity.

They are not able to buy into the added benefits of transpilers without also having to buy and entire ecosystem filled with complexity.

I often hear senior developers counter my view that this should be a problem, with: "My setup is complex / legacy / special, so we need the complexity"... "We NEED this complexity"... "NEED"?

I believe the need for complexity is a fallacy. I believe that nobody needs complexty. What you need is ability. And with the current set of tools you could only get this ability by also accepting the complexity. But what if we could figure out a way to get the ability without the complexity?



