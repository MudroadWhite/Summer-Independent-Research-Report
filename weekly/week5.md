# Goals

Write an HLS plugn that collect type errors.

# Accomplishments

#### Understood the model of a plugin. 

I can write a plugin that reacts whenever some code has been hovered, which is part of my plan for this week.

Some notes:

##### Frontend related

When will code action be triggered? what is code lens? --> refer LSP specification. A event being triggered will be connected with a provider defined in the plugin through the "main function" of the plugin, or the descriptor.

Where it is implemented? What's the API? --> https://hackage.haskell.org/package/lsp-types-1.2.0.0/docs/Language-LSP-Types.html SMethod parts

##### Backend related

What to check: Ghc API, GhcIDE API, Shake documentation

# Obstacles

#### Find the correct API to collect the type errors.

There is a line `tmr <- use TypeCheck normalizedFilepath` in the tutorial, and I tried to use this to get the typecheck errors and failed. By asking someone on internet I realized that when the code produces typecheck errors, the typecheck artifact will only return you the old artifact with no errors. And that person suggests me to look into GHC API, and there should be a method to extract all active diagnostics for code, which should contain all typecheck errors.

The most related page I found on GHC's documentation is [this](https://gitlab.haskell.org/ghc/ghc/-/wikis/Errors-as-(structured)-values#the-envelope-contents-ie-the-diagnostics). I still have to dive further in.