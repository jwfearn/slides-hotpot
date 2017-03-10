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
- `:code.get_object_code` - code to binary rep
- `:code.load_binary` - code from binary rep
- `:erlang.term_to_binary` - serialization
- `:erlang.binary_to_term` - de-serialization

---

## Distributed Elixir
- Elixir code runs on the Erlang VM
- Each VM is a _node_
- Nodes can communicate over the _global_ network

---

## Distributed APIs
- PIDs are _global_ (they include node info!)
- Ergo: messages are _global_ too!
- `:global.register_name`
- `:global.whereis_name`
- IEx `--name`, `--cookie` options
- `Node.start`, `Node.set_cookie`

---

## Implementation: Hotpot
- Short for "Hot Potato"
- Include in your app, be a leader or a follower
- Leaders can `distribute` modules
- Followers receive modules

---

## Demo

---

## Hotpot Modules
- Hotpot
- LiveStart
- Leader
- Follower

---

## LiveStart
- handles loading and saving binary files
- uses `term_to_binary` & `binary_to_term`

---

## Hotpot
- manages and supervises processes

---

## Leader
- has state (GenServer): follower pids
- can `distribute` modules
- sends `:load` messages to followers
- uses `get_object_code`

---

## Follower
- receives `:load` messages
- caches modules to files for restarting
- uses `load_binary` to load module

---

## Code Walk-through

---

## Q & A

---

## Resources
Slides: https://gitpitch.com/jwfearn/slides-hotpot

Library: https://github.com/jwfearn/hotpot

Samples: https://github.com/jwfearn/hotpot_samples
