# MTA Build Tool

Straight forward build tool for nice and easy resources development.  

This tool is designed to support you through the process of creating resources. It gives you the ability to skip some manual activities by automating the process of resource build and development.

[1. Feature list](#feature-list)  
[2. Creating a resource](#creating-a-resource)  
[3. Assets](#assets)  
[4. Bundle and compile](#bundle-and-compile)  
[5. Auto Meta.xml](#auto-metaxml)  
‎ ‎ ‎ ‎ ‎[5.1. Scripting type inference](#script-type-inference)

### Feature list

🟢 Built in server support  
🟢 Auto generated meta.xml  
🟡 Resource compile and bundling  
🟡 Secure element data  
🟡 TSToLua support

## Creating a resource

To create a resource with this tool it's as easy as creating a folder inside the `src` directory and start writing `.lua` files into it. You won't need to bother with `meta.xml` whatsoever.
The tool detects any top-level folder not surrounded by brackets which contains one or more `.lua` files as a resource.

**Examples:**

✔️ `src/resource/script.lua`  
✔️ `src/[organizer]/resource/script.lua`  
❌ `src/organizer/resource/script.lua`  
❌ `src/resource`

The tool will mirror your `src` directory into the `server/mods/deathmatch/resources` directory and will automatically make any necessary changes and creation of the `meta.xml`
during the development process.

## Assets

All files you normally would include in the meta with the `<file>` tag should be placed inside `assets` directory inside you resource root directory.
This is a special folder MTA Build Tool will look for. All files inside assets folder will be included in the final `meta.xml`.  

By default all files inside assets directory will be download automatically by the client. If you need to include files that are not going to be downloaded by the client - 
as you would do with property `download="false"` - then you should put it in another directory.

## Bundle and compile

MTA Build Tool comes with the ability to compile and zip your resources while building to production.  

Currently, it supports three types of compilation: `cached only`, `all` and `none`. By default, all scripts are going to be compiled to production. You can choose to compile cached only scripts which means client scripts with `cache="true"` property or disable compilation altogether.

Under the hood it uses the [MTA SA Lua Compiler](https://luac.mtasa.com) with obfuscation on level 3 to compile the scripts.

Bundling is the ability to ship your resource to .zip file and it's optional, but it will happen by default.

## Auto Meta.xml

### Script type inference

As to properly generate the meta.xml the tool searches for pieces of code inside a script that say something about its type. For example, if a script uses the `localPlayer`
variable or `getLocalPlayer` method the tool knows there is a chance for this script to be a client script.  

Among other checks, you can manually define a script type by prefixing or sufixing its name with one of `-s`, `_s`, `-c`, `_c`.

**Examples:**

`script-s.lua` - this will generate a `type="server"` in meta.xml  
`script-c.lua` - this will generate a `type="client"` in meta.xml  

If the naming convention could not be determined then it falls back to code analyzing.


