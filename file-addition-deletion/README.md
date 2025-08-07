# Problem
When adding a new java file or removing an existing java file, the bazel plugin is not automatically updating BUILD.bazel file, specifically `srcs` under `java_library`.

# Environment
IntelliJ IDEA 2025.2 (Ultimate Edition)
Bazel plugin 2025.2.3

# File Addition
## Expected Change in file-addition-deletion/src/main/org/example/BUILD.bazel
- Before:

   ```
   java_library(
       name = "Main",
       srcs = ["Main.java"],
   )
   ```
- After:

     ```
     java_library(
         name = "Main",
         srcs = ["Main.java", "NewClass.java"],
     )
     ```
## Reproduction Steps
1. Right click on `file-addition-deletion/src/main/org/example` -> New -> Java Class -> "NewClass"
   - Result: BUILD.bazel is not updated
2. Bazel plugin: `Resync`
   - Result: BUILD.bazel is not updated
3. Bazel plugin: `Build and Resync`
    - Result: BUILD.bazel is not updated

# File Deletion
## Expected Change in file-addition-deletion/src/main/org/example/BUILD.bazel
- Before:

   ```
   java_library(
       name = "Main",
       srcs = ["Main.java", "NewClass.java"],
   )
   ```
- After:

     ```
     java_library(
         name = "Main",
         srcs = ["Main.java"],
     )
     ```
## Reproduction Steps
1. Make sure you have `file-addition-deletion/src/main/org/example/NewClass.java`
2. Make sure you have `srcs = ["Main.java", "NewClass.java"]`
3. Delete file `file-addition-deletion/src/main/org/example/NewClass.java`
    - Result: BUILD.bazel is not updated
4. Bazel plugin: `Resync`
    - Result: BUILD.bazel is not updated
5. Bazel plugin: `Build and Resync`
   - Result: Build failed, missing input file '//src/main/org/example:NewClass.java'