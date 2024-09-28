# ACIT-2515-Linux-1-Assignment
ACIT 2515 Linux System Administration Assignment 1 (Digital Ocean Droplet Tutorial)

* Ill be making this tutorial on a MAC computer

## Step 1: Generate SSH Keys 
( An SSH key is used to access credintial for the SSH (Secure Shell) network protcol. This authenticated and encrypted  secure network is used for remote communication between machines on an unsecured open network.) 

To securely connect to your DigitalOcean server, you need to create SSH keys: 

  1. Open a terminal on your computer
  2. Run this command to generate a new SSH key wpair:

     ```
     BASH

         ssh-keygen -t ed25519 -C "your_own_email@example.com
     
     ```
  Save your key in the default location and you have the option to add a passphrase (If you do try not to foget it)


  3. Add SSH key to your Digital Ocean Account 
     
