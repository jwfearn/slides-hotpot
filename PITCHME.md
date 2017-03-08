## Moving Code in Elixir

#### John Fearnside
#### jfearnside@avvo.com
---
Can we move executable code between Elixir nodes?
---
## Distributed Features Overview
- [global registry](http://erlang.org/doc/man/global.html)
- PIDs
---
## Erlang VMs Are Nodes
---
## Demo
---
## Q & A


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
