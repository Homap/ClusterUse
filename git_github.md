# Connect local project to github

1. You have a directory on your local or remote computer
2. Create a repository on github without README
3. In your local repository, initialise git
`git init`

If you want to add the whole repository to git
git add . && git commit -m "adding repo X to git"
# If you want to add only a specific directory such as your codes, do:
git add code && git commit -m "code for this study"
# In terminal add the URL of the remote repository where the local will be pushed. If you use the address with https//, you will be later asked for username and password every time you need to push to github:
git remote add origin git@github.com:Homap/ClusterUse.git
# Check if it has been added
git remote -v
# Create ssh key for the repository
ssh-keygen -t ed25519 -C "hpapoli@gmail.com"
# When prompted:
# Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/homap/.ssh/id_ed25519): /home/homap/.ssh/id_ed25519_multi_express
# Change the name. Here I changed from ed25519 to id_ed25519_multi_express to have a project specific deploy key.
# The password is express.
# Start ssh agent in the background and add the key to it.

# Add the deploy key to the repository
# Copy the content of file ~/.ssh/id_ed25519_multi_express.pub and paste as deploy key for the repository on github.
# To do that, go to the github repository, under Settings, Deploy keys, paste the content and check "Allow write access".
# Now the key is added to your github. Activate the ssh-agent in your local repository in Terminal:

eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_multi_express

git branch -M main
git push -u origin main

# Things to remember
# When you want to show an image in github, don't forget to also push the image to the repository otherwise it WON't SHOW IT!
Source: # https://docs.github.com/en/get-started/importing-your-projects-to-github/importing-source-code-to-github/adding-locally-hosted-code-to-github
8816298Golabi?