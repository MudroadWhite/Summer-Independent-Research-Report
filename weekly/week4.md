# Goals

Start to write a HLS plugin to collect Type errors.

# Accomplishments

#### 1 HLS become stable to run in Docker with VSCode.

##### Full step to reproduce:

1. Win 10, WSL 2 Debian backend, Docker, VSCode, and its Docker extension.
2. Download the `containerized` HLS, along with the `.devcontainer` folder in it.
3. Use VSCode to build the HLS copy, and `cd hls`, which will direct to the root directory of HLS.
4. `cabal build`. Notice the last few outputs of the logs, and remember the location to the HLS executable `haskell-language-server`.
5. Create a soft link to the executable under `.cabal/bin` with name `haskell-language-server`. VSCode should be already configured so that it'll use the executable in `.cabal/bin`.

##### Known Issues:

The first issue is, there might be a chance that HLS cannot run anything at all but report "Multi Cradle" error. If you edit something the error might go off, but when you save the file the error will reappear again. This will happen in Docker, when the default location is the same as the working directory: VSCode (I guess) will load the HLS through the default location, while HLS has a bug accessing places with symlinked path. The location of the working directory however is actually mounted from the host machine. Both of these issue can be referred to [here](https://github.com/haskell/haskell-language-server/issues/1971). So, I cannot automate the `cd hls` step(?). Currently, I have set the default location to the home directory.

The second issue is almost the opposite of the first issue. When the folder you're opening in VSCode is not the same as the root location of your project, then HLS will have a large chance finding out many Haskell packages in the project unreachable. This issue has been closed and its discussions can be found [here](https://github.com/haskell/haskell-language-server/issues/299). The solution is use VSCode's `File -> Open Folder` and open the project's root folder.

What if, the project I want to open is exactly `hls`? Here it becomes interesting that both of the problems does not appear! When the hls executable I built accesses the code of hls I use to build for the hls executable, for the first time, it will still download something, being shown from the logs, to fully load the documentation of the packages.

#### 2 Research of plugins.

The HLS has been already stable enough, so that I can modify and recompile it. The note in another student's document says that I should use`./cabal-hls-install hls`, but I found out it actually will download the latest version of HLS supporting the current ghc version on your machine. I believe that the only way to build the HLS is `cabal build`, and it works out successfully.

In an [older tutorial](https://github.com/pepeiborra/hls-tutorial), the author says that the example plugin in HLS should have the functionality to turn implicit imports in Haskell code into explicit imports. This however is not what it does in `Example.hs` and `Example2.hs`. Judging from the names in the code, they might not even have `provider`s that should be an important part for a HLS plugin. It turns out that the old "example" plugin has become `ExplicitImports.hs`. For both the new `Example` plugins, they try to generate fixed code lens, errors, diagnostics, symbols and completion. 

The goal of reading code should be figuring out how to collect type errors. I have noticed that there's a `IdeState` that should contain all information of diagnostics or something else. I believe that I should find out which package the `IdeState` belongs to and what contents there could be, and then how to filter out the type errors.

# Obstacles

None.