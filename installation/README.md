# ransomware-attack-simulation
Simulation of a Ransomware Attack

## Installing Infection Monkey

### Step 1: Download it
Navigate to [this Github release](https://github.com/guardicore/monkey/releases/tag/v2.3.0) and download the package that corresponds
to your operating system.

### Step 2: Install it
Run the file or package depending on your OS.
<img width="1920" height="943" alt="cdc09f394e1801b3c76d20a2a704599e" src="https://github.com/user-attachments/assets/aa9561ea-4429-4bc4-ad68-90c5c8bfd218" />
As we are using Windows, we simply need to run the .exe. Below will be deployment instructions for Linux.

— [Deployment Instuctions for Linux](https://techdocs.akamai.com/infection-monkey/docs/linux)

### Step 3: Register an account and sign in

<img width="1920" height="1008" alt="c977b38f322b25cf0b72b67a06dc088d" src="https://github.com/user-attachments/assets/f025d7ca-f029-4900-a238-efa69a201f94" />

### Step 4: Install Plugins
Navigate to Plugins and install the Ransomware Plugin. 
<img width="1919" height="1015" alt="b50b7ec82345cdc83f507a9c68031303" src="https://github.com/user-attachments/assets/e8e662e3-4f0d-4b28-8fba-e63e20346777" />
Optional: Install the Powershell plugin as well.

### Step 5: Configure the Ransomware Payload
Configure the Ransomware plugin. Specify the directory specific to your OS to encrypt the files and potentially leave a ransomware note.
<img width="1920" height="908" alt="a1b9d299758c4e9d2289d028bb400d54" src="https://github.com/user-attachments/assets/6cd8215b-d39b-4fbf-81dd-793bc798989f" />

### Step 6: Execute the attack
<img width="1920" height="1039" alt="963b9ee7854e02aa9d9d87f787bab964" src="https://github.com/user-attachments/assets/7c12dc44-a5f1-4030-9037-01caeefc19a8" />

Select 'From Monkey' and the attack will start executing

### Step 7: Check the Aftermath

#### Exhibit A
<img width="1920" height="1004" alt="cff59d92e192afdb2f9a63299e9c98ea" src="https://github.com/user-attachments/assets/66987c28-42b2-4c75-bbb4-9c3454cc0601" />

The contents of my Documents folder was seized by the attacker and held for ransom. The README.txt has a ransom note from the attacker.

#### Exhibit B
<img width="1920" height="1019" alt="8b75bcf4aab6f83fd791ba011d8d87db" src="https://github.com/user-attachments/assets/4736cfcf-f963-48e3-bdd8-e232617d72fa" />

Navigate over to the Events tab. A FileEncryptionEvent is evidence of our files being encrypted.

### Step 8: Reverse Encryption and Reset Environment
In order to reverse the ransomware attack, simply delete the extensions at the end of each encrypted file. After this, 
re-run the ransomware attack. This will work because the ransomware attack encryption flips the bits in order to
encrypt them. Removing the .monk3y file extension and re-running the ransomware attack flips the bits back
to how they were before, decrypting it.

<img width="1920" height="1030" alt="faa6cae551bfd23d0e98001f009f1ff9" src="https://github.com/user-attachments/assets/2a9bd152-5950-4403-8d55-f14a24c78d6b" />

Click the 'Reset' button in the options on the left. This will reset the generated infection map of your detected endpoint(s) and refresh the reports.




