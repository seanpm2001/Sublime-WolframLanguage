# Sublime-WolframLanguage

Sublime Text 3 support for the [Wolfram Language](https://en.wikipedia.org/wiki/Wolfram_Language), the language used in Mathematica.

- Syntax highlighting.
- Auto completion for built-in functions.
- Requires Sublime Text 3 Build 3103 or later. This package uses the new syntax format `sublime-syntax`.


## Setup

Sublime-WolframLanguage depends on the CodeParser, CodeInspector, CodeFormatter, and LSPServer paclets. Make sure that the paclets can be found on your system:
```
In[1]:= Needs["CodeParser`"]
      Needs["CodeInspector`"]
      Needs["CodeFormatter`"]
      Needs["LSPServer`"]
```

[CodeParser on github.com](https://github.com/xxx)
[CodeInspector on github.com](https://github.com/xxx)
[CodeFormatter on github.com](https://github.com/xxx)
[CodeParser on github.com](https://github.com/xxx)

Install LSPServer and dependencies from the CodeTools paclet server:
```
In[1]:= PacletUpdate["CodeParser", "Site" -> "XXX", "UpdateSites" -> True]
      PacletUpdate["CodeInspector", "Site" -> "XXX", "UpdateSites" -> True]
      PacletUpdate["CodeFormatter", "Site" -> "XXX", "UpdateSites" -> True]
      PacletUpdate["LSPServer", "Site" -> "XXX", "UpdateSites" -> True]

Out[1]= PacletObject[CodeParser, 1.0, <>]
Out[2]= PacletObject[CodeInspector, 1.0, <>]
Out[3]= PacletObject[CodeFormatter, 1.0, <>]
Out[4]= PacletObject[LSPServer, 1.0, <>]
```

If you haven't already, [install Package Control](https://packagecontrol.io/installation), then select `WolframLanguage` from the `Package Control: Install Package` dropdown list in the Command Palette.

### Settings

Package Settings > Wolfram Language > Settings

```
{
  "lsp_server" : {
    "command":
    [
        "<<Path to WolframKernel>>",
        "-noinit",
        "-noprompt",
        "-nopaclet",
        "-run",
        "Needs[\"LSPServer`\"];LSPServer`StartServer[]"
    ]
  }
}

```

Restart Sublime

You should now have syntax highlighting and linting of Wolfram `.m` and `.wl` files working.

Test this by typing this into a new `.m` file and saving it:
```
Which[a, b, a, b]
```

You should see warnings about duplicate clauses.


#### Command arguments:

`<<Path to WolframKernel>>`

This is the path to your `WolframKernel` executable.

If you installed Mathematica in a default location, then this is something like:
```
/Applications/Mathematica.app/Contents/MacOS/WolframKernel
```

``"Needs[\"LSPServer`\"];LSPServer`StartServer[]"``

This is the command that the kernel runs to start the server.


## Building

Sublime-WolframLanguage uses a Wolfram Language kernel to build a `.sublime-package` file.

Sublime-WolframLanguage uses CMake to generate build scripts.

Here is an example transcript using the default make generator to build Sublime-WolframLanguage:
```
cd sublime-wolframlanguage
mkdir build
cd build
cmake ..
cmake --build . --target package
```

The result is a directory named `package` that contains the WolframLanguage Sublime package.

Specify `INSTALLATION_DIRECTORY` if you have Mathematica installed in a non-default location:
```
cmake -DINSTALLATION_DIRECTORY=/Applications/Mathematica111.app/Contents/ ..
cmake --build . --target package
```


## Troubleshooting

### Windows

It is recommended to specify `wolfram.exe` instead of `WolframKernel.exe`.

`WolframKernel.exe` opens a new window while it is running. But `wolfram.exe` runs inside the window that started it.

You may need to double-up quotations marks in the command:

``"Needs[\"\"LSPServer`\"\"];LSPServer`StartServer[]"``
