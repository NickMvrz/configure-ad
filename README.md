# configure-ad
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" height="40%" width="70%"alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring Active Directory (On-Premises) Within Azure</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resources
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (myadproject.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users

<h3 align="center">Setup Resources in Azure</h3>
<br />
<p>
  Create the Domain Controller VM (Windows Server 2022) named “DC-1”:
</p>
<p>
  <img src="https://i.imgur.com/gaAzjvb.png" height="75%" width="100%" alt="resource group"/>
  <img src="https://i.imgur.com/hubTfey.png" height="75%" width="100%" alt="vm ms server"/>
</p>
<p>
  Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in previous step:
</p>
<p>
  <img src="https://i.imgur.com/XyEmv8f.png" height="75%" width="100%" alt="vm windows"/>
</p>
<p>
  Set Domain Controller’s NIC Private IP address to be static:
</p>
<p>
  <img src="https://i.imgur.com/KHU9kC4.png" height="75%" width="100%" alt="static ip"/>
</p>
<p>
  Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher):
</p>
<p>
  <img src="https://i.imgur.com/rFpHLdQ.png" height="75%" width="100%" alt="topology"/>
</p>
<br />
<br />
<h3 align="center">Ensure Connectivity between the client and Domain Controller</h3>
<br />
<p>- Log on to Client-1 and ping the Domain Controller to ensure they are connected.
</p>

<img width="860" alt="Screenshot 2024-12-18 at 8 57 00 PM" src="https://github.com/user-attachments/assets/3d34dff9-dec9-41c0-b488-e6d0f709df49" />
<p> 
-Go into DC-1 and install Active Directory Domain Services. Domain name: NicksDomain (or whatever name you choose)
</p>


<img width="649" alt="Screenshot 2024-12-18 at 9 01 30 PM" src="https://github.com/user-attachments/assets/978b7b6f-97f8-4aa3-940a-a31eeb0bcace" />

<img width="1246" alt="Screenshot 2024-12-18 at 9 01 38 PM" src="https://github.com/user-attachments/assets/53dcf0eb-3b4e-4a67-b1b1-6c1e3f072e22" />

<img width="952" alt="Screenshot 2024-12-18 at 9 02 00 PM" src="https://github.com/user-attachments/assets/a194ad23-3e4e-4129-bac8-8e507493ae1e" />

<img width="787" alt="Screenshot 2024-12-18 at 9 02 26 PM" src="https://github.com/user-attachments/assets/2fbc5b67-31ee-49b4-8ce6-4e1dd938cc3e" />

<img width="1512" alt="Screenshot 2024-12-18 at 9 03 33 PM" src="https://github.com/user-attachments/assets/0e5328d3-2d80-4788-843b-cba0336b24b0" />

<img width="761" alt="Screenshot 2024-12-18 at 9 03 56 PM" src="https://github.com/user-attachments/assets/bca115aa-3e98-4135-b08e-6726497c3bc6" />


<img width="761" alt="Screenshot 2024-12-18 at 9 04 12 PM" src="https://github.com/user-attachments/assets/4a6327e1-95d0-4039-b03e-a73599195fff" />

<img width="761" alt="Screenshot 2024-12-18 at 9 04 56 PM" src="https://github.com/user-attachments/assets/9d19f39d-f636-4e23-8a29-f247b6f63113" />

<p>- Restart and log back on (Should be automatic).</p> 
<br />
<br />
<h3 align="center">Create an Admin and Normal User Account in AD</h3>
<br />
<p>
<p>- Because we configured our virtual machine into a domain controller, we have to specify the domain when we log in. For example, Username: NicksDomain.com\Labuser2k</p>
<p>- Now we will create a Domain admin within the domain.</p>
<p>- start>Windows Administrative Tools>Active Directory Users and Computers.
<p>
</p>
<img width="550" alt="Screenshot 2024-12-18 at 9 13 01 PM" src="https://github.com/user-attachments/assets/5d7379c0-692d-4ac1-8343-346168d2ba87" />
<p>
</p>
<p>- -Create two OUs. One named _EMPLOYEES and one name _ADMINS.</p>

<img width="776" alt="Screenshot 2024-12-18 at 9 13 43 PM" src="https://github.com/user-attachments/assets/c82f0fca-9e09-4dc5-843c-b1f553ba8931" />


<img width="439" alt="Screenshot 2024-12-18 at 9 14 05 PM" src="https://github.com/user-attachments/assets/fc212bbf-9c18-4c11-a388-7b64069ede86" />
<p>- Create a user named "Jane Doe" within the _ADMINS folder, user and password can be anything.</p>
<p>
</p>

<img width="597" alt="Screenshot 2024-12-18 at 9 15 59 PM" src="https://github.com/user-attachments/assets/48e66ec6-ee7b-4dc4-b83b-22f4e1c8fb55" />

<img width="439" alt="Screenshot 2024-12-18 at 9 16 33 PM" src="https://github.com/user-attachments/assets/d9020062-b8ff-4df2-8ef9-d32a7b3ca531" />

<img width="438" alt="Screenshot 2024-12-18 at 9 16 51 PM" src="https://github.com/user-attachments/assets/398e9aa4-047d-4980-b33b-bea9d6118510" />

<p>- Add Jane Doe to the "Domain Admins" security group to make her an Admin. Then log off and log back in as Jane.</p>
<p>Right Click User > Properties> Add> Enter Domain Admins > Check Names, click ok.</p>

<p>
</p>

<img width="392" alt="Screenshot 2024-12-18 at 9 17 25 PM" src="https://github.com/user-attachments/assets/c6bea7b2-c883-45e8-903a-e4e3a6567453" />

<img width="414" alt="Screenshot 2024-12-18 at 9 17 35 PM" src="https://github.com/user-attachments/assets/067c2d2a-cf33-424c-9438-e2df6091f9ca" />

<img width="460" alt="Screenshot 2024-12-18 at 9 17 46 PM" src="https://github.com/user-attachments/assets/481af655-92b3-4232-ac2f-e1ade76e8342" />
<br />
<h3 align="center">Join Client-1 to your domain (myadproject.com)</h3>
<br />
<p>- From Client-1 log back in as our original account (labuser2k) and join it to the domain controller.</p>
<p>
</p>

<img width="395" alt="Screenshot 2024-12-18 at 9 22 05 PM" src="https://github.com/user-attachments/assets/9d9e25df-8473-49c5-a7e2-489f529da862" />

<img width="1512" alt="Screenshot 2024-12-18 at 9 22 23 PM" src="https://github.com/user-attachments/assets/1b38dbc0-4250-491d-a017-8a7f9e48868d" />

<img width="411" alt="Screenshot 2024-12-18 at 9 22 34 PM" src="https://github.com/user-attachments/assets/f52d375a-ade1-469c-bf52-a154e9fa72c6" />


<img width="320" alt="Screenshot 2024-12-18 at 9 22 53 PM" src="https://github.com/user-attachments/assets/b73198aa-9db3-46f6-a6ff-42d45f1360b8" />

<img width="456" alt="Screenshot 2024-12-18 at 9 23 41 PM" src="https://github.com/user-attachments/assets/9e92db6f-8759-4c57-82aa-f5819cc2d7b1" />
<p>  
</p>
  <p>- When you log back into DC-1, you will notice Client-1 now exists within the directory </p>
  <p>  
</p>

<img width="383" alt="Screenshot 2024-12-18 at 9 25 22 PM" src="https://github.com/user-attachments/assets/4441cb97-47f6-4198-ba6a-605610561095" />

<img width="652" alt="Screenshot 2024-12-18 at 9 25 41 PM" src="https://github.com/user-attachments/assets/7aa773ff-3738-420d-9614-9ab34f42d6b4" />
<h3 align="center">Setup Remote Desktop for non-administrative users on Client-1</h3>
<br />
<p>  
</p>
  <p>- After Client-1 restarts, log back in with the jane_admin account. We are going to set up Remote Desktop. </p>
  <p>  
</p>
<img width="384" alt="Screenshot 2024-12-18 at 9 33 30 PM" src="https://github.com/user-attachments/assets/6e015132-e8e1-41c7-a870-4a40a05233cc" />

<img width="169" alt="Screenshot 2024-12-18 at 9 33 40 PM" src="https://github.com/user-attachments/assets/3ca31955-a956-440f-ac42-373545a9aa96" />


<img width="380" alt="Screenshot 2024-12-18 at 9 33 53 PM" src="https://github.com/user-attachments/assets/6493917a-abc8-42ed-b661-1bbb464048dc" />

<img width="882" alt="Screenshot 2024-12-18 at 9 34 33 PM" src="https://github.com/user-attachments/assets/8fb4b5fe-b3f2-4e01-a72f-1cf4ea9eeacb" />
<br />
<h3 align="center">Create a bunch of additional users and attempt to log into client-1 with one of the users</h3>
<br />
<p>  
</p>
  <p>- Head back to DC-1 as Jane, and we will use a script to create 1000 users with random names to simulate what you might see in real life. Script:(https://github.com/Xinloiazn/configure-ad/blob/main/adscript.ps1) </p>
  <p>  
</p>

<img width="225" alt="Screenshot 2024-12-18 at 9 37 53 PM" src="https://github.com/user-attachments/assets/223b6198-6fc1-4b49-9583-54d6d36e1bc8" />

<img width="1348" alt="Screenshot 2024-12-18 at 9 39 12 PM" src="https://github.com/user-attachments/assets/bb04de31-733f-4e7b-8f8c-3254f096444e" />

<img width="792" alt="Screenshot 2024-12-18 at 9 40 23 PM" src="https://github.com/user-attachments/assets/bdb69e26-6b77-4f45-a083-a86a5b26408d" />
<p>  
</p>
  <p>- Once the script is running, we can test if the users work by logging in as them. We will pick a random user and login with a password we created earlier. </p>
  <p>  
</p>


<img width="1007" alt="Screenshot 2024-12-18 at 9 40 35 PM" src="https://github.com/user-attachments/assets/e55a1b64-a6a7-4177-8354-6ac7e898f381" />

<img width="1511" alt="Screenshot 2024-12-18 at 9 41 04 PM" src="https://github.com/user-attachments/assets/002f33dc-0fd4-44cb-bff0-bd204f1838b9" />

<img width="391" alt="Screenshot 2024-12-18 at 9 44 15 PM" src="https://github.com/user-attachments/assets/6319d6ad-c2d6-4531-b2ec-6135c1b7d17a" />

<img width="439" alt="Screenshot 2024-12-18 at 9 44 59 PM" src="https://github.com/user-attachments/assets/05a1cfdd-cd24-4d99-a1a8-dee8e717671d" />
<p>  
</p>
  <p>- The next thing we can test is Account lockouts, but it needs to be configured first. </p>
  <p>- We will open Group Policy Management in DC-1.
  </p>  
<p>
</p>

<img width="269" alt="Screenshot 2024-12-18 at 9 52 46 PM" src="https://github.com/user-attachments/assets/9bbb3ad8-d127-47f3-b6e4-1a546be355b4" />

<img width="400" alt="Screenshot 2024-12-18 at 9 53 01 PM" src="https://github.com/user-attachments/assets/8dd6ceb9-374d-48da-9d57-c4e64122c6d7" />
<p>  
</p>
  <p>- Group Policy Objects > Default Domain Policy > Right Click and edit. </p>
  <p>  
</p>

<img width="1512" alt="Screenshot 2024-12-18 at 9 54 22 PM" src="https://github.com/user-attachments/assets/ef38d848-9b8b-425c-8f29-981cacfddc01" />

<img width="288" alt="Screenshot 2024-12-18 at 9 54 31 PM" src="https://github.com/user-attachments/assets/338364cc-14f4-4c14-b4aa-87b5bf12818c" />

<p>  
</p>
  <p>- In the editor, go to Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy > Account Lockout Duration > Select any amount of time you want. Note that changing this will automatically change the other policies.
 </p>
  <p>  
</p>
<img width="786" alt="Screenshot 2024-12-18 at 9 55 04 PM" src="https://github.com/user-attachments/assets/ddc91359-26eb-41b5-94c1-a308aa234df1" />

<img width="963" alt="Screenshot 2024-12-18 at 9 58 37 PM" src="https://github.com/user-attachments/assets/332910b6-bf88-46fb-96c3-ec62ec546610" />
<p>  
</p>
  <p>- Once this is finished, we will pick a random account and enter the incorrect login 5 times. You will notice it locks you out. </p>
  <p>- From here, go back to DC-1 and unlock the users account via properties.</p> 
</p>


<img width="265" alt="Screenshot 2024-12-18 at 10 03 46 PM" src="https://github.com/user-attachments/assets/0677fe43-12d6-46bd-a80c-b2dc3a0cb89d" />

<img width="326" alt="Screenshot 2024-12-18 at 10 05 34 PM" src="https://github.com/user-attachments/assets/2a5368dd-0fb4-4a2a-8578-d399bf2703dd" />

<img width="415" alt="Screenshot 2024-12-18 at 10 05 41 PM" src="https://github.com/user-attachments/assets/8ab4fef5-ec66-4c9c-96a4-7b9a2401f60b" />

<img width="326" alt="Screenshot 2024-12-18 at 10 05 34 PM" src="https://github.com/user-attachments/assets/88224f15-07f1-4f25-945c-eef0c5fc6c41" />

<img width="415" alt="Screenshot 2024-12-18 at 10 05 41 PM" src="https://github.com/user-attachments/assets/b877b052-1916-4cf1-a0db-b82177c4cec4" />

<img width="378" alt="Screenshot 2024-12-18 at 10 08 50 PM" src="https://github.com/user-attachments/assets/7066fd45-fb6d-485a-8bc2-429221f6ea48" />

<img width="533" alt="Screenshot 2024-12-18 at 10 08 38 PM" src="https://github.com/user-attachments/assets/29a2af9b-36a8-4927-b9f4-7b704587009f" />




































