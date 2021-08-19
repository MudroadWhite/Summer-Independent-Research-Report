# Goals

Set up the container for developing HLS in Docker with VSCode. This includes:

- Download Docker, VSCode extensions, WSL backend, enable virtualization for the machine, disable my android simulator
- Understand how to make a container --> understand how to make a Dockerfile
- Setup the Dockerfile for HLS

# Accomplishments

- Successfully set up Docker and the VSCode environment
- Understood and made a very basic container. In special, there is a devcontainer.json file that works specifically for VSCode to tell it what VSCode extensions should be installed for the container.

# Obstacles

The only obstacle here is the specific Dockerfile. Currently I think the Dockerfile should contain the following steps:

1. Install Haskell and anything that should be related
2. Install Haskell libraries that are necessary to the HLS, which I might have to figure out
3. Possibly, compile the HLS?
4. And then run the HLS in the last line of the command.

> *JGM*: I hadn't thought about building HLS as part of building the Docker image.  That might actually make sense tho---we could then always configure the Haskell plugin to point to the built language server.  As a first step, tho, having a Docker imagine that lets us just run `cabal build` for HLS is enough.

Currently I'm stuck on step 1. I used another script that I found on the internet and it somehow requires me to find a folder called /.ghci, which does not exist. I want to figure out what the folder does to ensure what it works for.

Also, when building the container, the VSCode seems like it won't print out anything in the terminal, even when you use something to print out the result like `echo`. Therefore another useful question is how to debug a Dockerfile?

> *JGM*: There's a VS command---press `F1`, then type "show container log" (or some prefix).  That should be the log you want.  I think you can also get there by click "show log" when it pops up the progress bog in the lower right-hand corner while building.

An quite unrelated obstacle is that my glasses are broken, so I have to make some effort to see anything clearly. Hope I can solve it soon.
