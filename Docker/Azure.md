# Azure VM Setup Guide

This guide provides instructions on how to create a free Azure account, deploy a Virtual Machine (VM) with Ubuntu, and access it using SSH keys, with additional steps to handle common issues encountered in Windows Subsystem for Linux (WSL).

## 1. Creating a Free Azure Account

### Requirements
- A valid email address.
- A phone number for verification.
- A credit card (for identity verification purposes; you will not be charged).

### Steps
1. **Visit Azure Free Account Setup**:
   - Navigate to [Azure Free Account](https://azure.microsoft.com/en-us/free/) page and click **Start free**.

2. **Provide Your Details**:
   - Use your email to get started or sign in with a Microsoft account and fill in the required fields.

3. **Verification by Phone**:
   - Input your phone number and verify it with the code sent via SMS.

4. **Verification by Credit Card**:
   - Enter your credit card details for identity verification. No charges will apply unless you opt for a paid upgrade.

5. **Agreement and Sign-Up**:
   - Agree to the Microsoft Azure subscription agreement and complete the sign-up process.

## 2. Creating a Virtual Machine with Ubuntu

### Steps
1. **Log In to Azure Portal**:
   - Go to [Azure Portal](https://portal.azure.com) and log in.

2. **Create a New VM**:
   - Click **Create a resource**, type "Ubuntu Server" in the search box, and select "Ubuntu Server 20.04 LTS".

3. **Configure Your VM**:
   - Set up the VM details (name, region, size, etc.).
   - For Authentication type, select "SSH public key".
   - Enter a username (e.g., `azureuser`).
   - Upload your public SSH key or use the one provided by Azure.

4. **Adjust Networking Settings**:
   - Ensure that the inbound port rules allow SSH connections (port 22).

5. **Review and Create**:
   - Verify your settings and click **Create** to deploy your VM.

6. **Download SSH Private Key**:
   - If Azure generated your SSH key pair, download the private key file (e.g., `Docker_key.pem`).

## 3. Accessing the VM Using SSH

### For Unix-like Systems (Linux/macOS)

1. **Set Permissions**:
   - Change the permissions of your SSH key file to read/write for the owner only:
     ```bash
     chmod 600 /path/to/your/private/key
     ```

2. **Connect via SSH**:
   - Use the following command to connect:
     ```bash
     ssh -i /path/to/your/private/key username@VM-public-IP
     ```

### For Windows Subsystem for Linux (WSL)

1. **Verify Permissions**:
   - Check the permissions with `ls -l` and ensure they appear as `-rw-------`.

2. **Change Permissions**:
   - If needed, set the permissions again:
     ```bash
     chmod 600 /path/to/your/private/key
     ```

3. **Move the Key to WSL Home**:
   - If permissions aren't correct under `/mnt/c`, move the key:
     ```bash
     mv /mnt/c/Users/your_username/path/to/key ~/key.pem
     chmod 600 ~/key.pem
     ```

4. **Connect via SSH**:
   - Connect to your VM using the moved and secured key:
     ```bash
     ssh -i ~/key.pem username@VM-public-IP
     ```

###  Connect via SSH Using PuTTY

Step 1: Download and Install PuTTY
Download PuTTY:

Go to the official PuTTY download page: PuTTY Download Page.
Download the .exe file appropriate for your system (typically putty-<version>-installer.msi for most users).
Install PuTTY:

Run the downloaded MSI file and follow the installation prompts to install PuTTY on your Windows machine.
Ensure that you install all the components, including PuTTYgen if you need to generate SSH keys.
Step 2: Generate an SSH Key Pair (if required)
If you do not already have an SSH key pair (which consists of a public and a private key), you will need to generate one using PuTTYgen:

Open PuTTYgen:

Start PuTTYgen from the Start menu.
For newer SSH-2 servers, make sure the parameters at the bottom are set to RSA and 2048 bits.
Generate the Key Pair:

Click the Generate button and follow the instructions to create the key pair. Typically, this involves moving your mouse over the blank area to generate randomness.
Once the key generation is complete, you will see your public key in the PuTTYgen window.
Save Your Private Key:

Click Save private key to save your private key to a file. Choose a secure location and remember where you saved it. You might also want to set a passphrase for added security.
Copy Your Public Key:

Right-click in the text field labeled "Public key for pasting into OpenSSH authorized_keys file" and select all the text, then right-click and copy. You will need to paste this into your .ssh/authorized_keys file on your server or provide it to your server administrator.
Step 3: Configure PuTTY for SSH Connection
Open PuTTY:

Start PuTTY from the Start menu.
Session Configuration:

In the Host Name (or IP address) field, enter the public IP address or hostname of the server you wish to connect to.
Ensure the Port is set to 22 (unless your server uses a different port for SSH).
Select SSH under Connection type.
Auth Configuration:

In the category tree on the left, go to Connection -> SSH -> Auth.
Click the Browse button under Private key file for authentication and select the private key file you saved earlier.
Optional: Save Your Session:

Go back to the Session category.
In the Saved Sessions field, enter a name for this session setup.
Click Save to save these settings so you don’t have to enter them again in the future.
Step 4: Connect to Your Server
Open PuTTY:

Load your saved session by double-clicking the session name under Saved Sessions in the PuTTY Configuration window.
Login:

Once the connection is established, a terminal window will open asking for your username.
Enter your username and press Enter.
If you set a passphrase for your private key, PuTTY will ask for it now; enter the passphrase and press Enter.
You're Connected:

If all goes well, you will be logged into your server via SSH.
    

## Conclusion

By following these instructions, you can set up a free Azure account, create and configure a VM with Ubuntu, and securely connect to it using SSH. Make sure to handle your private keys carefully, especially under Windows Subsystem for Linux (WSL), to maintain security.
