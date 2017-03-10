## Moving Code in Elixir

#### John Fearnside
#### jfearnside@avvo.com
---
How can we move executable code between Elixir nodes?
---
## Executable Elixir
- Elixir code runs on the Erlang virtual machine (aka, the BEAM)
- Elixir is compiled to binary form
- Elixir/Erlang has APIs for working with binary code
- Erlang APIs callable from Elixir
---
## Binary Code APIs
- `:code.get_object_code` - get binary from module
- `:code.load_binary` - get module from binary
- `:erlang.term_to_binary` - difference between term and module?
- `:erlang.binary_to_term`
---
## Distributed Elixir
- Elixir code runs on the Erlang VM
- Each VM is a _node_
- Nodes can communicate with each other over the _global_ network
---
## Distributed APIs
- PIDs are _global_ (they include node info!)
- Messages are _global_ too!
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
- Leader - sends `:load` messages, has state: follower pids
- Follower - receives `:load` messages
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
