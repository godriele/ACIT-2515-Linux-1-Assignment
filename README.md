# ACIT-2515-Linux-1-Assignment
ACIT 2515 Linux System Administration Assignment 1 (Digital Ocean Droplet Tutorial)

# Introduction
## In this tutorial, Ill be guiding you on how to create a remote server running Arch Linux on Digital Ocean. 
## In this guide you will learn to: (* Ill be making this tutorial on a MAC computer) 

  - Create SSH keys on your local machine
  - Add a custom Arch Linux image
  - Create a Droplet using the DigitalOcean Web console
  - Use a cloud-init configuration file to automate initial setup taks
  - Connect to your server using your SSH keys

---------------------------------------------------------------------------------------------------------------------
  
## Step 1: Creating SSH Keys 

   # What are SSH keys: 
   An SSH key is an access credential in the SSH protocol. It is used to authenticate the identity of a user or process that wants to access a remote system using     the SSH protocol
 
   # Why would you need it: 
   You need an SSH Key for secure, passwordless authentication to remote servers, enhancing security over passwords. 
   
   Reference = https://www.ssh.com/academy/ssh-keys
   ---------------------------------------------------------------------------------------------------------------------
 
   To securely connect to your DigitalOcean server, you need to create SSH keys: 

   1. Open a terminal on your computer
   2. Run this command to generate a new SSH key wpair:

         ssh-keygen -t ed25519 -C "your_own_email@example.com"
     
     
  Save your key in a location you would know (/User/downloads..........ssh/id_ed25519)
  you have the option to add a passphrase (If you do try not to foget it)

'''
Godrieles-Air:~ goody$ ssh-keygen -t ed25519 -C "goodydelacruz6@gmail.com"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/Users/goody/.ssh/id_ed25519): /Users/goody/Downloads/id_ed25519
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/goody/Downloads/id_ed25519
Your public key has been saved in /Users/goody/Downloads/id_ed25519.pub
The key fingerprint is:
SHA256:rtsf+GH9hKtoADyMZGbpgvxFfi61sfg/Z+PBR/b6dNg goodydelacruz6@gmail.com
The key's randomart image is:
+--[ED25519 256]--+
|   .             |
|  *  .           |
|o* +o            |
|o.o =o +         |
| .. .o= S    o   |
|   . o.= ...o..o |
|      o.o +oo.ooE|
|       +.= *o+o .|
|      oo+oOoo.o. |
+----[SHA256]-----+

** Mine
'''
  ---------------------------------------------------------------------------------------------------------------------

  3. Add SSH key to your Digital Ocean Account
   
     # Why would you need to connect your SSH key to your Digital Ocean account:
      You need to connect your SSH key to your DigitalOcean account because if you connect your key to your DigitalOcean,
      only someone with your private key can access you server. This proctects your servers from unauthorized users.

     Reference = https://docs.digitalocean.com/products/droplets/how-to/add-ssh-      keys/#:~:text=On%20DigitalOcean%2C%20you%20can%20upload,password%20while%20still%20remaining%20secure.
     ---------------------------------------------------------------------------------------------------------------------
     
      1. Copy your Public Key
           - cat /Users/goody/Downloads/id_ed25519.pub (yours will differ depending where you kept your key) 

      2. Log to your Digital Ocean Account
      3. On the left-hand side you will be able to see Settings, click on it
      4. On the Settings page, click on the security tab
      5. Click on the ADD SSH KEY
      6. Paste your public key and give it a proper name (Ex. Linux Assignment 1...) and click Add SSH Key

     ---------------------------------------------------------------------------------------------------------------------

## Step 2: Adding a custom Arch Linux Image using the web console 

  
  # Why would you need to a custom Arch Linux Image?:
  Having a custom Arch Linux images allows you to create a server that way you would want it. This means you can deploy new servers faster, and making sure they      all have a consistent setup
    
  Reference = https://www.freecodecamp.org/news/how-to-install-arch-linux/#:~:text=In%20other%20words%2C%20Arch%20Linux,desktop%20environment%2C%20and%20so%20on.


  You can grab a custome Arch Linux Image here -> https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/
  You can download any
  ---------------------------------------------------------------------------------------------------------------------
  

## Step 3: Creating a Droplet running Arch Linux using the Digital Ocean Web Console

  1. In the main page of Digital Ocean, on top right theres a button that says create. Click on it
  2. There will be a dropdown, Click on the first one (Droplets)
  3. Choose the region: San Fransico
  4. In the choose image option, click on the custom image
  5. Add the image you downloaded on ## Step 2
  6. Choose size: You can just choose the basic plan
  7. CPU option: Premium AMD -> This is up to you on how much you want to pay but I recommend just doing the cheapest one
  8. Choose Authentication: SSH Key
  9. Choose your SSH key: choose the one you've made on ## Step 1 (Ex. TestForAsssignment1
  10. On the bottom right click Create Droplet.
  11. Congrats you've made a Droplet!

  ---------------------------------------------------------------------------------------------------------------------

## Step 4: Use a cloud-init configuration file to automate initial setup tasks
 Reference = https://cloudinit.readthedocs.io/en/latest/tutorial/qemu.html

   1. Install QEMU:
      # What is QEMU:
        It is a cross platform emulator capable of running performant virtual machines

      # On mac you have to first install QEMU:
         brew install qemu
         Open your terminal and paste this: sudo apt install qemu-system-x86

      # To verify you have it download paste this:
      sudo qemu-system-x86_64 --version
      Reference: https://support.huawei.com/enterprise/en/doc/EDOC1100034237/2840d231/how-do-i-obtain-and-install-the-qemu-      tool

    ---------------------------------------------------------------------------------------------------------------------
    
  2. Create a Temporary directory
       # You will store our cloud image and configuration files for user data.

       $ mkdir temp
       $ cd temp

    ---------------------------------------------------------------------------------------------------------------------
        

  4. Download a Cloud image
       # You will first download wget:
       brew install wge
       # Paste this in your terminal: wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img

    ---------------------------------------------------------------------------------------------------------------------
      
  5. Define your User Data
      1. Create the user-data file
         paste this in the terminal: $ cat << EOF > user-data
                                     #cloud-config
                                     password: password
                                     chpasswd:
                                      expire: False
                                    
                                     EOF

          2. Inspect the your data file: cat user-data
             Your should see the following
             #cloud-config
             password: password
             chpasswd:
             expire: False
      ---------------------------------------------------------------------------------------------------------------------
        

  6. Define your Metadata:
     # Which is another configuration fule that cloud-init uses to gather instance-specif information
     
     1. Paste this code: cat << EOF > meta-data
                         instance-id: someid/somehostname

                         EOF
        
     2. Define our vendor data
        # Inside the temp directory paste this in to speed up the retry wait time
        Paste this: touch vendor-data


    ---------------------------------------------------------------------------------------------------------------------
      
  7. Stat an Ad Hoc IMDS Web Server
     1. Open a new terminal: In the terminal app press cmd+n
     2. Navigate back to the temp directory : cd temp
     3. Run this Command to start the Python HTTP Server


    ---------------------------------------------------------------------------------------------------------------------
      
  8. Launch the Virtual Machine with QEMU
     1. Go to the orginal terminal where you downloaded the cloud image and paste this command:
        qemu-system-x86_64                                            \
          -net nic                                                    \
          -net user                                                   \
          -machine accel=kvm:tcg                                      \
          -m 512                                                      \
          -nographic                                                  \
          -hda jammy-server-cloudimg-amd64.img                        \
          -smbios type=1,serial=ds='nocloud;s=http://10.0.2.2:8000/'
        
        This may take time to load, if its stop scrolling but you dont see a login prompt, just press enter

    ---------------------------------------------------------------------------------------------------------------------
      

9. Log In
   1. Paste these in the terminal:
     unbuntu login: ubuntu
     Password: password

   2. Check the status
      Paste this in the terminal: cloud-init status --wait

      You should be able to see: status done


   ---------------------------------------------------------------------------------------------------------------------
      
10. Close the Environment
    1. Exit the QEMU Virtaul Machine
       Press: Ctrl + A
       Then press: x

    2. Stop the Python HTTP Server
       Go to the terminal where your Started the Python HTTP server and stop it by pressing: Ctrl + C
  ---------------------------------------------------------------------------------------------------------------------

## Step 5: Connecting your server using your SSH key
  Reference = https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/

  1. Get your Droplets's IP Address
     1. Log in to Digital Ocean
        - On the left side, click on Droplets
        - Select the Droplet you've created on ## Step 3:

     2. Copy the IP Address: (Ex. ipv4: 161.35.226.10) 


 2. Open the terminal

 3. Use the SSH Command to Conenct to your Droplet
    1. Open your terminal and paste this in: ssh -i ~/.ssh/do-key root@161.35.226.10
    2. If set a passphrase for your SSH key, type in your passphrase
   
 4. Succesful Connection
    - To see if you've connected sucessfuly, you should see the foollowing: [root@Test1Assingment ~]#

  5. If you want to exist type in: Exit

  ---------------------------------------------------------------------------------------------------------------------


Congrats, if you were able to get to this part of the tutorial, you've successfuly:
  - Created a SSH key on your local machine
  - Added a Custom Arch Linux using the web console
  - Created a Droplet running Arch Linux using the Digital Ocean web console
  - Use a cloud-init configuration file to automate initial setup task
  - Connected your server using your SSH Keys.



