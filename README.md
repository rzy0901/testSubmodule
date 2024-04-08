# testSubmodule

Create a submodule:

```bash
git submodule add <repository_url> <path_to_directory>
# Example:
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

Remove submodule:

```bash
git submodule deinit -f -- <path_to_submodule>
git rm -f <path_to_submodule>
rm -rf .git/modules/<path_to_submodule>
# Example:
git submodule deinit -f -- sampleSubmodule 
git rm -f sampleSubmodule
rm -rf .git/modules/sampleSubmodule
```

Clone and pull with submodule:

```bash
git clone --recurse-submodules <repository_url>
git pull --recurse-submodules
```

Automatically update submodule using github action (every day at 10:45 UTC, every push, or manually):
    
+ Add a Personal Access Token (PAT) to the repository secrets with `repo` and `workflow` scope.

+ Add a workflow file to the repository at `.github/workflows/update_submodules.yml`:

    ```yaml
    name: Update Submodules

    on:
    push:
        branches: [ main ]
    schedule:
        - cron: "45 10 * * *"
    workflow_dispatch:

    jobs:
    update_submodules:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout repository
            uses: actions/checkout@v2
            with:
            submodules: recursive
            token: ${{ secrets.PAT }}  

        - name: Update submodules
            run: |
            git submodule update --remote

        - name: Commit changes
            run: |
            if git diff --exit-code; then
                echo "No submodule updates detected."
            else
                git config --local user.email "action@github.com"
                git config --local user.name "GitHub Action"
                git add .
                git commit -m "Update submodules"
                git push
            fi
    ```