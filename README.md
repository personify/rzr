# rzr
thin build agent

rzr provides:
  * a simple convention for build steps ("blades)
  * a DSL for build automation: @build, @writeTo, @agent

```coffeescript
@build  
  agent: ‘Archivist’
  use: ’gitomic’, ‘shake’
  lookFor: ‘*’
  pipeline: 
    1: apetail
    2: uglify
    3: @writeTo @agent.fs.bin
    4: @agent.commit() #!
    5: @agent.deploy()  #!
```

in the above example, rzr simply walks from the root of your application (the cwd where $ rzr build is run) 
and executes each step across each file 

how it works:

rzr provides @agent.fs - an instance of the Filesystem Monad.  The Filesystem Monad creates
an in-memory version of the file system (of any type) and allows you to modify the structure 
and contents, and then optionally commit the changes somewhere (back to the file system, 
to a database)