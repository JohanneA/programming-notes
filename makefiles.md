# [Understanding make](https://www.cprogramming.com/tutorial/makefiles.html)
##### From cprogramming.com by Alex Allain

# [Makefile Tutorial](http://www.sis.pitt.edu/mbsclass/tutorial/advanced/makefile/)
##### From sis.pitt.edu by S. M. Garver

Make is used to associate names with a series of commands to execute. When used on the command line, make will read the Makefile in the current directory (if there is one).
 * Running `make` will run the default case, running `make <insert word>` will run the commands associated with `<insert word>`

Lines starting with `#` are ignored

Some basic components are *macros* and *target definitions*
 * **MACROS** Act like constants `CC=gcc`, to get the value in a target definition, put it in between `$()`. `$(CC) a_source_file.c` becomes `gcc a_source_file.c`
 A macro can be specified in terms of other macros `COMP = $(CC) $(OPT)`
 Macros must be capital letters
 * **TARGET DEFINITIONS** These are what makes make files useful. They covert a command line input into a series of actions. They have three components: the name, their dependencies and then the actions associated with the target.
 ```
 target: [dependencies]
  <command>
  <command2>
  ...
  ```
  **OPS:** Each command must be proceeded by a tab. A number of spaces is not enough

  Dependencies are either other targets or files themselves. Will only execute target if any of the files have changed since the last time the command was executed. Dependencies can also be left out.

If a command line is too long `\` can be added at the end of a line and the command can be continued on the next line. Add `@` in front of a command to indicate that you don't want it to be shown on screen as make executes.


When targets execute they return a status based on whether or not it was successful. If you have other targets that depend on another target, they might not build successfully if one of the dependencies fail. To ignore the status of a make target include a `-` in front of the commands whose status should be ignored.

##### The Default Target
If you type in `make` it will run the first target in your make file.

___
## More advanced make files

If you want a different name for your makefiles other than `Makefile` or `makefile` you can use the `-f` when running make.
```
make -f <filename>.<whatever you want>
```

#### Phony targets
A phony target is one that is not really the name of a file, just the name for a recipe to execute. Use to avoid conflict with a file of the same name and to improve performance. Classic example is `make clean`
Good practice is to declare the target phony using `.PHONY: clean` so make will know that it shouldn't look for a file called clean.

### Suffix rule
Use `.<suffix1>.<suffix2>` to create a general rule that says "for all <filename>.<suffix2> there must be a <filename>.<suffix1> to build" so for `main.o` there must be a `main.c` in the directory.

### Special characters
| Character | description |
|----|----|
|$?| List of dependencies changed more recently than the current target|
|$^| Names of dependencies with spaces in between |
|$@ | Name of current target|
|$<| Name of current dependency|
|$*|Name of current dependency without extention|

[See more special characters here](https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html)
