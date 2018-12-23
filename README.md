# How-to-work-with-different-Git-accounts
Working with a Github account is as easy as 1-2-3. However, all this complicates when you have to juggle, from your development workstation, between different GitHub accounts (personal and pro for example) and a GitLab account.
We'll see together how to make it easy!


# Requirement

- [Git](https://git-scm.com/) installed
- Have a GitHub account
- Have a GitLab account

# Configuration
## Create SSH Keys
### GitHub
We need to generate a unique SSH key for our GitHub account.
```
ssh-keygen -t rsa -C "email-address-to-your-GitHub-account"
```

Be careful that you don't over-write your existing key!
When prompted, save the file in your ssh directory (`~/.ssh/`) as `id_rsa_github`.

### GitLab
As for GitHub, we create a unique SSH key for our GitLab account and register it with 'id_rsa_gitlab'.
```
ssh-keygen -t rsa -C "email-address-to-your-GitLab-account"
```

### Other
If you wish, you can reproduce this for other GitHub/GitLab accounts (professional for example). To do this, enter the email address to your pro account when generating the SSH key and register it under 'id_rsa_gitlab_company'.

## Attach the keys
Now, connect to your GitHub account, access the "SSH and GPG keys" page from "Settings" to your account. Attach the key, by clicking on the "New SSH key" button, in the "SSH keys" section.
To retrieve the value of the key that you just created, return to the Terminal, and type: `vim ~/.ssh/id_rsa_github.pub`. Copy the entire string that it displayed, and paste this into the GitHub textarea. Feel free to give it any title you wish.
Next, because we saved our key with a unique name, we need to tell SSH about it. Within the Terminal, type `ssh-add ~/.ssh/id_rsa_github`.
If successful, you'll see a response of "Identity Added".

Let's do the same for our personnal GitLab or GitLab company account.
Copy/paste the complete content to your id_rsa_gitlab file into the SSH Keys section from your Gitlab account and run the following command `ssh-add ~/.ssh/id_rsa_gitlab`.

## Create a Config file
We've done the bulk of the workload; but now we need a way to specify when we wish to push our GitHub account, and when we should instead push to or GitLab account. To do so, let's create a `config` file.

```
touch ~/.ssh/config
vim config
```
Paste in the following snippet.
```
# GitHub account
Host github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_github
```
This is the setup for publishing to our GitHub account.
Let's add another one for the GitLab account. Directly below the code aboce, add:

```
# GitLab account
Host gitlab.com
HostName gitlab.com
User git
IdentityFile ~/.ssh/id_rsa_gitlab
```
For push to GitLab, we defined the host at `gitlab.com`, we changed the host name to `gitlab.com` and we provide the path to our `id_rsa_gitlab` file for IdentityFile.

Si besoin, vous pouvez configurer d'autres comptes GitHub/GitLab (professionnel par exemple).
```
# GitLab company account
Host gitlab-company
HostName gitlab.com
User git
IdentityFile ~/.ssh/id_rsa_gitlab_company
```

Save and exit!

# Try it out
It's time to see if our configuration were successful.

Try to push a test code to your personal GitHub account, and push another test code to your GitLab company account:
```
git remote add origin git@github.com:your-personal-account/testing.git
git push origin master
...
git remote add origin git@gitlab-company:your-company-account/testing.git
git push origin master
```
Note that, the second time, rather than pushing to `git@gitlab-company`, we're using the custom host that we create in the `config` file.

Remember, when cloning/pushing:
- To your personnal GitHub account, proceed as you always have.
- For your personnal GitLab or GitHub company account, make sure that you use `git@gitlab.com` or `git@github-company` as the host.

# Credits
- [Jeffrey Way's post on code.tutsplus.com](https://code.tutsplus.com/tutorials/quick-tip-how-to-work-with-github-and-multiple-accounts--net-22574)
