# Goals

Completed setting up the HLS in a container! But I should try to make it as stable as possible.

> *JGM*: Make sure to push your work to the `containerized` branch at some point, so I can experiment with it too!

Also, the next tasks are up. I have to start writing plugins for the hls.

# Accomplishments

This will be a place for me to write out, the complete step to build the hls container:

1. Currently, I start with the Dockerfile script that I found on the internet.
2. This could be a strange phenomenon: I have already written and asked the machine to install `ghcide`. It seems that after I have built the image, I still have to run the `cabal install ghcide` in the hls folder. Could it be that I haven't added the cabal folder to the `$PATH`? I might try this out.
   > *JGM*: This could be that by default Cabal installs libraries locally.  You might need to update your initial `ghcide` installation command to be `--global`?  Or even just figure out how to run the installation in the `hls` folder.
4. Then use `cabal build` under the folder to build the project.
5. Haskell extension for VSCode should be installed into the container.
6. Some systems requires using the hls wrapper rather than hls executable. But in my case, that is Debian, what we need is the executable. While building the hls, in the last lines of the output cabal might teel you where it puts the executable in. In my case this is `/workspaces/haskell-language-server/dist-newstyle/build/x86_64-linux/ghc-8.10.4/haskell-language-server-1.2.0.0/x/haskell-language-server/build/haskell-language-server/haskell-language-server`
7. I can copy the executable to some place for a shorter path. I then fill the path to the executable, rather than the path to the folder that contains the executable, into VSCode's server executable settings, for both the remote container and the workspace.

> *JGM*: Can we automate the entire initial build and installation as part of the initial container setup?  Then we could specify our new HLS executable as part of the container configuration as well, I imagine.

# Obstacles

As a last examination, I need to write a simple test file to test the HLS. First of all it is working because the file is not just giving syntax highlights, but also generated mistakes as well. Second the error it generated is strange.

If I stuff a test file into the folder for the executable, and directly run the hls, then it will find any `.hs` file to test out, and from that I see that the hls should be fine. But if I'm using it in VSCode, it will report that "Multi Cradle: No prefixes matched". I wonder it is something about the `PATH` again.

> *JGM*: There are a couple of bug reports in the main HLS repository of people encountering this issue.
