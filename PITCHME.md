## Moving Code in Elixir

#### John Fearnside
#### jfearnside@avvo.com
---
Exploring distributed Elixir
---
Can we move code between Elixir nodes?
---
## Executable Elixir
- Elixir code runs on the Erlang VM (aka "the BEAM")
- Elixir is compiled to binary form
- Elixir/Erlang has APIs for working with binary code
- Erlang APIs are callable from Elixir
---
## Binary Code APIs
- `:code.get_object_code` - code to memory format
- `:code.load_binary` - code from memory format
- `:erlang.term_to_binary` - code to file format
- `:erlang.binary_to_term` - code from file format
---
## Distributed Elixir
- Elixir code runs on the Erlang VM
- Each VM is a _node_
- Nodes can communicate with each other over the _global_ network
---
## Distributed APIs
- PIDs are _global_ (they include node info!)
- Ergo: messages are _global_ too!
- IEx `--name`, `--cookie` options
- `:global.register_name`
- `:global.whereis_name`
---
## Implementation: Hotpot
- Short for "Hot Potato"
- Include in your app, be a leader or a follower
- Leaders can `distribute` modules
- Followers receive module
---
## Demo
---
## Hotpot Modules
- LiveStart - handles loading and saving binary files
- Hotpot - manages processes
- Leader
  - has state (GenServer): follower pids
  - can `distribute` modules
  - sends `:load` messages to followers
- Follower
  - receives `:load` messages
  - loads received modules
  - caches modules to files
---
## Code Walk-through
---
## Q & A
---
## Resources
Slides: https://gitpitch.com/jwfearn/slides-hotpot

Library: https://github.com/jwfearn/hotpot

Samples: https://github.com/jwfearn/hotpot_samples

<!--
# TODO

```bash
myip () { ipconfig getifaddr en0; }
iex --name leader@$(myip) --cookie cookie -S mix
```



``` Node.connect :"worker@10.3.17.24"```

get the remote pid:
```
john_pid = :global.whereis_name(:johnny)
```

create the anonymous function that will load the code
```
fun = fn(mod, path, bin) -> :code.load_binary(mod, path, bin) end
```

get the object code of the module
```{mod, bin, path} = :code.get_object_code(Foo)```

Send a message to the pid to fun the anonymous function
```send(john_pid, {:r, fun, [mod, path, bin]})```

I bet we could do the same thing with an rpc call

```
iex --name master@10.3.17.68 --cookie abc123 -S mix
```
 
```
:global.register_name(:skyler, self())
```

```
receive do {:r, fun, args} -> apply(fun, args) end
```

http://erlang.org/documentation/doc-5.5.1/lib/kernel-2.11.1/doc/html/erl_prim_loader.html



Process APIs:
=================
Kernel.{spawn, spawn_link, spawn_monitor, self, send}
Process.*
Registry.*
-->
