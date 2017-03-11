# visual:clojure

## How, what and why?

* Create a nice developer-environment for ClojureScript, that is not dependent on me using Emacs :)
* Most of the existing extensions that I found had variable/good support for Clojure, but none existed for ClojureScript!

This extension is mostly geared towards creating a nice ClojureScript developer experience, but will actively try to support
both clj and cljs as much as possible.

![Features](/assets/howto/features.gif)

![underline error](/assets/howto/error.png)  

## Current features
* Intellisense
* Underlining compile-time errors
* Go to / Peek at definition
* View docstrings on hover
* View function signatures on hover
* Interactive REPL From visual code 
  * Compile files
  * Evaluate forms
* Supports all clojure filetypes, clj, cljc and cljs.
 * cljc evaluted using clj-REPL session

## Future stuff
* Better error-support for cljs
* Add more / useful snippets
* Start REPL directly from code
* Auto-connect to existing repl using 'repl-port'-file
* Add signature-provider for better function signature assistance
* Visualize scoped vars
* Debugger(?)
* Other (please add suggestions as issues)

## Getting started
### Dependencies
* Uses nrepl for evaluation / communication
* Uses cider-nrepl for added nrepl functionality
* Uses piggieback and nrepl middleware cemerick.piggieback/wrap-cljs-repl for clojurescript session support

### ClojureScript
Make sure the dependencies is either in your project.clj or profiles.clj.
profiles.clj is located in your home/.lein folder, for more information [look here.](https://github.com/technomancy/leiningen/blob/master/doc/PROFILES.md)

A sample profiles.clj could look like this:
```
{:user {:plugins [[cider/cider-nrepl "0.14.0"]]
        :dependencies [[com.cemerick/piggieback "0.2.1"]
                       [org.clojure/tools.nrepl "0.2.10"]]
        :repl-options {:nrepl-middleware [cemerick.piggieback/wrap-cljs-repl]}}}
```

### Clojure
Make sure the dependencies is either in your project.clj or profiles.clj:

A sample profiles.clj could look like this:
```
{:user {:plugins [[cider/cider-nrepl "0.14.0"]]
        :dependencies [[org.clojure/tools.nrepl "0.2.10"]]}
```

### Initiating a CLJ/CLJS REPL
After installing the extension, and adding the dependencies mentioned above, 
you should now have access to a set of commands if/when opening a clojure file.

First initiate a repl, the easiest way to do this is opening a terminal and writing ```lein repl```

![Lein REPL](/assets/howto/lein_repl.gif)

Notice the port **49864** generated by  REPL at nrepl://localhost:49864
This is what is used to connect to the nREPL from visual code.

If you want to start a ClojureScript REPL-session you can start this from the existing clojure-REPL that we just created.  
Using piggieback we can initiate a cljs-repl using e.g. rhino:  

Run the following command in the REPL to start a cljs-session with rhino: ```(cemerick.piggieback/cljs-repl (cljs.repl.rhino/repl-env))```  

![ClojureScript REPL](/assets/howto/cljs_repl.gif)

However most people use figwheel when developing in ClojureScript. It is just as easy to connect to the cljs-repl provided by figwheel!  
You need to start the initial repl like above with ```lein repl``` and initiate figwheel from there, not directly from the cmd, 
this is to preserve the nREPL port that we use from vscode to connect to the REPL..  

**To initiate a figwheel-repl you need the figwheel-sidecar dependency -> [figwheel-sidecar "0.5.8"] as well correct cljs classpaths**
read more about this [here](https://github.com/bhauman/lein-figwheel/wiki/Using-the-Figwheel-REPL-within-NRepl)  

If you have created a figwheel-project from a template (using e.g. lein new), you should be good to go as long as you start the repl in the projects folder.  

So instead of running ```lein figwheel dev``` you will need to run ```lein repl``` and then start the figwheel REPL using    
```(start-figwheel!)```

I like to keep a start.clj file in my projects for this, and use (load-file "start.clj") to start:
sample start.clj  
```
 (use 'figwheel-sidecar.repl-api)
 (start-figwheel!)
 (cljs-repl)
```

But you can ofcourse also do it manually in the nREPL:  
**NOTE:** You need to connect to the browser to activate figwheels the cljs-repl session.  
![Figwheel REPL](/assets/howto/figwheel.gif)


### Connect to the active REPL from vscode

Now that we have a working clj/cljs REPL running, we can connect from vscode using the extension!  
Make sure that the REPL is up and running (it has a user-namespace prompt ```user=>```)  
To connect either use the shortcut:  ```Alt + C``` (default), or the command ```VisualClojure: Connect to an existing nREPL session```  
When the input-field is displayed enter the correct REPL **port** and you should see a notification in the lower-left indicating the sessions found.  

![Connect from VSCode](/assets/howto/connect.gif)

There are 3 different states that the extensions connection can be in:  
 ![clj connection](/assets/howto/status_clj.png)  
 ![cljs connection](/assets/howto/status_cljs.png)  
 ![no connection](/assets/howto/status_not_connected.png)  


That's it!, you are now connected and can start writing clojure :)
