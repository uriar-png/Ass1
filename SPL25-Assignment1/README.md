# DJ Session Management System - README

## Project Overview
This is a C++ project that implements a DJ Session Management system with support for audio track management, playlists, caching, and mixing operations.

## Project Structure
```
SPL25-Assignment1/
├── bin/                    # Compiled executables and configuration files
│   └── dj_config.txt      # Configuration file for DJ settings
├── include/               # Header files (.h)
├── src/                   # Source files (.cpp)
├── .devcontainer/         # Development container configuration
├── Makefile              # Build system configuration
└── README.md             # This file
```

## Prerequisites

### Option 1: Local Installation on a Virtual Machine (Recommended)
Run Ubuntu Linux inside a virtual machine (VM) on your own computer. This gives you the same Linux tools used for grading (g++, valgrind, gdb), whatever operating system you normally use.

1. Install virtualization software:
   - **Windows / Linux / Intel Mac**: [VirtualBox](https://www.virtualbox.org/)
   - **Apple Silicon Mac (M1/M2/M3/...)**: [UTM](https://mac.getutm.app/) (use the ARM64 version of Ubuntu)
2. Download [Ubuntu Desktop](https://ubuntu.com/download/desktop) (an LTS version) and create a new VM from it. At least 4 GB of RAM and 25 GB of disk space are recommended.
3. Inside the VM, open a terminal and install the tools (a fresh Ubuntu doesn't include `make` yet, so use `apt-get` directly):
   ```bash
   sudo apt-get update
   sudo apt-get install -y build-essential g++ valgrind gdb
   ```
   This installs:
   - A C++ compiler (g++)
   - Make build tool
   - valgrind (for memory leak detection)
   - gdb (for debugging)
4. Optionally, install [VS Code](https://code.visualstudio.com/) inside the VM to edit and build the project.

### Option 2: Using Dev Container
This project includes a `.devcontainer` configuration that provides a complete development environment with all necessary tools pre-installed. If you're using Visual Studio Code with Docker and the Dev Containers extension, simply open the project and it will set everything up for you.

### Option 3: University Lab Computers
All required tools are pre-installed on the lab computers at BGU. You can use those directly without any setup.

## Getting Started

### 1. Getting the Project Files

The project files are distributed as a **zip archive on the course Moodle page**. You don't need Git to get started.

1. Download the assignment zip file from Moodle
2. Extract it to a folder (e.g. `SPL25-Assignment1/`) - if you're using a virtual machine, do this inside the VM
3. Open VS Code and select **File → Open Folder...**, then choose the extracted folder
4. Only if you're using the dev container (Option 2): click **"Reopen in Container"** when VS Code prompts you

**Note**: The `.devcontainer/` folder starts with a dot, so your file manager may hide it. It is still there after extracting - make sure you open the folder that contains it, not a folder above or below it.

Tip: Using Git to track your own changes is still a good idea (see the submission instructions), but it is optional.

### 2. Understanding the Build System
This project uses **Make**, a build automation tool that compiles your code. The `Makefile` contains instructions for how to build the project.

Think of Make as a recipe book for building your program. Instead of manually compiling each file, Make reads the `Makefile` and knows exactly which files to compile and in what order.

### 3. Building the Project

To compile the entire project, open a terminal in the project folder (the one containing the `Makefile`) and run:
```bash
make
```

This command will:
- Create the `bin/` directory if it doesn't exist
- Compile all `.cpp` files from the `src/` directory
- Link them together
- Create an executable called `dj_manager` in the `bin/` directory

For a debug build (useful when developing):
```bash
make debug
```

For an optimized release build:
```bash
make release
```

### 4. Cleaning Build Files

To remove all compiled files and start fresh:
```bash
make clean
```

### 5. Running the Program

After building, the program requires both the `-I` (interactive) and `-A` (all playlists) flags:

**Running All Playlists**:
```bash
./bin/dj_manager -I -A
```
This runs the system in automatic mode, processing all available playlists sequentially.

Or use the convenient test target:
```bash
make test
```

**Note**: The `-I` flag enables interactive mode, while the `-A` flag processes all playlists automatically. Both flags are required for proper operation.

### 6. Checking for Memory Leaks

To run the program with valgrind memory leak detection:
```bash
make test-leaks
```

## Main Components

- **AudioTrack**: Base class for audio files
- **MP3Track/WAVTrack**: Specific audio format implementations
- **Playlist**: Manages collections of tracks
- **LRUCache**: Implements Least Recently Used caching strategy
- **CacheSlot**: Individual cache entry management
- **DJSession**: Main session management
- **DJControllerService**: Handles DJ control operations
- **DJLibraryService**: Manages music library
- **MixingEngineService**: Handles audio mixing operations
- **ConfigurationManager**: Manages application settings
- **SessionFileParser**: Parses session configuration files

## Configuration

Edit `bin/dj_config.txt` to modify DJ session settings before running the program.

## Common Make Commands

- `make` or `make all` - Build the entire project
- `make debug` - Build with debug information for development
- `make release` - Build optimized version for production
- `make clean` - Remove all compiled files
- `make test` - Build and run the program
- `make test-leaks` - Run with valgrind to check for memory leaks
- `make install-deps` - Install required development tools (Ubuntu/Debian)
- `make help` - Display all available commands with descriptions

## Student Workflow

The recommended workflow for completing this assignment:

1. **Build with debug info**: `make debug`
2. **Run the program**: `make test`
3. **Find and fix TODOs** in the code
4. **Check for memory leaks**: `make test-leaks`
5. **Repeat** steps 3-4 until all issues are resolved!

## Troubleshooting

**Build Errors**: If you get compilation errors:
1. Make sure all required files are present in `src/` and `include/`
2. Check that your compiler is properly installed: `g++ --version`
3. Try running `make clean` first, then `make`
4. Read the error messages carefully - they usually point to the problem

**Permission Errors**: If you can't execute the program:
```bash
chmod +x ./bin/dj_manager
```

**"Command not found" errors**: 
- If `make` is not found, install it with `sudo apt-get install build-essential` (`make install-deps` needs `make` itself, so it won't work yet)
- If `valgrind` is not found for memory testing, run `make install-deps`

## Development Tips

1. After modifying any `.cpp` or `.h` file, run `make` to rebuild
2. The build system automatically detects which files changed and only recompiles those
3. Always test after making changes by rebuilding and running the program
4. Use `make debug` during development for better error messages
5. Run `make test-leaks` frequently to catch memory issues early

## Getting Updates from Instructors

During the assignment's period, your instructors may publish updates, bug fixes, or clarifications. While we hope there won't be any updates needed, it's good to know how to get them just in case.

### How Updates Are Published
Updates are announced on the course Moodle page (and forum), together with an updated zip file and a list of the files that changed.

### Applying an Update
1. Download the updated zip from Moodle and extract it to a **new, separate folder** - do not extract it over your work
2. Check the announcement to see which files changed
3. Copy only those files into your project folder
4. If a changed file is one you have already edited (for example, a `.cpp` file with your code in it), don't overwrite it - copy the relevant changes into your version by hand
5. Rebuild with `make clean` and then `make`

### When to Check for Updates
- At the start of each work session (just to be safe)
- If your instructor announces an update via email or the course website
- If you encounter unexpected errors that classmates don't have

If you're unsure how to apply an update, contact your course instructor or TA.

---

For questions or issues, please contact the TA in charge on the assignment.