# How To Package the Game
https://piazza.com/class/lq5ikpoifn5wb/post/180

Edit by Suzuran: getting a "proper" executable game requires a lot of packaging work that we won't be going over. The following is a workaround/hack that will work, but may cause some antivirus software to complain (it is also Windows specific):

  
1. Open "ext/project_path.hpp.in" and change the value of “PROJECT_SOURCE_DIR” from “@CMAKE_CURRENT_SOURCE_DIR@/” to “./”  
  
This will ensure that you won't encounter pathing issues for loading assets.  
  
2. Build and compile the game  
  
3. Copy the executable file from where it is built (/out/x64-Release/...) to a new folder for packaging the game.  
  
4. Copy the data and shader folders to the same folder  
  
5. Copy over any project-specific DLLs from where your game was build (/out/x64-Release/...)  
  
6. Some additional DLLs could be necessary on someone's device who doesn't usually play games. For example, running the game would require the Visual C++ runtime redistributable so on a clean install of Windows it won't run. These are the msvcpxxxd.dll, vcruntimexxxd.dll, vcruntimexxx_1d.dll, and the ucrtbased.dll if it's built on debug mode, where the "xxx" is the runtime version. You can find these from downloading Ascent from the [past games course website](https://www.students.cs.ubc.ca/~cs-427/games/) if that is easier. FYI, this is bad practice and usually real games will prompt you to install the runtime redistributable if it detects that it is not installed instead.

  
7. Double check that your directory contains:

- The data folder
- The shaders folder
- The executable file
- All of the dlls. The SDL2, SDL2_mixer, and GLFW dlls will be necessary unless they were removed from the template. If Freetype is used, a dll for that is necessary as well. 
- If using DLLs from Ascent to have it run on a clean install of Windows, then msvcp140d.dll ucrtbased, vcruntime140_1d, vcruntime140d, may be necessary.