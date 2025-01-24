
# Setting up a dev container for Rust

* Primary author: [Arsh Madhani](https://github.com/arshm06)
* Seconday author: Kris Jordan's Comp 423 Tutorial
* Reviewer: [Rithwik Mishra](https://github.com/rithwik-mishra)

## Part 1 - Prerequisites:

Before we dive in, make sure you have:

1. **A GitHub account**: If you don’t have one yet, sign up at [GitHub](https://github.com/).

2. **Git installed**: Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if you don’t already have it.

3. **Visual Studio Code (VS Code)**: Download and install it from [here](https://code.visualstudio.com/).

4. **Docker installed**: Required to run the dev container. [Get Docker here](https://www.docker.com/).

5. **Command-line basics**: A quick online course should provide the necessary information. A suggested text is attached [here](https://www.learnenough.com/command-line-tutorial/basics?srsltid=AfmBOoo-cNiimvpZ0gjStoCaKkLb28Z3x73CFC5OQDVN1AvisxNV6J48).


## Part 2 - Git: Creating the Repository

### Step 1. Create a Local Directory and Initialize Git

A. Open your terminal or command prompt.

B. Create a new directory for your project.
```bash 
mkdir rust-setup-tutorial
cd rust-setup-tutorial
```
C. Initialize a Git repository.
```bash
git init
```
D. Create a README file:
```bash
echo "# Hello Rust" > README.md
git add README.md
git commit -m "Initial commit with README"
```

### Step 2. Create a Remote Repository on GitHub

1. Log in to your GitHub account and navigate to the [Create a New Repository](https://github.com/new) page.

2. Fill in the details as follows:
    - **Repository Name**: `rust-setup-tutorial`
    - **Description**: "Rust tutorial + containerizing for a first time user"
    - **Visibility**: Public

3. Do **not** initialize the repository with:
    - a README
    - a `.gitignore`
    - a license

4. Click **Create Repository**.

### Step 3. Link Your Local Repository to GitHub

1. **Add the GitHub repository as a remote:**
```bash
git remote add origin https://github.com/your-username/rust-setup-tutorial.git
```
!!! warning
    Make sure to replace `your-username` with your GitHub username.

1. Check your default branch name with the subcommand ` git branch `. If it's not `main`, rename it to main with the following command: ` git branch -M main `. Old versions of ` git ` choose the name ` master ` for the primary branch, but these days ` main ` is the standard primary branch name.

1. Push your local commits to the GitHub repository:
```bash
git push --set-upstream origin main
```

!!! info "Understanding the --set-upstream Flag"
    ` git push --set-upstream origin main `: This command pushes the main branch to the remote repository origin. The ` --set-upstream ` flag sets up the main branch to track the remote branch, meaning future pushes and pulls can be done without specifying the branch name and just writing ` git push origin ` when working on your local ` main ` branch. This long flag has a corresponding ` -u ` short flag.

## Part 3 - Devolpment Container
### What is a Devlopment (Dev) Container?
A dev container ensures that your development environment is consistent and works across different machines. At its core, a dev container is a preconfigured environment defined by a set of files, typically leveraging Docker to create isolated, consistent setups for development. Think of it as a "mini computer" inside your computer that includes everything you need to work on a specific project—like the right programming language, tools, libraries, and dependencies.

Why is this valuable? In the technology industry, teams often work on complex projects that require a specific set of tools and dependencies to function correctly. Without a dev container, each developer must manually set up their environment, leading to errors, wasted time, and inconsistencies. With a dev container, everyone works in an identical environment, reducing bugs caused by "it works on my machine" issues. It also simplifies onboarding new team members since they can start coding with just a few steps.

### How are software project dependencies managed?
To effectively manage software dependencies, it's important to understand package and dependency management. In most software projects, you rely on external libraries or packages to save time and leverage work that has already been done by others. Managing these dependencies ensures that your project has access to the correct versions of these libraries, avoiding compatibility issues.

To ensure your dependencies are always correctly installed and available, in standard Rust projects relying on cargo, requirements are traditionally listed out in a Cargo.toml file in the project's root directory. This file is committed to your project's version control history so that as your project adds or updates dependencies, it is reflected in the project's history. This allows anyone working on the project to quickly set up their environment by installing the necessary dependencies with the pip install command. The dev container configuration you setup will automatically install dependencies from Cargo.toml when the container is created. This allows anyone working on the project to have a complete environment setup in one step: starting a dev container.

In summary, the the devcontainer.json file specifies configuration for a consistent development environment using a Docker image. The Cargo.toml file ensures all needed Rust package for our project are installed when the container is created. Together, these files automate the process of setting up a developer environment, making it easier for you and others to work on the project.

### Step 1. Add Development Container Configuration
1. In VS Code, open the rust-setup-tutorial directory. You can do this via: File > Open Folder.

2. Install the Dev Containers extension for VS Code.
3. Create a `.devcontainer` directory in the root of your project with the following file inside of this "hidden" configuration directory: `.devcontainer/devcontainer.json`. The devcontainer.json file defines the configuration for your development environment. Here, we're specifying the following:
    - **name**: A descriptive name for your dev container.
    - **image**: The Docker image to use, in this case, the latest version of a Rust environment. [Microsoft maintains a collection of base images for many programming language environments](https://hub.docker.com/r/microsoft/vscode-devcontainers), but you can also create your own!
    - **customizations**: Adds useful configurations to VS Code, like installing the Rust extension. When you search for VSCode extensions on the marketplace, you will find the string identifier of each extension in its sidebar. Adding extensions here ensures other developers on your project have them installed in their dev containers automatically.
    - **postCreateCommand**: A command to run after the container is created. The curl command will install Rust using the official rustup installer.

```json
{
  "name": "Rust Dev Container",
  "image": "mcr.microsoft.com/devcontainers/rust:latest",
  "customizations": {
    "vscode": {
      "settings": {},
      "extensions": ["rust-lang.rust-analyzer"]
    }
  },
  "postCreateCommand": "curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh"
}
```

### Step 3. Reopen the Project in a VSCode Dev Container

1. Reopen the project in the container by pressing `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac), typing "Dev Containers: Reopen in Container," and selecting the option. This may take a few minutes while the image is downloaded and the requirements are installed.

2. Once your dev container setup completes, close the current terminal tab (trash can), open a new terminal pane within VSCode, and try running ```#!bash rustc --version``` to see your dev container is running a recent vesrsion of Rust without much effort!

### Step 4. Create a new Rust Project

1. With the Dev Container running, the next step is to create a new Rust project using the cargo new command. Run the following in your terminal:
```bash
cargo new rust_project --vcs none
cd rust_project
```
!!! note
    The --vcs flag ensures that a new git repository is not already created on our behalf since we made one in our previous steps!

2. Inside your rust_project directory, open the src/main.rs file. Modify it to print "Hello COMP423" to the standard output:
```rust
fn main() {
    println!("Hello COMP423");
}
```

3. Next, use the cargo build command to compile the project.
```bash
cargo build
```
4. The ```#!bash cargo build``` command will download any dependencies (if needed) and compile the project. This is similar to how you would use the ```gcc``` command in COMP211 to compile a C program. The compiled file will be located in the ```target/debug``` directory.
<br/>
To run the compiled program, use the ```#!bash cargo run command```. This will both compile and execute the program, and you should see the command line output the string "Hello COMP423" from the main.rs file.


!!! info "Differences Between Cargo build and Cargo run:"
    Cargo build: This command only compiles the project without running it. The compiled binary is stored in the target/debug directory.
    <br/>
    Cargo run: This command compiles the project (if needed) and immediately runs the resulting binary.