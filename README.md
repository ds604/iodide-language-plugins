# iodide-language-plugins

- [Lua](#lua)
- [Ruby](#ruby)
- [Assemplyscript](#assemblyscript)

## <a name="lua">[Lua (Fengari)](https://fengari.io/)</a>

Add [Lua language](https://fengari.io/) support to [Iodide](https://alpha.iodide.io/). Demo notebook link: https://alpha.iodide.io/notebooks/1416/.

How to use: paste this into an Iodide notebook.
```
%% plugin
{
  "languageId": "lua",
  "displayName": "lua",
  "codeMirrorMode": "lua",
  "keybinding": "a",
  "url": "https://cdn.jsdelivr.net/npm/fengari-web@0.1.4/dist/fengari-web.js",
  "module": "fengari",
  "evaluator": "evalLua",
  "pluginType": "language"
}

%% js
fengari.evalLua = function(code){ fengari.load(code)() }
```

Sample Lua usage (including interoperating with Javascript):
```
%%lua
-- print goes to browser console
for i=1,5 do
	print(i)
end

%%lua
-- run javascript console.log from lua
js.global.console:log("hello world")

%%lua
-- use javascript array in lua
t = {"foo", "bar", "baz"}
for k,v in pairs(t) do print(k,v) end

for f in js.of(js.global:Array(10,20,30)) do
	print(f)
end

%%lua
-- create variable to share with to js
local js = require "js"
local window = js.global
local document = window.document

window.foobar = 42

%%js
// use lua variable
window.foobar
```

## <a name="ruby">[Ruby(Opal)](https://opalrb.com/)</a>

Add [Ruby language](https://opalrb.com/) support to [Iodide](https://alpha.iodide.io/). Demo notebook link: https://alpha.iodide.io/notebooks/1453/.

How to use: paste this into an Iodide notebook.
```
%% fetch
js: https://cdn.opalrb.com/opal/current/opal.js
js: https://cdn.opalrb.com/opal/current/opal-parser.js

%%js
Opal.load('opal')
Opal.load('opal-parser')

%% plugin
{
  "languageId": "ruby",
  "displayName": "ruby",
  "codeMirrorMode": "ruby",
  "keybinding": "r",
  "url": "https://cdn.opalrb.com/opal/current/opal.js",
  "module": "Opal",
  "evaluator": "eval",
  "pluginType": "language"
}
```

Sample Ruby usage:
```
%% ruby
[1,2,3,4].map{|d| d*2}

%% ruby
(1..10).map{|i| i*2}

%% ruby
(1..100).map{|n|
  if (n%15).zero?
    'FizzBuzz'
  elsif (n%3).zero?
    'Fizz'
  elsif (n%5).zero?
    'Buzz'
  else
    n.to_s
  end
}
```

## <a name="assemblyscript">[Assemblyscript](https://github.com/AssemblyScript/assemblyscript)</a>

Add [Assemblyscript](https://github.com/AssemblyScript/assemblyscript) support to [Iodide](https://alpha.iodide.io/). Demo notebook link: https://alpha.iodide.io/notebooks/1460/.

How to use: paste this into an Iodide notebook.
```
%% fetch
js: https://bundle.run/ts-node@8.0.3
%% fetch
js: https://cdn.jsdelivr.net/npm/binaryen@73.0.0/index.min.js
%%
js: https://raw.githubusercontent.com/AssemblyScript/assemblyscript/master/dist/assemblyscript.js
%%
js:  https://raw.githubusercontent.com/AssemblyScript/assemblyscript/master/dist/asc.js
%% js
window.exports = ''
window.asmScript = {}

asmScript.eval = function(assemblyscriptString){
  const { binary, text, stdout, stderr } = asc.compileString(assemblyscriptString,{})
  var compiled = new WebAssembly.Module(binary);
  var instance = new WebAssembly.Instance(compiled, {});
  window.exports = instance.exports;
}

%% plugin
{
  "languageId": "assemblyscript",
  "displayName": "assemblyscript",
  "codeMirrorMode": "javascript",
  "keybinding": "a",
  "url": "https://raw.githubusercontent.com/AssemblyScript/assemblyscript/master/dist/asc.js",
  "module": "asmScript",
  "evaluator": "eval",
  "pluginType": "language"
}
```

Sample Assemblyscript usage: 
```
%% assemblyscript
export function add(a: i32, b: i32): i32 {
  return a + b;
}

%% js
exports.add(4,5)
```
