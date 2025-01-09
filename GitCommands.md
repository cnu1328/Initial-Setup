## Setting Up Git Configuration

1. Connect your GitHub account to your Git account by configuring your user name and email globally.
    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "youremail@example.com"
    ```
2. To check your configured user name:
    ```bash
    git config --global user.name
    ```
3. To check your configured email:
    ```bash
    git config --global user.email
    ```

## Cloning a Repository

1. Clone a repository from GitHub to your local machine.
    ```bash
    git clone "repository link"
    ```
2. Navigate to the cloned repository directory.
    ```bash
    cd <repository-directory>
    ```
## Clone the Repository Using SSH
Now, clone the repository using the SSH URL:

Copy the SSH URL from your GitHub repository:
```bash
git@github.com:cnu1328/Django.git
```

Clone the repository:
```bash
git clone git@github.com:cnu1328/Django.git
```


## Managing Changes

1. Check the status of your repository.
    ```bash
    git status
    ```
2. Add all changes to the staging area.
    ```bash
    git add -A
    ```
3. Check the status again to ensure changes are staged.
    ```bash
    git status
    ```
4. Commit the staged changes with a descriptive message.
    ```bash
    git commit -m "Write your descriptive message here"
    ```

## Pushing Changes to GitHub

1. Push the committed changes to the remote repository on GitHub.
    ```bash
    git push origin main
    ```
2. If you encounter the error `! [rejected] main -> main (fetch first)`, follow these steps:
    ```bash
    git fetch origin
    git merge origin/main
    git push origin main
    ```

## Cloning a Specific Branch

If you want to clone a specific branch from the repository, use the following command:
```bash
git clone -b <branch-name> <remote-repository-url>
```

## Creating a New Branch and Pushing Changes in Repository

1. **Create a new branch:**

```bash
git checkout -b <branch-name>
```

2. **Push the branch to the remote repository:**

```bash

git push origin <branch-name>

```

3. **Add changes to the staging area:**

```bash
git add -A
```

4. **Commit changes:**

```bash

git commit -m "Your commit message"

```

5. **Push changes to the remote repository:**

```bash

git push origin <branch-name>


```

