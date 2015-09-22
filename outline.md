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

Simple tools. Unix 1-to-1 command line. They enable us you use the abstraction we like, give us access to powerful new features immediately

# The Problem

But there are some problems we face when using transpilers. The main problem is how to deal with the build artifact.

Lets set up a project with a transpiler as an example of this.

Figure:
  - Source code --> build artifact in same directory. Simple and easy, but gets you in trouble with version control with clashing file extensions or handling ignores.
  - So we move the build artifacts to a different directory. Version control problems solved. But now your HTML, JS and CSS are no longer pointing to the correct path on disk
  - So we build a web server that looks for files first in the build artifact folder, then in the source folder
  - Running the transpiling manually gets tedious, so we set up a file watcher to trigger transpiling on each source file increment
  - And you probably also need a task runner to orchestrate all of this
  - When you set up a production build system, this has to also know where to find the build artifacts, or if not, it has to redo compilation in its own temporary directory

Does that look about right?

Not really. This only covers the CSS flow. For JS you might also need:
  - Transpilers plugins for your test runner
  - Transpiler plugins for your module loader
  - Transpiler plugins for your linter
  - Any other types of tools

Lets take a step back and look at this again. Does it look simple? (No)

So what we do is wrap a bow around it to make it easy: "Yo webapp"

But easy is not the same as simple. And when a newcomer tries to learn frontend development and something goes wrong with this setup, they hit a wall of massive complexity.

They are not able to buy into the added benefits of transpilers without also having to buy and entire ecosystem filled with complexity.

I often hear senior developers dismiss that this should be a problem. "My setup is complex / legacy / customized / special, so we need the complexity"... "We NEED this complexity"... "NEED"?

I believe the need for complexity is a fallacy. I believe that nobody needs complexity. What you need is ability. And with the current set of tools you could only get this ability by also accepting the complexity. But what if we could figure out a way to get the ability without the complexity?

# Requirements

I think that most of this complexity comes from wrong abstractions. And I believe that the wrong abstraction for the job of transpiling in a simpler way, is the task runner. Not Grunt, Gulp, Broccoli or any other task runner specifically. Any task runner. All task runners. From Make and up.

I believe that the task runners lack of knowledge of user intention is causing configuration to explode and too much work to be done. And the lack of interoperability with other tools is causing ecosystem lock-in. How many combinations of grunt-sass, gulp-sass or broccoli-sass do we have to see before we realize we are stuck in a local optimum and need to rethink our approach?

Now, task runners are not all bad. In fact I think task runners are one of the primary reasons we have seen an explosion in the ecosystem of tooling. They have provided a stable forum for all the tools to come together. But now that we have all of these high quality tools I think it's time to step back, rethink and recompose.

These are the features I think transpiler tooling should offer:
- Handle all types of transpilers, not just a single one
- Have a good API that other tools easily can integrate with
- Be a stand alone tool that imposes no ecosystem on the user
- Compile on demand
- Keep urls valid
- Sourcemaps
- Autoprefixing
- Caching
- Little to no configuration
- Simple mental model

That's a long list, but let's see what we can do.

# Experiments

With the Assetgraph project I've actually been on a parallel track to task runner based build systems all along, and have gotten by wihtout them for quite a long time.

At one point I used in-browser transpilers with less, or requirejs plugins. These obviously tick very few of the above boxes, since they require a browser environment, loose all build artifacts on page reloads and don't intergrate well with command line tools. But they did give me an idea on the next iteration.

Up until very recently I have been using Express middlewares for my transpiling needs. They have better cahing than the in-browser transpiler, but generally suffer from the same lack of access to build artifacts for tools that don't understand http.

But what they have in common is a reversal of the control flow. The browser runtime expresses a need for an asset and initiates a request for it. The transpiler tool can intercept this request and do the needed transpiling work.

This feature has some interesting implications.
- Assets are compiled just-in-time and on demand. No extra work is ever done to assets that are left untouched by your current workflow.
- Configuration reduces down to nothing, since there is no longer a need for instructions on where to read files or where to put them. This knowledge is implicit in a request

It's still not stand alone though. It does impose the ecosystem requirement of web server with installed middlewares. But I think there is something about this concept that has merit.

The biggest problem left is that integration with most other tools requires build artifact presence on disk. But we don't want the build artifact to clog up our source folder, and we don't want to look for it anywhere else because that would add complexity as well.

So how do we deal with the fact that we want our build artifacts to be transient? Available when we need them, but not require us to configure tools to avoid them when we don't?

If I undeerstand the Broccoli task runner correctly, this is actually what it allows you to do. You can create a full on-disk copy or your entire source folder, but with your assets transpiled. Jo-liss has spent a great deal of effort at making Broccoli really fast at this. But it's not a stand alone tool that is easy to wield, and it does require some research and configuration to make it work.

I'd like to see if I can create something that's even simpler to understand.

# The idea

So what if we could take this transient nature of build artifacts and extend the concept to a complete source folder?

It turns out that we can. Node has had Fuse bindings since 2012, and we even saw a demo using them last year on this very spot.

So this is the idea:
- A fuse mounted directory containing a proxy of your source folder
- Any asset that needs transpiling is transpiled on deman when read
- Retains all the benefits from express middlewares
- Adds a file system API to let other tools intergrate

# Demo time

So this is what I built:



