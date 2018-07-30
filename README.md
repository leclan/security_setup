# Security setup for using GitHub and Heroku with 1Password

**1Password :**

First of all follow the 1Password setup steps and make sure you download the desktop app and the Google Chrome browser extension, and that everything works fine !

***

**SSH Key :** 

Generate an SSH key following these steps :


Open a terminal and type this, replacing the email with yours (the same one you used to create your GitHub account). It will prompt for information. Just press enter until it asks for a passphrase.


`mkdir -p ~/.ssh && ssh-keygen -t ed25519 -o -a 100 -f ~/.ssh/id_ed25519 -C "TYPE_YOUR_EMAIL@HERE.com"`

When asked for a passphrase, open your 1Password desktop app, and create a new strong password and call it *SSH Key*, and then copy paste it in the terminal. Nothing will show up on the screen, **that's normal**, when you're done, press Enter.


**NB :** If you are already using an SSH key, and configured your computer in order not to re-type your SSH passphrase at every `git push`, you need to follow these steps in order to reset it. We want you to retype your passphrase at every `git push`, to make sure that if someone gains access to your computer, he cannot compromise the GitHub or Heroku repos from the Command Line.


In the terminal type `st ~/.ssh/config `


Then in Sublime text you should see something like :


`Host *  
  AddKeysToAgent yes  
  UseKeychain yes`  
  
  
Remove these lines and save the file. Then restart your computer and voilà !

***

**GitHub :**


1. Using SSH key with GitHub


Then you need to give your public key to GitHub. Run:


`cat ~/.ssh/id_ed25519.pub`


It will prompt on the screen the content of the id_ed25519.pub file. Copy that text, then go to github.com/settings/ssh. Click on Add SSH key, fill in the Title with your computer name, and paste the Key. Finish by clicking on the Add key green button.


To check that this step is completed, in the terminal run this. You will be prompted a warning, type yes then Enter.


`*ssh -T git@github.com*`


If you see something like this, you're done!


`*Hi --------! You've successfully authenticated, but GitHub does not provide shell access*`


If it does not work, try running this before trying again the ssh -T command:


`*ssh-add ~/.ssh/id_ed25519*`


2. Using Two-factor Authentication with GitHub and 1Password


Go to https://github.com/settings/security and follow the different steps.  
At some point it will show you a QR Code, you will need to follow these steps:  
1. go to your mobile 1Password app  
2. click on the GitHub login  
3. click on Edit  
4. scroll down to *add 1 time password*  
5. click on the QR code logo  
6. scan the QR code that is displayed on your computer screen  
7. *Save* and then provide the 6 digit code to GitHub and validate


Voilà !

***

**Heroku :**


1. Using SSH key with Heroku


We will use the same SSH Key that we use for GitHub and that we generated in the first section. For that follow these steps :


In the terminal, type :


`*heroku keys:add ~/.ssh/id_ed25519.pub*`


Then type in the terminal :


`*git config --global url.ssh://git@heroku.com/.insteadOf https://git.heroku.com/*`


Then check if it worked by typing the following (from a folder that is connected to an Heroku repo):


`*git remote -v*`


You should see something like that :


`*heroku	ssh://git@heroku.com/limitless-chamber-84110.git (fetch)*`  
`*heroku	ssh://git@heroku.com/limitless-chamber-84110.git (push)*`  
`*origin	git@github.com:oruhtra/chat-rails-redux.git (fetch)*`  
`*origin	git@github.com:oruhtra/chat-rails-redux.git (push)*`


Then on your first connection to Heroku from the terminal, you may be prompted something like :


`*The authenticity of host 'heroku.com (50.19.85.132)' can't be established.*`  
`*RSA key fingerprint is SHA256:8tF0wX2WquK45aGKs/...*`  
`*Are you sure you want to continue connecting (yes/no)?*`


Type *yes* and voilà ! Everytime you will interact with heroku from the command line you will be prompter for the SSH passphrase which you get from 1Password.

2. Using Two-factor authentication with Heroku


Go to your Heroku account in the browser, and click on **Account settings**, then scroll and enable Two-factor authentication. Follow the steps (similar to GitHub), using your 1Password mobile app.

Voilà !

=> When you'll want to push to Heroku, you might be first asked to provide your credentials (fill in with your information)  
`Email [arthur.reboul@gmail.com]: [YOUR HEROKU ACCOUNT EMAIL] 
Password: [YOUR HEROKU ACCOUNT PASSWORD STORED IN 1PASSWORD]
Two-factor code: [A ONE TIME CODE PROVIDED BY 1PASSWORD ON THE HEROKU LOGIN PAGE]`

=> Then you will be asked with the SSH key passphrase:  
`Warning: Permanently added the RSA host key for IP address '50.19.85.154' to the list of known hosts.  
Enter passphrase for key '/Users/arthurreboul/.ssh/id_ed25519': [YOUR SSH KEY PASSPHRASE]`

