# About the function to get diagnostics

A critical example that might be useful for reference is the following code from `ExplicitImport.hs`:

````haskell
minimalImportsRule :: Rules ()
minimalImportsRule = define $ \MinimalImports nfp -> do
  tmr <- use TypeCheck nfp
  hsc <- use GhcSessionDeps nfp
  (imports, mbMinImports) <- liftIO $ extractMinimalImports hsc tmr
  -- ........
  
extractMinimalImports ::
  Maybe HscEnvEq ->
  Maybe TcModuleResult ->
  IO ([LImportDecl GhcRn], Maybe [LImportDecl GhcRn])
extractMinimalImports (Just hsc) (Just TcModuleResult {..}) = do
  let tcEnv = tmrTypechecked
      (_, imports, _, _) = tmrRenamed
      ParsedModule {pm_parsed_source = L loc _} = tmrParsed
      span = fromMaybe (error "expected real") $ realSpan loc

  gblElts <- readIORef (tcg_used_gres tcEnv)

  let usage = findImportUsage imports gblElts
  (_, minimalImports) <-
    initTcWithGbl (hscEnv hsc) tcEnv span $ getMinimalImports usage

  return (imports, minimalImports)
extractMinimalImports _ _ = return ([], Nothing)
````

Several notes:

- `use` only returns results when there are results.
- The `TcModuleResult` contains data like `ParsedModule`. From the documentation, `ParsedModule` is only for successfully parsed code. Therefore, it seems that we can get a `TcModuleResult` mostly only when the code has no error.

Several attempts, to figure out how to extract diagnostic messages from some kind of environment artifact:

- Search `error`/`diagnostic`/`message` under `ghcide`. Found several `Error.hs` or similar files, but their types are quite unrelated. Nevertheless, they follow a similar pattern of extracting errors from some environment, so I was wondering if there's a way to convert the `TcGblEnv` that is used in the above code to a environment that suits them.
- I tried to figure out, how the hls, or haskell plugin for vscode, report the errors it found and send them to the IDE. By searching `error`/`diagnostic` in hls, I found a rule being useful. `getParsedModuleRule`  is said to contain parsed errors from `getParsedModuleWithCommentsRule`(which, doesn't produce any diagnostics)'s comments. So I checked the `getParsedModuleRule`'s code, and it should return diagnostics. 

Currently I have found a function being mostly useful, `getDiagnostics`. It is used like this:

````haskell
do
	diag <- getDiagnostics ide
````

Where ide is an element of `IdeState`.

By checking the diagnostics it generates, it contains positions information which should become very handy, severity to distinguish between warnings and errors, but what it lacks is the tags to different type of errors, and all it has is merely messages presented in `String`. So I'm afraid that something like "cannot find a module", also counts as an error, can not be distinguished from real type check errors by collecting the diagnostics.

# About the event to trigger the plugin

By checking the language server protocol specification, I found several events being quite nice. One is `save` which is triggered when a file is saved, but I wonder it will work as interactive as the IDE does. The other one is `didChangeWatchedFiles` and I think it is more sensible.

I have tried to write a plugin based on `didChangeWatchedFiles`, but my hls ran into the unknown multi cradle error again. Either I fix it or I should rebuild my image, but there are already little time. Maybe I can write the plugin out if I find it is fun, but before the semester ends, I decide to write all the thoughts in a report.