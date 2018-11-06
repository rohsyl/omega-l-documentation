# Getting started and configure - `omega-cli`

This guide will show you how to start using and configure `omega-cli` command.

## Location

`omega-cli` can be found inside the OmegaCMS directory into `tools/omega-cli`.


## Configure

1. Edit the `omega-cli.exe.config` file
2. Set the value of `omegaRoot` to the directory of omega
    ```
    <add key="omegaRoot" value="D:\git\omegav3"/>
    ```
3. Test if it's okay
    ```
    omega-cli omega:valid-dir
    ```
    
    It will return this if success :
    ```
    SUCCESS : D:\git\omegav3 is valid.
    ```

## Run `omega-cli`

### On Windows

To run `omega-cli` on Windows, just start a Command prompt and run the program.

### On Linux

Use `mono` to run .NET program on linux system.

TODO

## Use it

When `omega-cli` is configured and ready to run, you can see the available command by typing :
```
omega-cli help
```

Output :
```
List of all 5 commands
001: help => Displays the list of commands, or the help file for a specified command
002: alias => Display the list of all aliases for a given command or show if a command is an alias of another command
003: echo => Echoes a message to the active Output
004: exit => Exit the program
005: plugin:create => Create a plugin for OmegaCMS
```

and you can also see how command work by typing :
```
omega-cli help [command]
```


**Enjoy !**