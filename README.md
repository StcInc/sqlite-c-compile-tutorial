# Compiling C/C++ app that works with SQLite on windows with VC/MinGW/TDM-GCC

This process can be a little bit tedious, especially for beginners, as most of the existing instructions are broken or outdated.

Here there are couple of methods for compiling a simple test application, code for which is contained in a single source file, borrowed from [here](https://dcravey.wordpress.com/2011/03/21/using-sqlite-in-a-visual-c-application/)

Suggested set of methods contains using command line tools for both [VC++ 2015](https://msdn.microsoft.com/en-us/library/60k1461a.aspx), [MinGW](http://www.mingw.org/) (or [TDM-GCC](http://tdm-gcc.tdragon.net/) - improved version of MinGW) and GUI building approach for Visual Studio


# Before you start
1. Clone the repository (or copy `main.cpp` to your machine)
2. Download and install either
    - Visual Studio 2015 community (with command line tools) or newer (visual studio 2017 community were used for this tutorial)
    - [MinGW compiler](http://www.mingw.org/) or [TDM-GCC compiler](http://tdm-gcc.tdragon.net/) and add to your machine's [PATH](https://superuser.com/a/949577) **system variable**
3. Download SQLite's [Precompiled Binaries for Windows](http://www.sqlite.org/download.html) and extract them from archives.
You will need
    - **One of** `sqlite-dll-win32-x86-3230100.zip` **or** `sqlite-dll-win64-x64-3230100.zip` (depending on your platform)- for `sqlit3.dll` and `sqlite3.def` files
    - `sqlite-tools-win32-x86-3230100.zip` - for `.sqlite3.h` file

    Suggested files are relevant for the time of tutorial's creation, so`-3230100` can be replaced with the newer versions, this should not affect tutorial's relevance
4. Proceed with either section
    - `For MS VC` - you can do either
        - `Console build with VS2015 command prompt`
        - `GUI build in Visual Studio`

        In both cases you will need Visual Studio's command line tools to produce `sqlite3.lib` file out of `sqlite3.def` file. Though the rest of the app's build process can be completed in different ways
    - `For MinGW/TDM-GCC`- command prompt only, though if you are using [Code blocks](http://www.codeblocks.org/) or [Dev-C++](https://sourceforge.net/projects/orwelldevcpp/) IDEs you can follow the tutorial with only differences in the actual compiling setup process

# For Microsoft's  Visual Studio compiler:
1. Open VS command prompt
 (e.g. search in windows menu for `command prompt` -  something like `VS2015 X64 Native Tools Command Prompt` - open this, maybe `x86` if you go for x86 build)
Navigate to where you've extracted `sqlite-dll-win64-x64-3230100.zip` (or `sqlite-dll-win64-x86-3230100.zip`) files from  within command prompt, e.g.
    ```
    D:
    cd Downloads/sqlite-dll-win64-x64-3230100
    ```

3. Run (to produce `sqlite3.lib` file from `sqlite3.def` file, supplied with `sqlite3.dll`)
- for 32-bit DLL (x86) for SQLite (using `VS2015 X86 Native Tools Command Prompt`):
    ```
    LIB /MACHINE:X86 /DEF:sqlite3.def
    ```

- for 64-bit DLL (x64) for SQLite (using `VS2015 X64 Native Tools Command Prompt`):
    ```
    LIB /MACHINE:X64 /DEF:sqlite3.def
    ```
if everything is ok, you will see that `sqlite3.lib` file has appeared in the folder

Don't close the command prompt yet, if you are going to proceed with `Console build with VS2015 command prompt`

4. Copy `sqlite3.lib`, `sqlite3.dll` from `sqlite-dll-win64-x64-3230100`, and `sqlite3.h` from `sqlite-tools-win32-x86-3230100` files to the project folder (you can use your file explorer to simplify the process; `sqlite-dll-win64-x64-3230100` `sqlite-tools-win32-x86-3230100` may vary depending on your platform and version downloaded from http://www.sqlite.org/download.html)


### Console build with VS2015 command prompt
5. Navigate within VS2015 command prompt (opened earlier) to the project's folder, e. g.
    ```
    D:
    cd cpp/sqlite_win
    ```
6. Compile app using following command prompt line
    ```
    cl /EHsc main.cpp sqlite3.lib /link
    ```



### GUI build in Visual Studio
5. Go to
    ```
    Project > `Your project` Properties > Linker > Input
    ```
6. Add to `Additional dependencies` path to `.lib` file, e. g.
    ```
    D:\vsqlite\sqlite3.lib
    ```
7. Add `sqlite3.h` file to the project, rebuild the solution
8. Copy `sqlite3.dll` file to the folder where app's executable is placed (e.g. `Debug` folder in the root of the solution, or in project's folder)
8. Run executable (either from console or using Run Debug in the IDE)



# For MinGW/TDM-GCC:

1. Run in command prompt with MinGW/TDM accessible, navigate to the folder with extracted `sqlite-dll-win64-x64-3230100` (or other platforms/versions, depending on your case)
    ```
    D:
    cd Downloads/sqlite-dll-win64-x64-3230100
    ```
2. Run (to produce `sqlite3.lib` from `sqlite3.def`)
    ```
    dlltool --output-lib sqlite3.lib --dllname sqlite3.dll --input-def sqlite3.def
    ```
If everything is ok, file called `sqlite3.lib` will appear in the folder

3. Copy `sqlite3.lib` and `sqlite3.dll` from  `sqlite-dll-win64-x64-3230100` folder, `sqlite3.h` from `sqlite-tools-win32-x86-3230100` folder to your project's folder (you can simplify this process by using your file explorer)
5. Navigate within command prompt to your pronject's folder e.g.
    ```
    cd cpp/sqlite_win
    ```

4. Run (to compile and link, execute the app)
    ```
    g++ -o main main.cpp -L. -lsqlite3
    ./main
    ```



# In all the shown cases, the output of the application should be like
```
Opening MyDb.db ...
Opened MyDb.db.

Creating MyTable ...
Created MyTable.

Inserting a value into MyTable ...
Inserted a value into MyTable.

Retrieving values in MyTable ...
id           value
~~~~~~~~~~~~ ~~~~~~~~~~~~
1            A Value
Closing MyDb.db ...
Closed MyDb.db

Please press any key to exit the program ...
```

If you have any issues, look at the examples in either `mingw_sqlite`, `vc_command_prompt_sqlite` or `vs_sqlite/vsqlite` folder


# Thanks to
https://dcravey.wordpress.com/2011/03/21/using-sqlite-in-a-visual-c-application/

http://source.online.free.fr/Windows_HowToCompileSQLite

http://www.mingw.org/wiki/createimportlibraries

https://msdn.microsoft.com/en-us/library/ms235639.aspx
