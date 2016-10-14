# Reference on Build Systems

[Custom Sublime Text Build Systems For Popular Tools And Languages](http://addyosmani.com/blog/custom-sublime-text-build-systems-for-popular-tools-and-languages/)

“.sublime-build” file – a JSON representations of the commands, paths and configuration needed to build a project using a specific tool or set of tools.

## Adding a custom Build System

Sublime populates its Tools/Build System menu based on the “.sublime-build” files stored in “~/Library/Application Support/Sublime Text 2/Packages/User” (if using OS X). A basic “.sublime-build” file could be represented in key/value form as follows:

<pre class="javascript" name="code">{
    "cmd": ["command", "argument", "--flag"],
    "selector": ["source.js"],
    "path": "/usr/local/bin",
    "working_dir": "/projects/"
}
</pre>

### Keys supported include:

*   **cmd** - An array containing a command to run and its desired arguments and flags. Note that Sublime will search your PATH for any tools listed unless an absolute path has been used to point to them.
*   **selector** – An optional string used to locate the best builder to use for the current file scope. This is only relevant if Tools/Build System/Automatic is true.
*   **path** – An optional string that replaces your current process’s PATH before calling the commands listed.
*   **working_dir** – An optional string defining a directory to switch the current directory to prior to calling any commands.
*   **shell** - An optional boolean that defines whether commands should be run through the shell (e.g bash).
*   **file_regex** – An optional regular expression used to capture error output from commands.

For a comprehensive list of keys supported in Sublime build scripts, see the [unofficial docs](http://docs.sublimetext.info/en/latest/reference/build_systems.html).

### Build Variables:

In addition, Sublime supports variable substitutions in build files such as `$file_path` (for the path to the current file) and more. These include:

*   **$file_path** – the directory of the current file being viewed
*   **$file_name** - only the name portion of the current file (extension included)
*   **$file_base_name** - the name portion of the current file (extension excluded)
*   **$project_path** - the directory path to the current project
*   **$project_name** – the name portion of the current project

A complete list of [substitutions](http://docs.sublimetext.info/en/latest/reference/build_systems.html) supported is also available.

### Grouping build tasks

Some developers also like to group together tasks within an external bash script (or equivalent). For example, here’s a simple [git-ftp](https://github.com/resmo/git-ftp) deploy script you can use with Sublime to commit and push your latest changes with `git` and then upload your latest files to FTP.

### Example: Commit, Push And Upload To FTP

**deployment.sh:**

<pre class="javascript" name="code">#!/bin/bash
git add . && git commit -m 'deployment' && git push && git ftp init -u username  -p password - ftp://host.example.com/public_html
</pre>

**deployment.sublime-build:**

<pre class="javascript" name="code">{
  "cmd": ["deployment"],
  "working_dir": "${project_path:${folder}}"
}
</pre>

If you haven’t used git-ftp before, Alex Fluger has a solid [article](http://alexfluger.blogspot.co.uk/2012/01/easy-deploy-to-ftp-from-git.html) about using it that may be of interest.
