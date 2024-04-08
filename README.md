# testSubmodule

Create submodule from: <https://github.com/rzy0901/sampleSubmodule>:

```bash
git submodule add <repository_url> <path_to_directory>
git submodule add https://github.com/rzy0901/sampleSubmodule ./sampleSubmodule
```

Initialize and update submodule:

```bash
git submodule update --init --recursive 
```

Update submodule to the latest commit in its remote repository:

```bash
git submodule update --remote
```