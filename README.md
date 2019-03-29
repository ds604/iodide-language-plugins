# iodide-language-plugins

## [Lua (Fengari)](https://fengari.io/)

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

Some examples of Lua interoperating with javascript:
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
