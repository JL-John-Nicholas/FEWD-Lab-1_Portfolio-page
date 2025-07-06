## Experiment 1:
Establish an AWS account. Use the AWS Management Console to launch an EC2 instance and connect to it from PUTTY.
Note: Create Instance name as External_Rollno, PEM File as External_Rollno
1. Write a Java code for Fibonacci series in AWS
2. Print Fibonacci series in Putty Console.

Steps in detail:

# Establish an AWS account, launch EC2 instance, install Java 17, and print Fibonacci series

## Agenda

* Establish AWS account
* Launch EC2 instance (Ubuntu)
* Connect via PuTTY
* Install Java 17
* Write and run Fibonacci program

---

## Step 1: Establish AWS account

1. Go to [https://aws.amazon.com/](https://aws.amazon.com/).
2. Click **Create an AWS Account**.
3. Enter your email, password, and billing details.
4. Complete phone verification (OTP).
5. Log in to **AWS Management Console**.

---

## Step 2: Launch EC2 instance

1. Go to **EC2 Dashboard** → Click **Launch Instance**.
2. Enter:

   * **Name**: `External_Rollno`
3. Select:

   * **AMI**: `Ubuntu Server 22.04 LTS (HVM), SSD Volume Type`
   * **Instance type**: `t2.micro` (Free Tier)
4. **Key Pair (login)**:

   * Click **Create new key pair**
   * Name: `External_Rollno`
   * Type: `.pem`
   * Click **Create key pair** and download the PEM file.
5. **Network settings**:

   * Ensure **SSH (port 22)** is allowed from your IP (or `0.0.0.0/0` for testing).
6. Click **Launch Instance**.

---

## Step 3: Convert PEM to PPK for PuTTY

1. Open **PuTTYgen**.
2. Click **Load**, select `All Files (*.*)` and open `External_Rollno.pem`.
3. Click **Save private key**, name it `External_Rollno.ppk`.

---

## Step 4: Connect to EC2 via PuTTY

1. Open **PuTTY**.
2. In **Host Name**, enter:

   ```
   ubuntu@<public-ip>
   ```

   (Replace `<public-ip>` with your instance’s Public IPv4 from EC2 console).
3. Go to **Connection → SSH → Auth** → browse and select `External_Rollno.ppk`.
4. Click **Open**, accept the security alert.

---

## Step 5: Install Java 17 on Ubuntu

1. Run the following commands to install OpenJDK 17:

   ```
   sudo apt update
   sudo apt install openjdk-17-jdk -y
   ```
2. Verify installation:

   ```
   java -version
   javac -version
   ```

   Should display:

   ```
   openjdk version "17..."
   ```

---

## Step 6: Write Java program for Fibonacci series

1. Create a new file:

   ```
   nano Fibonacci.java
   ```
2. Paste the following code:

   ```java
   public class Fibonacci {
       public static void main(String[] args) {
           int n = 10, first = 0, second = 1;
           System.out.print("Fibonacci Series up to " + n + ": ");
           for (int i = 1; i <= n; ++i) {
               System.out.print(first + " ");
               int next = first + second;
               first = second;
               second = next;
           }
       }
   }
   ```
3. Press `Ctrl+O` → `Enter` to save, then `Ctrl+X` to exit.

---

## Step 7: Compile and run the Java program

1. Compile the program:

   ```
   javac Fibonacci.java
   ```
2. Run it:

   ```
   java Fibonacci
   ```
3. You should see:

   ```
   Fibonacci Series up to 10: 0 1 1 2 3 5 8 13 21 34
   ```

---

# Experiment 2
# Implement and Manage S3 Bucket Versioning in AWS

## AIM:

* Create an S3 bucket with versioning.
* Upload multiple versions of `File1.jpeg`.
* Manage permissions, test URL access.
* Delete and restore specific versions.

---

## Step 1: Login into AWS

1. Open your browser → go to [https://aws.amazon.com/](https://aws.amazon.com/).
2. Click **Sign In to the Console**.
3. Enter your **IAM user / root credentials**.

---

## Step 2: Start Lab by Searching S3

1. In **AWS Console Home**, use the top search bar.
2. Type `S3`.
3. Click on **S3 Scalable Storage in the Cloud**.

---

## Step 3: Create S3 Bucket

### a. Create bucket

1. Click **Create bucket**.
2. Fill in **General configuration**:

   * **Bucket Name**: `Name-Rollno` (e.g. `John-22BD1234`).
3. Uncheck **Block All Public Access**:
4. **Bucket Versioning**:

   * Click **Enable** to turn on versioning at creation.
5. Keep **Tags & Advanced settings** default.
6. Click **Create bucket**.

---

## Step 4: Verify S3 Bucket Creation

* Bucket appears in your S3 list.
* Click on it → you’ll see it empty.

---

## Step 5: Upload First Image (File1.jpeg)

### a. Upload object

1. Inside your bucket → click **Upload**.
2. Click **Add files** → select `File1.jpeg` that contains:

   * **KMIT logo + Your Name & Roll No**.
3. Click **Upload** (will use default private ACL).

---

## Step 6: Enable Public Access to Bucket

### a. Modify Block Public Access

1. Go to **Permissions tab**.
2. Scroll to **Block Public Access**.
3. Click **Edit**.
4. Uncheck `Block all public access` and related checkboxes.
5. Click **Save changes** and confirm.

---

## Step 7: Add Bucket Policy for Public Read

1. Go to **Permissions tab → Bucket Policy → Edit**.
2. Paste this JSON policy (replace `Name-Rollno`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion"
      ],
      "Resource": "arn:aws:s3:::external-rollno-exp2/*"
    }
  ]
}
```

3. Click **Save changes**.

---

## Step 8: Test Public Access of File1.jpeg

1. Go to **Objects**, click on `File1.jpeg`.
2. Copy **Object URL**, e.g.:

```
https://Name-Rollno.s3.amazonaws.com/File1.jpeg
```

3. Open it in a new browser tab.
4. You should see the image **publicly visible**.

---

## Step 9: Upload New Versions of File1.jpeg

Because versioning is enabled, each upload creates a new version.

### a. Upload 2nd image

1. Modify `File1.jpeg` to include:

   * **KMIT logo + KMIT full form & location + Your Name & Roll No**.
2. Upload using **same file name** (`File1.jpeg`).

### b. Upload 3rd image

1. Modify to include:

   * **KMIT + NGIT logos + Your Name & Roll No**.
2. Upload same file name.

### c. Upload 4th image

1. Modify to include:

   * **KMIT, NGIT, KMEC logos + Your Name & Roll No**.
2. Upload same file name.

### d. Upload 5th image

1. Modify to include:

   * **KMIT, NGIT, KMEC, KMCES logos + Your Name & Roll No**.
2. Upload same file name.

---

## Step 10: Display Latest File

1. Copy the **Object URL** again.
2. Open in browser — it will display the **latest uploaded image**, i.e. with:

   ```
   KMIT, NGIT, KMEC, KMCES logos + Your Name & Roll No
   ```

---

## Step 11: Retrieve Older Version

1. Go to your bucket → click on `File1.jpeg`.
2. Click **Versions** tab.
3. You’ll see multiple versions listed by **Version IDs**.
4. Find the version corresponding to your **3rd upload** (KMIT + NGIT logos).
5. Click on the **3 dots** → choose **Download** to see locally.

### a. Make this old version the current latest

* Re-upload that downloaded file as `File1.jpeg` to make it the new current version.

---

## Step 12: Delete a Specific Image Version

1. In **Objects → File1.jpeg → Versions**, select the latest version (or any version you want).
2. Click **Delete** → confirm.
3. This adds a **Delete Marker** if you delete the latest version.

---

## Step 13: Restore Deleted Image

1. Go to **File1.jpeg → Versions**.
2. Find the **Delete Marker** at the top.
3. Select it → click **Delete**.
4. This removes the marker, making the previous version visible again.

---

## Step 14: Test the Restored Image

* Open the same **Object URL** again.
* It will now show the restored older image.

---

## Explanation: How S3 maintains versions

* **Versioning keeps all changes** to an object.
* Each upload creates a **new version with a unique Version ID**.
* **Deletes** do not erase data — they just add a **Delete Marker**, hiding the file from default view.
* You can **restore** by removing the delete marker or **access any older version by Version ID**.
* Only if the **bucket is permanently deleted**, all versions are lost forever.

---

✅ **Done:**

* Bucket `Name-Rollno` created with versioning.
* Multiple versions of `File1.jpeg` uploaded and tested.
* Demonstrated deletion and restoration using S3 versioning.

---

## Experiment 3:
Creating EFS & Mounting EC2
Create 2 Instance namely External_KMIT_Rollno, External_NGIT_Rollno
Creating EFS System &  Mount
Communicate with two servers (Server 1 File as to display in Server 2)

Steps in detail:

## Step 1: Launch Two EC2 Instances

1. Go to **EC2 Dashboard** → **Launch Instances**.
2. Create two EC2 instances:

   * **Instance-1 Name**: `Rollno-KMIT`
   * **Instance-2 Name**: `Rollno-NGIT`
3. Use the **same Key Pair (PEM file)** for both.
4. Keep **default settings** (8 GiB).
5. Ensure both instances are in the **same VPC and region**.
6. Click **Launch Instances**.

---

## Step 2: Create a Security Group for EFS

1. Go to **EC2 Dashboard → Security Groups** → **Create Security Group**.
2. Enter:

   * **Name**: `Rollno_EFS`
   * **Description**: `EFS SG`
   * **VPC**: Default (or the same VPC as EC2)
3. Add **Inbound Rules**:

   * **Type**: NFS
   * **Port Range**: `2049`
   * **Source**: Custom
   * Select security groups of both `Rollno-KMIT` and `Rollno-NGIT` instances.
4. Click **Create Security Group**.

---

## Step 3: Create an EFS File System

1. Go to **AWS Console** → search **EFS** → **Elastic File System**.
2. Click **Create File System**.
3. Enter details:

   * **Name**: `Rollno`
   * **VPC**: Same as EC2 instances
   * **Lifecycle**: Transition to IA after 7 days
   * **Throughput Mode**: Elastic
   * **Security Group**: Select `Rollno_EFS`
4. Click **Create**.

---

## Step 4: Attach EFS to EC2 Instances

1. In **EFS Dashboard → File Systems**, click on the created file system `Rollno`.
2. Click **Attach**.
3. Under **Mount via NFS client**, copy the mount command (looks like):

   ```
   sudo mount -t efs fs-xxxx:/ /mnt/efs
   ```

---

## Step 5: Mount EFS on Instance-1 (Rollno-KMIT)

1. Connect to `Rollno-KMIT` via **PuTTY** or **SSH**.
2. Install EFS utilities:

   ```
   sudo yum install -y amazon-efs-utils
   ```
3. Create mount directory:

   ```
   sudo mkdir /mnt/efs
   ```
4. Mount EFS:

   ```
   sudo mount -t efs fs-xxxx:/ /mnt/efs
   ```
5. Verify:

   ```
   df -h
   ```

---

## Step 6: Mount EFS on Instance-2 (Rollno-NGIT)

1. Connect to `Rollno-NGIT` via **PuTTY** or **SSH**.
2. Install EFS utilities:

   ```
   sudo yum install -y amazon-efs-utils
   ```
3. Create mount directory:

   ```
   sudo mkdir /mnt/efs
   ```
4. Mount EFS (same command as above):

   ```
   sudo mount -t efs fs-xxxx:/ /mnt/efs
   ```
5. Verify:

   ```
   df -h
   ```

---

## Step 7: Verify Communication Between Instances

1. On `Rollno-KMIT` (Instance-1):

   ```
   echo "<h1>Hello from Rollno-KMIT</h1>" | sudo tee /mnt/efs/File1.html
   ```
2. On `Rollno-NGIT` (Instance-2):

   ```
   cat /mnt/efs/File1.html
   ```
3. Expected Output:

   ```
   <h1>Hello from Rollno-KMIT</h1>
   ```

---


## Experiment 4:
Create your First AWS S3 Bucket and Upload Content to Bucket and Manage their Access and Create Static Website using AWS S3
1. Create a Bucket as External_Rollno
2. Create a React Project 
Text Box: You need to Enter Username
Button:  While clicking this button, you need to display message as 
      Output: “Hi ‘Name!!!’ Welcome to S3 Bucket Lab External”
3.  Provide Permission &  Policy generator
4. Get the Public URL to access Static Web Page

Steps in detail:

## Step 1: Login into AWS

1. Open [https://aws.amazon.com/](https://aws.amazon.com/) in your browser.
2. Click **Sign In to the Console**.
3. Enter your **IAM user / root account credentials** to log in.

---

## Step 2: Search for S3 Service

1. In the **AWS Console Home**, use the top **search bar**.
2. Type `S3` and press Enter.
3. Click on **S3 Scalable Storage in the Cloud**.

* **Advantage:** Store and retrieve any amount of data from anywhere.

---

## Step 3: Create an S3 Bucket

### a. General configuration

1. Click **Create bucket**.
2. Fill in:

   * **Bucket Name:** `RollNumber-S3` (example: `22bd1a056t-s3`)

     * Note: Follow naming rules (no capitals, no `_`, no spaces).

### b. Object Ownership

* Keep default ownership settings.

### c. Block Public Access

* Keep default (enabled).

### d. Bucket Versioning

* Click to **Enable**.

### e. Tags

* Keep default (0).

### f. Default Encryption

* Encryption type: **Server-side encryption with Amazon S3 managed keys (SSE-S3)**.
* Bucket Key: **Enable**.

### g. Advanced settings

* Keep default.

### h. Create bucket

* Finally, click **Create bucket**.

---

## Step 4: Verify Bucket Creation

* You will see a success message and your bucket listed.
* Click **View Details**.

---

## Step 5: Upload First File

### a. Click Upload

1. In the bucket details, click **Upload**.

### b. Upload your React App

#### Project TASK:

* Create a **ReactJS file** before uploading.

---

## Step 6: Build your React App

### a. Create the App

1. Open terminal / CMD on your local machine.
2. Run:

   ```
   npx create-react-app rollNumber-app
   cd rollNumber-app
   ```

### b. Create Component

1. Inside `src` folder, create a file `Welcome.js`.
2. Paste below code:

```javascript
import React, { useState } from 'react';

const WelcomeMessage = () => {
  const [name, setName] = useState('John');
  const [showMessage, setShowMessage] = useState(false);

  const handleButtonClick = () => {
    setShowMessage(true);
  };

  return (
    <div style={{ textAlign: "center", marginTop: "20px" }}>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter your name"
      />
      <button onClick={handleButtonClick}>Show Welcome Message</button>
      {showMessage && <p>Hi "{name}!!!" Welcome to S3 bucket</p>}
    </div>
  );
};

export default WelcomeMessage;
```

### c. Use it in App.js

```javascript
import WelcomeMessage from "./Welcome";

function App() {
  return (
    <div className="App">
      <WelcomeMessage />
    </div>
  );
}

export default App;
```

### d. Run and Build

* Run locally:

  ```
  npm start
  ```
* Then build for production:

  ```
  npm run build
  ```
* This creates a **`build` folder**.

---

## Step 7: Upload Build Files to S3

1. Back in AWS Console → open your S3 bucket.
2. Click **Upload**.
3. **Drag and drop all contents inside the `build` folder** (not the folder itself).
4. Click **Upload**.

---

## Step 8: Enable Static Website Hosting

1. Go to **Properties** tab → scroll down to **Static website hosting**.
2. Click **Edit**, then:

   * Enable **Static website hosting**.
   * **Hosting type:** Host a static website.
   * **Index document:** `index.html`.
3. Click **Save changes**.

---

## Step 9: Allow Public Access

### a. Disable Block All Public Access

1. Go to **Permissions tab**.
2. Under **Block public access**, click **Edit**.
3. Uncheck all options to disable block public access.
4. Click **Save changes**, type `confirm` when prompted.

---

## Step 10: Set Bucket Policy for Public Read

1. Go to **Bucket Policy → Edit**.
2. Click on **Policy Generator** to help you.

   * Select **Policy Type:** S3 Bucket Policy.
   * Principal: `*`
   * Actions: `GetObject`
   * ARN: Copy from your bucket details like `arn:aws:s3:::22bd1a056t-s3`.
3. Click **Add Statement** → **Generate Policy**.
4. Copy the generated JSON.

### b. Modify and Paste

1. Paste it in Bucket Policy editor.
2. Modify `"Resource"` to:

   ```
   "Resource": "arn:aws:s3:::22bd1a056t-s3/*"
   ```
3. Click **Save changes**.

---

## Step 11: Test Your Static Website

1. Go to **Properties tab → Static website hosting**.
2. Copy the **Bucket website endpoint**, e.g.:

```
http://22bd1a056t-s3.s3-website-us-east-1.amazonaws.com/
```

3. Open it in your browser. You should see:

* A text box to enter your name.
* Clicking the button shows:

```
Hi "<Name>!!!" Welcome to S3 bucket
```

---

✅ **Done:**
You have successfully:

* Created S3 bucket `RollNumber-S3` with versioning and encryption.
* Uploaded your **React app build files**.
* Configured **static website hosting**.
* Set **permissions & bucket policy** for public access.
* Accessed your hosted website via S3 static endpoint.

---

## Experiment 5
Create Extra EBS Attach in Region1 & insert files. Finally take Snapshot & attach EBS in another Region2 EBS and display files in Region2
1. Create a EC2 with External_Rollno
2. Create an Extra Volume of 10GB
3. Attach and Mount Extra EBS(Amazon Elastic Block Store) Volume to Linux EC2 in AWS
4. Inserting a Files in EBS, Taking a Snapshot & Attaching to another Region EBS
5. Display the Extra Volume with Data in another Region
Note: You need take name as External_Rollno for EC2, EBS, EC2-another Region etc

Steps in detail:

## Part-1: Attach and Mount Extra EBS (Amazon Elastic Block Store) Volume to Linux EC2 in AWS

---

### Step 1: Access AWS Academy and Start the Lab

1. Log in to **AWS Academy**.
2. Navigate to **Launch AWS Academy Learner Lab** and start the lab.
3. Click the **AWS Button** to activate your session.

---

### Step 2: Add an Extra EBS Volume (10GB) to an Existing EC2 Instance

1. Open the **EC2 Dashboard** in the AWS Management Console.
2. Locate and select your EC2 instance (linked to your Roll Number).
3. Click on the **Storage tab** to view existing volumes.

---

### Step 3: Create a New 10GB EBS Volume

1. In the left-hand menu, go to **Elastic Block Store → Volumes**.
2. Click **Create Volume**.
3. Configure the new volume:

   * **Volume Type:** General Purpose SSD (gp2)
   * **Size:** 10 GiB
   * **Availability Zone:** Must match your EC2 instance’s availability zone (e.g., `us-east-1b`).
   * **Tags (optional):**

     * Key: RollNo
     * Value: Extra-Volume-RollNo
4. Click **Create Volume**.

---

### Step 4: Attach the Newly Created Volume

1. In **EBS Volumes**, select your new volume.
2. Click **Actions → Attach Volume**.
3. Choose your EC2 instance.
4. Set **Device Name:** `/dev/sdf`.
5. Click **Attach Volume**.
6. Verify the volume is in `in-use` state.

---

### Step 5: Connect & Prepare the Volume

1. Go to **EC2 Dashboard → Instances**, select your instance, and click **Connect → EC2 Instance Connect**.
2. Switch to root:

   ```
   sudo su
   ```
3. List all block devices:

   ```
   lsblk
   ```
4. Check if filesystem exists on the new volume:

   ```
   file -s /dev/xvdf
   ```

   * If it shows `data`, the volume is empty.

---

### Step 6: Format the Volume

1. Format with XFS file system:

   ```
   mkfs -t xfs /dev/xvdf
   ```
2. Verify filesystem:

   ```
   file -s /dev/xvdf
   ```

---

### Step 7: Mount the Volume

1. Create mount point:

   ```
   mkdir /Rollno
   ```
2. Mount the volume:

   ```
   mount /dev/xvdf /Rollno
   ```
3. Verify mount:

   ```
   df -h
   ```

---

### Step 8: Create and Verify Files

1. Create a file:

   ```
   cd /Rollno
   vi 1.txt
   ```
2. Inside `vi`, press `i` to insert, type your Roll Number, then:

   * Press `Esc`, type `:wq` to save & exit.
3. List contents:

   ```
   ls -lart /Rollno
   ```
4. Check total size:

   ```
   df -h /Rollno
   ```

---

## Part-2: Creating Files in EBS, Taking Snapshot & Attaching to Another Region

---

### Step 1: Create More Files

1. Go to mounted directory:

   ```
   cd /Rollno
   ```
2. Verify location:

   ```
   pwd
   ```
3. Create additional files:

   * Python file:

     ```
     echo 'print(10+20)' > 2.py
     ```
   * Java file:

     ```
     echo 'class Main { public static void main(String[] args) { System.out.println("Welcome RollNo"); } }' > 3.java
     ```
4. Verify:

   ```
   ls -l
   ```

---

### Step 2: Import Key Pair into New Region

1. In AWS Console, switch to a different region (top-right corner, e.g. `US West (Oregon)`).
2. Go to **EC2 Dashboard → Key Pairs → Import Key Pair**.
3. Give name: `my-key-new-region`.
4. Open folder where your .pem key pair is there and open it in CMD
5. Execute this command ssh-keygen -y -f my-key.pem > my-key.pub
6. Open the .pub file in Notepad copy content and paste

---

### Step 3: Launch EC2 in New Region

1. Go to **EC2 Dashboard (new region)**.
2. Launch new instance:

   * **Name:** `Rollno_EBS_OtherRegion`
   * **Key Pair:** `my-key-new-region`
   * **Storage:** 8 GiB (default)
3. Click **Launch Instance**.
4. Connect using SSH:

   ```
   ssh -i your-key.pem ec2-user@<new-instance-public-ip>
   ```
5. Verify disk:

   ```
   sudo fdisk -l
   ```

---

### Step 4: Create Snapshot from Old Region

1. In **old region EC2 Dashboard**, go to **Elastic Block Store → Volumes**.
2. Select the 10 GiB volume, click **Actions → Create Snapshot**.
3. Provide description: `Snapshot_Rollno`.
4. Click **Create Snapshot**.
5. Go to **Snapshots** and wait until status is `available`.

---

### Step 5: Copy Snapshot to New Region

1. In **old region Snapshots**, select the snapshot.
2. Click **Actions → Copy Snapshot**.
3. Set **Destination Region:** your new region.
4. Click **Copy**.
5. Go to **Snapshots in new region**, wait for status `available`.

---

### Step 6: Create Volume from Snapshot & Attach

1. In new region, under **Snapshots**, select snapshot → **Actions → Create Volume**.
2. Ensure same availability zone as new EC2 instance.
3. Click **Create Volume**.
4. In **Volumes**, select new volume → **Actions → Attach Volume**.
5. Choose new EC2 instance, set **Device Name:** `/dev/sdf`, and **Attach**.

---

### Step 7: Mount in New EC2 Instance

1. SSH into the new instance.
2. Check devices:

   ```
   lsblk
   ```
3. Create mount point and mount:

   ```
   sudo mkdir /Rollno
   sudo mount /dev/xvdf /Rollno
   ```
4. List contents:

   ```
   ls -l /Rollno
   ```

   * You should see `1.txt`, `2.py`, `3.java`.

---

✅ **Done:**
You have successfully:

* Attached EBS volume to original EC2.
* Created files, taken snapshot, copied snapshot to another region.
* Created a new volume from snapshot, attached to new EC2, and accessed data.

---

### Experiment 6:
1. Create a React Project.
React Project Contains:  
Text Box: You need to Enter Username
Button:  While clicking this button, you need to display message as 
      Output: “Hi ‘Name!!!’ Welcome to S3 Bucket Lab External”

2. Clone in AWS with git or manual method
3. Write & Build Docker Image in AWS
4. Get the public url for project
Note: EC2, Docker Build should contain your Roll No.

Steps in detail:

### Step 1: Create a React App

```bash
npx create-react-app rollnumber-app
```

✅ Sets up React development environment with `package.json`, `node_modules`, `src/`.

---

### Step 2: Move into Project Directory

```bash
cd rollnumber-app
```

✅ All project work will be done inside this directory.

---

### Step 3: Create Welcome Component

Inside `src/`, create `welcome.js` file with:

```javascript
import React, { useState } from 'react';

const WelcomeMessage = () => {
  const [name, setName] = useState('John');
  const [showMessage, setShowMessage] = useState(false);

  const handleButtonClick = () => {
    setShowMessage(true);
  };

  return (
    <div style={{ textAlign: "center", marginTop: "20px" }}>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter your name"
      />
      <button onClick={handleButtonClick}>Show Welcome Message</button>
      {showMessage && <p>Hi "{name}!!!" Welcome to S3 bucket</p>}
    </div>
  );
};

export default WelcomeMessage;
```

---

### Step 4: Edit `App.js`

Import and use your component:

```javascript
import React from 'react';
import WelcomeMessage from './welcome';

function App() {
  return (
    <div className="App">
      <WelcomeMessage />
    </div>
  );
}

export default App;
```

---

### Step 5: Test Locally

```bash
npm start
```

✅ Opens on `http://localhost:3000`.

---

## 🐳 Part 2: Dockerize React App

---

### Step 6: Create Dockerfile

Create `Dockerfile` in the root folder:

```dockerfile
FROM node:16.17-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
RUN npm install -g serve
EXPOSE 3000
CMD ["serve", "-s", "build", "-l", "3000"]
```

✅ Uses `serve` to run the built static app on port `3000`.

---

### Step 7: Create `.dockerignore`

```bash
nano .dockerignore
```

Add:

```
node_modules
.dockerignore
Dockerfile
```

✅ Keeps image smaller by ignoring local modules.

---

## 🗂 Part 3: Push to GitHub

---

### Step 8: Initialize Git Repo

```bash
git init
git add .
git commit -m "first commit"
```

---

### Step 9: Push to GitHub

```bash
git remote add origin https://github.com/<username>/<repo>.git
git branch -M main
git push -u origin main
```

✅ Your code is now on GitHub for later cloning.

---

## ☁ Part 4: Setup AWS EC2

---

### Step 10: Launch EC2

* **AMI:** Ubuntu Server (or Amazon Linux 2).
* **Instance type:** t2.micro
* **Ports:**

  * 22 (SSH)
  * 3000 (React app)

---

### Step 11: Connect via SSH

```bash
ssh -i "your-key.pem" ubuntu@<ec2-public-ip>
```

✅ Now inside your EC2 server.

---

## 🐳 Part 5: Install Docker on Ubuntu EC2

---

### Step 12: Install Docker

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
```

✅ Adds your user to `docker` group.

---

### Step 13: Refresh Docker Group

Run either:

```bash
newgrp docker
```

or log out & log in again to apply the group.

---

### Step 14: Verify Docker

```bash
docker --version
```

✅ Shows Docker is installed.

---

## 📝 Part 6: Pull Code from GitHub on EC2

---

### Step 15: Install Git

```bash
sudo apt install git -y
```

---

### Step 16: Clone Repo

```bash
git clone https://github.com/<username>/<repo>.git
cd rollnumber-app
```

✅ Your project files are now on EC2.

---

## 🐳 Part 7: Build & Run Docker Container

---

### Step 17: Build Docker Image

```bash
docker build -t rollno-react-app .
```

✅ Builds image using `Dockerfile`.

---

### Step 18: Run Docker Container

```bash
docker run -p 3000:3000 rollno-react-app
```

✅ Runs your app on EC2 port `3000`.

---

## 🌐 Part 8: Access in Browser

---

### Step 19: Test From Browser

Open on your laptop:

```
http://<ec2-public-ip>:3000
```

✅ Your custom React app is now live from EC2 via Docker.

---

### Experiment 7
Create a VPC Network & perform the following scenario
1. Create two Subnets namely Public_Rollno & Private_Rollno
2. Create Internet Gateways for step1
3. Create Routing Tables to access the network 
4. Create two Instance namely Public_Rollno & Private_Rollno
I.  Private Instance: Create a confidential data
II. Public Instance: Write a Business logic for java code to generate random numbers  
5. You need to Access Private network using Bastion Network
Finally Upload your Document with Screenshots & steps with Explanation

Steps in detail:

## Part 1: Create a VPC

---

### Step 1: Open AWS VPC

* Start AWS Lab.
* Go to AWS Management Console.
* In the search bar, type **VPC** and select it.

---

### Step 2: Navigate to VPC Dashboard

* Click **VPC**.
* You will see the **VPC Dashboard**.

---

### Step 3: Create a New VPC

* Click **Create VPC**.
* Configure:

  * **Resources to create:** Select.
  * **Name tag:** `Rollno-VPC`
  * **IPv4 CIDR:** `12.0.0.0/16`
  * **IPv6:** No IPv6
  * **Tenancy:** Default
* Tags:

  * Key: Your Name
  * Value: `Rollno-VPC`
* Click **Create VPC**.

✅ VPC created successfully.

---

## Part 2: Create an Internet Gateway (IGW)

---

### Step 4: Create and Attach IGW

* Go to **Internet Gateways** → Click **Create Internet Gateway**.
* Name: `Rollno-IG`.
* Click **Create Internet Gateway**.

✅ Now attach it:

* Select the IGW.
* Click **Actions → Attach to VPC**.
* Select `Rollno-VPC` → Click **Attach**.

---

## Part 3: Create Subnets

---

### Step 5: Create Subnets

* Go to **Subnets → Create Subnet**.
* Select your VPC: `Rollno-VPC`.

#### Create Public Subnet

* **Subnet name:** `Rollno-public-SB1`
* **AZ:** `us-east-1a`
* **CIDR:** `12.0.0.0/20`

#### Create Private Subnet

* Click **Add New Subnet**.
* **Subnet name:** `Rollno-private-SB2`
* **AZ:** `us-east-1a`
* **CIDR:** `12.0.16.0/20`

✅ Click **Create Subnet**.

---

## Part 4: Setup Route Tables

---

### Step 6: Create Public Route Table

* Go to **Route Tables → Create Route Table**.
* Name: `Rollno-Public-RT`
* VPC: `Rollno-VPC`
* Click **Create**.

---

### Provide Internet Access

1. Select `Rollno-Public-RT` → Go to **Routes → Edit Routes**.
2. Click **Add Route**:

   * **Destination:** `0.0.0.0/0`
   * **Target:** Select your **Internet Gateway**.
3. Click **Save Changes**.

---

### Associate Public Subnet

* Go to **Subnet Associations → Edit Subnet Associations**.
* Select `Rollno-public-SB1`.
* Click **Save**.

---

### Step 7: Create Private Route Table

* Click **Create Route Table**.
* Name: `Rollno-private-RT`.
* VPC: `Rollno-VPC`.
* Click **Create**.

---

### Associate Private Subnet

* Select `Rollno-private-RT`.
* Go to **Subnet Associations → Edit Subnet Associations**.
* Select `Rollno-private-SB2`.
* Click **Save**.

---

## Part 5: Launch EC2 in Public Subnet

---

### Step 8: Create EC2 Instance

* Go to **EC2 → Instances → Launch Instance**.
* Name: `EC1_Rollno_Public_subnet-Instance`
* **AMI:** Ubuntu
* **Key Pair:** (your PEM file)

#### Network Settings:

* **VPC:** `Rollno-VPC`
* **Subnet:** `Rollno-public-SB1`
* **Auto-assign Public IP:** Enabled.

#### Security Group:

* Name: `Public-Rollno-ec2-SG`
* Allow:

  * **SSH:** TCP, Port 22, Source `0.0.0.0/0`.

✅ Click **Launch Instance**.

---

### Step 9: SSH into Public Instance (Bastion Host)

* Copy the **public IPv4** of the instance.

```bash
ssh -i "your-key.pem" ubuntu@<public-ip>
```

✅ You are now inside your **bastion host**.

---

### Test Network

* Check private IP:

```bash
hostname -I
```

* Check public IP via Internet:

```bash
curl ifconfig.me
```

✅ Confirms IGW + route table working.

---

## Part 6: Launch EC2 in Private Subnet

---

### Step 10: Create EC2 in Private Subnet

* Go to **EC2 → Launch Instance**.
* Name: `EC2_Rollno_Private_subnet-Instance`.

#### Network Settings:

* **VPC:** `Rollno-VPC`
* **Subnet:** `Rollno-private-SB2`
* **Auto-assign Public IP:** Disabled.

#### Security Group:

* Name: `Private-Rollno-ec2-SG`
* Allow:

  * **SSH:** TCP, Port 22.
  * **Source:** Custom → Select **security group ID** of bastion EC2 (`Public-Rollno-ec2-SG`).

✅ Click **Launch Instance**.

---

## Part 7: Connect to Private EC2 via Bastion Host

---

### Step 11: SSH Jump from Public to Private

1. Already SSH’ed into **bastion host**.
2. Create PEM file inside bastion:

```bash
nano aws_privatekey_ec2.pem
```

* Paste contents of your local PEM.
* Save & exit.

3. Set permission:

```bash
chmod 400 aws_privatekey_ec2.pem
```

4. Connect to private EC2:

```bash
ssh -i aws_privatekey_ec2.pem ubuntu@<private-ip-of-private-EC2>
```

✅ You are now inside your **private subnet EC2 via bastion**.

---

### Experiment 8
SNS & SQS Application:
Create a Canteen Application which  contains following steps :
Owner: Accept the order, Give a Preparation waiting Time, Inform after handover to Delivery Boy via Email notification
Customer: Place the Order, Get Notifications from Owner via Email(order accepted, Food processing time, food ready alert, delivery boy handover)
Delivery Boy: Take the order and give notification to customer for picking parcel, after reaching destination give a  arrived notification & Finally send SMS for Feedback/Rating.
NOTE: Use SNS, SQS Service, the data to store in DynamoDB.

Steps in detail:

### Step 1: Create Orders Table

* Go to AWS Console → DynamoDB → Create Table
* Table Name: `Orders`
* Partition Key: `orderId` (String)
* Click Create
* After creation, add attributes:

  * `customerEmail` (String)
  * `status` (String)
  * `prepTime` (Number)
  * `deliveryBoyId` (String)

---

### Step 2: Create DeliveryStatus Table

* Table Name: `DeliveryStatus`
* Partition Key: `deliveryId` (String)
* Add attributes:

  * `orderId` (String)
  * `status` (String)

---

### Step 3: Create Users Table

* Table Name: `Users`
* Partition Key: `userId` (String)
* Add attributes:

  * `role` (String)
  * `email` (String)
  * `phone` (String)

---

## Part 2: Create SNS Topics

---

### Step 4: Create OrderStatusTopic

* Go to SNS → Create Topic
* Type: Standard
* Name: `OrderStatusTopic`
* Create subscriptions (Email) for customers.

---

### Step 5: Create DeliveryNotificationsTopic

* Name: `DeliveryNotificationsTopic`
* Add subscribers:

  * Email for delivery updates
  * SMS for quick notifications

---

### Step 6: Create FeedbackTopic

* Name: `FeedbackTopic`
* Add subscribers:

  * SMS for sending feedback requests

---

## Part 3: Create SQS Queues

---

### Step 7: Create OwnerQueue

* Go to SQS → Create Queue
* Name: `OwnerQueue`
* Type: Standard
* Used to notify owner when new order is placed.

---

### Step 8: Create DeliveryBoyQueue

* Name: `DeliveryBoyQueue`
* Type: Standard
* Used to notify delivery boy when food is ready.

---

## Part 4: Create Lambda Functions

---

### Step 9: Create placeOrderLambda

* Go to: AWS Console → Lambda → Create function
* Name: `placeOrderLambda`
* Runtime: Python 3.9
* Execution role: Attach `LambdaCanteenExecutionRole` (with DynamoDB, SQS, SNS permissions)
* Add Trigger: API Gateway (HTTP API → POST `/placeOrder`)

**Logic:**

* Generates a unique `orderId`
* Saves new order into `Orders` table with status `Placed`
* Sends message to `OwnerQueue` to notify the owner

**Code:**

```python
import boto3, json, uuid

def lambda_handler(event, context):
    orderId = str(uuid.uuid4())
    body = json.loads(event['body'])
    customerEmail = body['email']

    dynamo = boto3.resource('dynamodb')
    table = dynamo.Table('Orders')
    table.put_item(Item={
        'orderId': orderId,
        'customerEmail': customerEmail,
        'status': 'Placed'
    })

    sqs = boto3.client('sqs')
    sqs.send_message(
        QueueUrl='https://sqs.us-east-1.amazonaws.com/443369304872/OwnerQueue',
        MessageBody=json.dumps({'orderId': orderId})
    )

    return {'statusCode': 200, 'body': 'Order Placed'}
```

---

### Step 10: Create processOwnerQueueLambda

* Go to: AWS Console → Lambda → Create function
* Name: `processOwnerQueueLambda`
* Runtime: Python 3.9
* Execution role: Attach `LambdaCanteenExecutionRole`
* Add Trigger: SQS (OwnerQueue)

**Logic:**

* Reads new order from `OwnerQueue`
* Updates `Orders` table status to `Accepted` and sets `prepTime`
* Sends email to customer via `OrderStatusTopic`

**Code:**

```python
import boto3, json

def lambda_handler(event, context):
    dynamo = boto3.resource('dynamodb')
    table = dynamo.Table('Orders')
    sns = boto3.client('sns')

    for record in event['Records']:
        msg = json.loads(record['body'])
        orderId = msg['orderId']
        prepTime = 15

        table.update_item(
            Key={'orderId': orderId},
            UpdateExpression='SET #s = :s, prepTime = :p',
            ExpressionAttributeNames={'#s': 'status'},
            ExpressionAttributeValues={':s': 'Accepted', ':p': prepTime}
        )

        order = table.get_item(Key={'orderId': orderId})['Item']
        sns.publish(
            TopicArn='arn:aws:sns:us-east-1:443369304872:OrderStatusTopic',
            Message=f"Order accepted! Ready in {prepTime} minutes.",
            Subject="Order Accepted"
        )
```

---

### Step 11: Create foodReadyLambda

* Go to: AWS Console → Lambda → Create function
* Name: `foodReadyLambda`
* Runtime: Python 3.9
* Execution role: Attach `LambdaCanteenExecutionRole`
* Add Trigger: API Gateway (POST `/markReady`)

**Logic:**

* Updates order status to `Ready` in `Orders` table
* Sends email notification to customer
* Sends message to `DeliveryBoyQueue` to notify delivery boy

**Code:**

```python
import boto3
import json

def lambda_handler(event, context):
    try:
        body = json.loads(event['body'])
        orderId = body['orderId']

        dynamo = boto3.resource('dynamodb')
        table = dynamo.Table('Orders')
        table.update_item(
            Key={'orderId': orderId},
            UpdateExpression='SET #s = :s',
            ExpressionAttributeNames={'#s': 'status'},
            ExpressionAttributeValues={':s': 'Ready'}
        )

        sns = boto3.client('sns')
        sns.publish(
            TopicArn='arn:aws:sns:us-east-1:443369304872:OrderStatusTopic',
            Message="Your order is ready! Being handed to delivery boy.",
            Subject="Food Ready"
        )

        sqs = boto3.client('sqs')
        sqs.send_message(
            QueueUrl='https://sqs.us-east-1.amazonaws.com/443369304872/DeliveryBoyQueue',
            MessageBody=json.dumps({'orderId': orderId})
        )

        return {
            'statusCode': 200,
            'body': json.dumps('Marked as Ready')
        }

    except Exception as e:
        print("Error:", str(e))
        return {
            'statusCode': 500,
            'body': json.dumps(f"Internal Server Error: {str(e)}")
        }
```

---

### Step 12: Create processDeliveryQueueLambda

* Go to: AWS Console → Lambda → Create function
* Name: `processDeliveryQueueLambda`
* Runtime: Python 3.9
* Execution role: Attach `LambdaCanteenExecutionRole`
* Add Trigger: SQS (DeliveryBoyQueue)

**Logic:**

* Assigns `deliveryBoyId` to the order
* Notifies customer by publishing to `DeliveryNotificationsTopic` (Email + SMS)

**Code:**

```python
def lambda_handler(event, context):
    import boto3, json
    dynamo = boto3.resource('dynamodb')
    table = dynamo.Table('Orders')
    sns = boto3.client('sns')

    for record in event['Records']:
        msg = json.loads(record['body'])
        orderId = msg['orderId']
        deliveryBoyId = 'DELIVERY-123'  # Simulated assignment

        table.update_item(
            Key={'orderId': orderId},
            UpdateExpression='SET deliveryBoyId = :d',
            ExpressionAttributeValues={':d': deliveryBoyId}
        )

        sns.publish(
            TopicArn='arn:aws:sns:us-east-1:443369304872:DeliveryNotificationsTopic',
            Message=f"Delivery boy assigned and en route for order {orderId}",
            Subject="Delivery Started"
        )
```

---

### Step 13: Create deliveryBoyUpdatesLambda

* Go to: AWS Console → Lambda → Create function
* Name: `deliveryBoyUpdatesLambda`
* Runtime: Python 3.9
* Execution role: Attach `LambdaCanteenExecutionRole`
* Add Trigger: API Gateway (POST `/deliveryUpdate`)

**Logic:**

* Receives `orderId` from Delivery Boy’s app / frontend.
* Sends email + SMS to customer that delivery boy has arrived via `DeliveryNotificationsTopic`.
* Sends feedback SMS via `FeedbackTopic`.

---

**Code:**

```python
import boto3
import json

def lambda_handler(event, context):
    try:
        body = json.loads(event['body'])
        orderId = body['orderId']

        sns = boto3.client('sns')

        # Notify customer that delivery boy has arrived
        sns.publish(
            TopicArn='arn:aws:sns:us-east-1:443369304872:DeliveryNotificationsTopic',
            Message=f"Delivery boy has reached your location for order {orderId}.",
            Subject="Order Arrived"
        )

        # Send feedback request
        sns.publish(
            TopicArn='arn:aws:sns:us-east-1:443369304872:FeedbackTopic',
            Message="Thank you for ordering! Please share your feedback."
        )

        return {
            'statusCode': 200,
            'body': json.dumps('Customer notified')
        }

    except Exception as e:
        print("Error:", str(e))
        return {
            'statusCode': 500,
            'body': json.dumps(f"Internal Server Error: {str(e)}")
        }
```

---

## Part 5: Attach IAM Roles to Lambda Functions

---

### Step 14: Create IAM Role

* Go to IAM → Roles → Create Role
* Trusted entity: Lambda
* Attach policies:

  * `AmazonDynamoDBFullAccess`
  * `AmazonSQSFullAccess`
  * `AmazonSNSFullAccess`
  * `CloudWatchLogsFullAccess` (optional for logs)
* Name: `LambdaCanteenExecutionRole`

---

### Step 15: Attach Role to Each Lambda

* Go to Lambda → Select function → Configuration → Permissions
* Attach `LambdaCanteenExecutionRole` to each function.

---

# Part 6: Setup API Gateway

---

## Step 16: Create API Gateway & Integrate with Lambda

1. **Go to AWS Console → API Gateway**

2. **Create API**

   * Choose `HTTP API` (simpler and faster) or `REST API` (for advanced features).
   * For this example, `HTTP API` is recommended.

3. **Configure API**

   * Give it a name: `CanteenAPI`
   * Click `Next`.

4. **Add routes**

   | HTTP Method | Resource path   | Integration target       |
   | ----------- | --------------- | ------------------------ |
   | POST        | /placeOrder     | placeOrderLambda         |
   | POST        | /markReady      | foodReadyLambda          |
   | POST        | /deliveryUpdate | deliveryBoyUpdatesLambda |

   **To add:**

   * Click `Add Integration`.
   * Choose `Lambda`.
   * Select your Lambda function.
   * For each route, click `Attach integration`.

5. **Enable CORS**

   * In `Routes` page, select each route.
   * Click `CORS` → Enable default settings (allow all origins).

6. **Deploy API**

   * Click `Deployments` in left menu.
   * Click `Create`.
   * Stage name: `prod`.
   * Click `Deploy`.

7. **Copy your Invoke URL**

   * It will look like:

     ```
     https://j8yd5bn3d8.execute-api.us-east-1.amazonaws.com/prod
     ```

✅ Now your API Gateway is live and connected to your Lambda functions.

---

# Part 7: Testing the System

---

## Step 17: Test /placeOrder Endpoint

* **URL:**

  ```
  POST https://<api-id>.execute-api.<region>.amazonaws.com/prod/placeOrder
  ```

* **Body:**

  ```json
  {
    "email": "customer@example.com"
  }
  ```

* **Expected:**

  * HTTP `200 OK` with response `"Order Placed"`
  * New item created in `Orders` DynamoDB table with:

    * `status = Placed`
    * `customerEmail`
    * unique `orderId`
  * SQS message sent to `OwnerQueue`.

✅ Use `Postman` or `curl` to test:

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"email": "customer@example.com"}' \
  https://<api-id>.execute-api.<region>.amazonaws.com/prod/placeOrder
```

---

## Step 18: Test /markReady Endpoint

Go to AWS Console → Lambda → Select your foodReadyLambda

Click Configuration → General configuration → Edit

Set Timeout: 10 seconds (or even 30 seconds for testing).

* **URL:**

  ```
  POST https://<api-id>.execute-api.<region>.amazonaws.com/prod/markReady
  ```

* **Body:**

  ```json
  {
    "orderId": "<orderId-from-previous-step>"
  }
  ```

* **Expected:**

  * HTTP `200 OK` with response `"Marked as Ready"`
  * `Orders` table updates `status = Ready`.
  * SNS email sent to customer.
  * SQS message sent to `DeliveryBoyQueue`.

✅ Example `curl`:

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"orderId": "123e4567-e89b-12d3-a456-426614174000"}' \
  https://<api-id>.execute-api.<region>.amazonaws.com/prod/markReady
```

---

## Step 19: Test /deliveryUpdate Endpoint

* **URL:**

  ```
  POST https://<api-id>.execute-api.<region>.amazonaws.com/prod/deliveryUpdate
  ```

* **Body:**

  ```json
  {
    "orderId": "<orderId-from-previous-step>"
  }
  ```

* **Expected:**

  * HTTP `200 OK` with response `"Customer notified"`
  * SNS sends:

    * Email + SMS to customer (delivery boy has arrived).
    * SMS feedback request.

✅ Example `curl`:

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"orderId": "123e4567-e89b-12d3-a456-426614174000"}' \
  https://<api-id>.execute-api.<region>.amazonaws.com/prod/deliveryUpdate
```

---

# Summary

✅ You have now **successfully implemented a canteen management system using multiple AWS services:**

| Service     | Usage                                        |
| ----------- | -------------------------------------------- |
| DynamoDB    | Stores orders, users, delivery status data.  |
| SQS         | Queue system for Owner & Delivery Boy tasks. |
| SNS         | Sends Email & SMS notifications.             |
| Lambda      | Contains business logic to process orders.   |
| API Gateway | Provides HTTP API endpoints for frontend.    |

---

## Experiment 9
Design a Amazon LEX chatbot with following criteria:
You are need to build a Lex chatbot named GoldPriceBot_Rollno that can respond to user queries like: 
Input:     “What is the current price of gold?”
Output:  Todays Gold rate 24K is INR10,002/-
Integrate Gold API(Metals-API or GoldAPI) by using web scraping Method.
NOTE: Your chatbot must fetch real-time gold price data from a public API

Steps in detail:

## Part 1: Set up Lambda Layer

### Step 1: Set Up the Python Layer in CloudShell

1️⃣ Open AWS Console → Region: `eu-west-1 (Ireland)` (Lex works here).
2️⃣ Click the **CloudShell** icon (top right).
3️⃣ Run commands one by one:

```bash
mkdir my-layer
cd my-layer
mkdir python
pip install requests -t python/
pip install beautifulsoup4 -t python/
cd ..
cd my-layer
zip -r ../my-layer.zip python/
```

✅ Your zip file will look like:

```
my-layer.zip
└── python/
    ├── requests/
    └── bs4/
```

4️⃣ Download it to your system:

* Click **Actions → Download file → my-layer.zip**

---

## Part 2: Create Lambda Layer

### Step 2: Upload Layer

1️⃣ Go to **AWS Console → Lambda → Layers → Create layer**
2️⃣ Name: `requests-bs4-layer`
3️⃣ Upload `my-layer.zip`
4️⃣ Select **Compatible runtimes:** `Python 3.13`
5️⃣ Click **Create**

---

## Part 3: Create Lambda Function

### Step 3: Create Lambda Function

1️⃣ Go to **Lambda → Functions → Create function**
2️⃣ Select **Author from scratch**
3️⃣ Name: `GetGoldRates`
4️⃣ Runtime: `Python 3.13`
5️⃣ Click **Create function**

---

### Step 4: Attach Layer to Lambda

1️⃣ On function page → scroll to **Layers**
2️⃣ Click **Add a layer → Custom layers**
3️⃣ Choose `requests-bs4-layer`, Version: `1`
4️⃣ Click **Add**

---

### Step 5: Configure Memory & Timeout

1️⃣ Go to **Configuration tab → General configuration → Edit**
2️⃣ Set **Memory:** `512 MB`
3️⃣ Set **Timeout:** `45 sec`
4️⃣ Click **Save**

---

## Part 4: Deploy Web Scraping Code

### Step 6: Add Lambda Code

1️⃣ Go to **Code tab → replace content** with:

```python
import requests
from bs4 import BeautifulSoup

def lambda_handler(event, context):
    url = 'https://www.bankbazaar.com/gold-rate.html'
    headers = {"User-Agent": "Mozilla/5.0"}

    response = requests.get(url, headers=headers)
    soup = BeautifulSoup(response.content, 'html.parser')

    table = soup.find('table')
    hyderabad_rate_24k = None

    if table:
        rows = table.find_all('tr')
        for row in rows[1:]:
            cols = row.find_all('td')
            if len(cols) >= 3:
                city = cols[0].get_text(strip=True).lower()
                rate_24k = cols[2].get_text(strip=True).split('(')[0].strip()
                if city == 'hyderabad':
                    hyderabad_rate_24k = rate_24k
                    break

    if hyderabad_rate_24k:
        message = f"The current 24K gold rate in Hyderabad is {hyderabad_rate_24k}."
    else:
        message = "Sorry, I couldn’t find the gold rate for Hyderabad."

    return {
        "sessionState": {
            "dialogAction": {"type": "Close"},
            "intent": {"name": "GetGoldRate", "state": "Fulfilled"}
        },
        "messages": [{
            "contentType": "PlainText",
            "content": message
        }]
    }
```

---

### Step 7: Test Lambda

1️⃣ Click **Test → Configure test event:**

* Name: `goldtest`
* JSON: `{}` (empty)
  2️⃣ Click **Test → check output → should show gold rate message**

---

## Part 5: Create Lex Bot

### Step 8: Create Lex Bot

1️⃣ Go to **Amazon Lex → Create bot**
2️⃣ Bot Name: `GoldRateBot`
3️⃣ Runtime role: **Create new with basic Lex permissions**
4️⃣ COPPA: **No**
5️⃣ Language: `English (US)`
6️⃣ Click **Next → Done**

---

### Step 9: Create Intent

1️⃣ Go to **Intents tab → Add Intent → Add empty intent**
2️⃣ Name: `GetGoldRate`
3️⃣ Add sample utterances:

```
What is the gold rate?
Tell me Hyderabad gold price
Gold rate today
What is the gold price in Hyderabad?
```

4️⃣ Scroll to **Fulfillment → enable Lambda function**
5️⃣ Click **Save Intent**

---

### Step 10: Connect Lambda to Alias

1️⃣ In Lex left panel → click **Aliases → TestBotAlias**
2️⃣ Under **Languages → English → Lambda function** → select `GetGoldRates`
3️⃣ Click **Save**

---

## Part 6: Build & Test

### Step 11: Build and Test the Bot

1️⃣ Go to **Intents page → click Build**
2️⃣ After success, click **Test**
3️⃣ Try phrases like:

```
What is the gold rate?
```

✅ It will call Lambda → scrape the gold price → respond via Lex.

---

## Experiment 10
Create a CloudFront Feedback Application, Where the user has to provide valuable feedback.
1. Reactjs: User as to enter roll no(primary key), name, subjects(CC, FEWD, NN, IOT) & Rating; Finally push the project into S3 bucket with versioning enable
2. Lambda: Run REST APIs on ExpressJS Server; Insert feedback with POST Method  
3.  DynamoDB: Capture Feedback in DB
4. Get DNS of Cloud front Domain serve feedback application.
NOTE: Cloud front involves ReactJS+ Lambda + DynamoDB + S3 versioning

Steps in detail:

# Part 1: Set Up AWS Learner Lab

### Step 1: Start Your Lab

1. Go to **[Qwiklabs](https://learn.qwiklabs.com)** or **AWS Academy portal**
2. Start the assigned lab → note down temporary AWS credentials.
3. Open **AWS Console** using those credentials.

---

# Part 2: Build Frontend with React

### Step 2: Create React Feedback App

#### 2.1 Initialize App

```bash
npx create-react-app feedback
cd feedback
```

#### 2.2 Modify App.js

```jsx
import React, { useState } from 'react';
import axios from 'axios';
import './App.css';

function App() {
  const [rollNo, setRollNo] = useState('');
  const [name, setName] = useState('');
  const [subject, setSubject] = useState('Math');
  const [rating, setRating] = useState('5');

  const handleSubmit = async (e) => {
    e.preventDefault();
    const data = { rollNo, name, subject, rating };
    try {
      await axios.post('https://<API_GATEWAY_URL>/feedback', data);
      alert('Feedback submitted!');
    } catch (error) {
      console.error(error);
      alert('Submission failed.');
    }
  };

  return (
    <div className="App">
      <h1>Feedback Form</h1>
      <form onSubmit={handleSubmit}>
        <input placeholder="Roll No" value={rollNo} onChange={(e) => setRollNo(e.target.value)} required /><br /><br />
        <input placeholder="Name" value={name} onChange={(e) => setName(e.target.value)} required /><br /><br />
        <select value={subject} onChange={(e) => setSubject(e.target.value)}>
          <option value="Math">Math</option>
          <option value="Science">Science</option>
          <option value="English">English</option>
        </select><br /><br />
        <select value={rating} onChange={(e) => setRating(e.target.value)}>
          {[1, 2, 3, 4, 5].map(n => <option key={n} value={n}>{n}</option>)}
        </select><br /><br />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
}
export default App;
```

#### 2.3 Build the App

```bash
npm run build
```

---

# Part 3: S3 for Hosting Static Website

### Step 3: Create S3 Bucket & Enable Website Hosting

#### 3.1 Create Bucket

* Go to **S3 → Create bucket**
* Name: `feedback-app-bucket-<unique_suffix>`
* Uncheck: `Block all public access`
* Create.

#### 3.2 Enable Static Website Hosting

* Go to **Properties → Static website hosting**
* Enable
* Set:

  * Index document: `index.html`
  * Error document: `index.html`

#### 3.3 Upload Build Files

* Go to **bucket → Upload → upload all files inside build/**

#### 3.4 Set Bucket Policy for Public Access

* Go to **Permissions → Bucket Policy**, paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::feedback-app-bucket-<your_suffix>/*"
    }
  ]
}
```

---

# Part 4: Set Up CloudFront CDN

### Step 4: Create CloudFront Distribution

1. Go to **CloudFront → Create Distribution**
2. Set:

   * Origin domain: **S3 static website endpoint** (not bucket ARN!)
   * Viewer Protocol Policy: **Redirect HTTP to HTTPS**
   * Default root object: `index.html`
3. Click **Create**
4. Note the **CloudFront URL** (e.g., `https://dxxxxx.cloudfront.net`)

---

# Part 5: Set Up DynamoDB

### Step 5: Create Table

1. Go to **DynamoDB → Create table**
2. Set:

   * Table name: `FeedbackTable-<your_suffix>`
   * Partition key: `id` (String)
3. Leave rest default → Create.

---

# Part 6: Set Up Lambda

### Step 6: Create Lambda Function

#### 6.1 Lambda Code

```python
import json
import boto3
import uuid

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('FeedbackTable-<your_suffix>')

def lambda_handler(event, context):
    body = json.loads(event['body'])
    item = {
        'id': str(uuid.uuid4()),
        'rollNo': body['rollNo'],
        'name': body['name'],
        'subject': body['subject'],
        'rating': body['rating']
    }
    table.put_item(Item=item)
    return {
        'statusCode': 200,
        'headers': {'Access-Control-Allow-Origin': '*'},
        'body': json.dumps({'message': 'Feedback received!'})
    }
```

#### 6.2 Deploy Lambda

* Go to **Lambda → Create function**
* Name: `SubmitFeedbackFunction`
* Runtime: `Python 3.9`
* Use default role or create new with **basic Lambda permissions**.
* Paste code above → Deploy.

#### 6.3 Attach DynamoDB Policy

* Go to **Configuration → Permissions → Execution role → Add permissions**
* Attach `AmazonDynamoDBFullAccess`.

---

# Part 7: Set Up API Gateway

### Step 7: Create API & Route

#### 7.1 HTTP API

* Go to **API Gateway → Create API → HTTP API**
* Add integration: **Lambda (SubmitFeedbackFunction)**

#### 7.2 Add Route & Enable CORS

* Create route:

  * Method: `POST`
  * Resource path: `/feedback`
* Go to **Routes → POST /feedback → Enable CORS**
* Set:

  * Allow headers: `Content-Type`
  * Allow methods: `POST`
  * Allow origin: `*` or your CloudFront URL.
* Click **Add CORS configuration**

#### 7.3 Deploy & Copy Endpoint

* Note endpoint:
  e.g. `https://abcde12345.execute-api.<region>.amazonaws.com/feedback`

---

# Part 8: Redeploy Frontend with API URL

### Step 8: Update & Upload

#### 8.1 Update App.js

* Replace Axios URL with new API endpoint.

#### 8.2 Rebuild & Upload

```bash
npm run build
```

* Go to **S3 → Upload new build/** files (overwrite).

---

# Part 9: Test Entire Flow

### Step 9: Access App via CloudFront

1. Open **CloudFront URL**.
2. Submit feedback form.
3. Check DynamoDB → Table → `Items` → verify entry.

---

✅ **Done:**
🎉 You now have a **serverless app** with:

* **React (S3 + CloudFront)**
* **API Gateway + Lambda + DynamoDB**

---

## Experiment 11
Create a AWS IAM service with following steps:
Create a new IAM role as Rollno_IAM
Attach an administrator policy to that role
Create two groups namely Faculty(Read+Write) & Student(Read)
Provide permissions boundaries for users, groups, or roles
Finally Integrate with CloudFront application, Where student can able to provide feedback only where as Faculty can fetch the feedback given by students

Steps in detail:

# 🏗 Full Architecture Recap

| Layer               | What it does                                |
| ------------------- | ------------------------------------------- |
| **S3 + CloudFront** | Hosts React app (static site)               |
| **API Gateway**     | Exposes POST + GET endpoints                |
| **Lambda**          | Handles write (Submit) + read (Fetch)       |
| **IAM**             | Secures what each Lambda can do in DynamoDB |
| **DynamoDB**        | Stores the feedback                         |

---

# 🚀 PART 1: IAM SETUP (in detail, with JSON policies)

---

## 1️⃣ Create IAM Groups

### ➡ Faculty group

* Go to **IAM → User groups → Create group**
* Name: `Faculty`
* **Attach policy:**

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["dynamodb:GetItem", "dynamodb:Scan"],
            "Resource": "*"
        }
    ]
}
```

You’ll do this by creating a new policy:

* IAM → Policies → Create → JSON → paste above → Name: `FacultyReadPolicy`
* Then go to Group → Attach policies → select `FacultyReadPolicy`.

---

### ➡ Student group

* Go to **IAM → User groups → Create group**
* Name: `Student`
* **Attach policy:**

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["dynamodb:PutItem"],
            "Resource": "*"
        }
    ]
}
```

Same as above: create policy `StudentWritePolicy` and attach.

---

## 2️⃣ Create IAM Role for Lambda — `Rollno_IAM`

* IAM → Roles → Create role →
  **Trusted entity:** Lambda
* **Permissions:** Attach `AWSLambdaBasicExecutionRole` (for logs).
* Name: `Rollno_IAM`.

---

### ➡ Attach Inline Policies to enforce access

Go to `Roles → Rollno_IAM → Add inline policy`.

For `SubmitFeedbackFunction`:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["dynamodb:PutItem"],
            "Resource": "*"
        }
    ]
}
```

And for `FetchFeedbackFunction`:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["dynamodb:Scan", "dynamodb:GetItem"],
            "Resource": "*"
        }
    ]
}
```

(Tip: you could also split these into two roles if you want to be very strict.)

---

# ⚙ PART 2: Lambda Functions

---

## ✅ SubmitFeedbackFunction (students)

**Python code:**

```python
import json
import boto3
import uuid

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('FeedbackTable')

def lambda_handler(event, context):
    body = json.loads(event['body'])
    item = {
        'id': str(uuid.uuid4()),
        'rollNo': body['rollNo'],
        'name': body['name'],
        'subject': body['subject'],
        'rating': body['rating']
    }
    table.put_item(Item=item)
    return {
        'statusCode': 200,
        'headers': { 'Access-Control-Allow-Origin': '*' },
        'body': json.dumps({ 'message': 'Feedback submitted!' })
    }
```

* Use IAM role `Rollno_IAM`.

---

## ✅ FetchFeedbackFunction (faculty)

**Python code:**

```python
import json
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('FeedbackTable')

def lambda_handler(event, context):
    response = table.scan()
    items = response.get('Items', [])
    return {
        'statusCode': 200,
        'headers': { 'Access-Control-Allow-Origin': '*' },
        'body': json.dumps(items)
    }
```

* Also use IAM role `Rollno_IAM`.

---

# 🌐 PART 3: API Gateway Setup

---

## ➡ Create HTTP API

* API Gateway → Create HTTP API → Add integration → Lambda.
* Setup routes:

| Route       | Method | Lambda Function        |
| ----------- | ------ | ---------------------- |
| `/feedback` | POST   | SubmitFeedbackFunction |
| `/feedback` | GET    | FetchFeedbackFunction  |

---

## ➡ Enable CORS

* Go to Routes → `/feedback` → Enable CORS
* Allow headers: `Content-Type`
* Allow methods: `POST, GET`
* Allow origins: `*`

---

# 🎯 PART 4: CloudFront + S3 React Frontend

---

## ➡ React app

```bash
npx create-react-app feedback-app
cd feedback-app
npm install axios
```

### Modify `App.js`:

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function App() {
  const [role, setRole] = useState('student');
  const [rollNo, setRollNo] = useState('');
  const [name, setName] = useState('');
  const [subject, setSubject] = useState('Math');
  const [rating, setRating] = useState('5');
  const [feedbacks, setFeedbacks] = useState([]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    await axios.post('https://<api-id>.execute-api.<region>.amazonaws.com/feedback', {
      rollNo, name, subject, rating
    });
    alert('Feedback submitted!');
  };

  const fetchFeedback = async () => {
    const res = await axios.get('https://<api-id>.execute-api.<region>.amazonaws.com/feedback');
    setFeedbacks(res.data);
  };

  return (
    <div style={{padding: 20}}>
      <select value={role} onChange={(e) => setRole(e.target.value)}>
        <option value="student">Student</option>
        <option value="faculty">Faculty</option>
      </select>

      {role === 'student' && (
        <form onSubmit={handleSubmit}>
          <input placeholder="Roll No" value={rollNo} onChange={(e) => setRollNo(e.target.value)} required /><br/>
          <input placeholder="Name" value={name} onChange={(e) => setName(e.target.value)} required /><br/>
          <select value={subject} onChange={(e) => setSubject(e.target.value)}>
            <option>Math</option><option>Science</option><option>English</option>
          </select><br/>
          <select value={rating} onChange={(e) => setRating(e.target.value)}>
            {[1,2,3,4,5].map(n => <option key={n}>{n}</option>)}
          </select><br/>
          <button type="submit">Submit</button>
        </form>
      )}

      {role === 'faculty' && (
        <div>
          <button onClick={fetchFeedback}>Load Feedback</button>
          <ul>
            {feedbacks.map(fb => (
              <li key={fb.id}>{fb.name} ({fb.rollNo}) - {fb.subject} - {fb.rating}</li>
            ))}
          </ul>
        </div>
      )}
    </div>
  );
}
export default App;
```

---

## ➡ Build + Upload

```bash
npm run build
```

* Upload to S3 bucket (static hosting enabled).
* Use CloudFront to point to S3 website endpoint.

---

# 🛠 PART 5: Testing

---

✅ Students can:

* Select `Student`, fill form, submit → triggers POST → stores in DynamoDB.

✅ Faculty can:

* Select `Faculty`, click Load Feedback → triggers GET → fetches from DynamoDB.

If a student **modifies JS to send GET**, the Lambda policy **forbids scan**, and API returns `403`.

---

# 🔍 DONE! ✅

---

Absolutely! Here’s a **very detailed, beginner-friendly step-by-step guide** on how to:

✅ Upload your **React app** to an **S3 bucket with static website hosting**
✅ Then create a **CloudFront distribution** that points to your S3 website endpoint, to serve the app via a CDN.

---

# 🚀 Part A: Upload to S3 (Static Website Hosting)

---

## 1️⃣ Create the S3 bucket

1. Go to **AWS Console → S3 → Create bucket**
2. Enter a unique bucket name, e.g.

   ```
   feedback-app-bucket-<your-name-or-id>
   ```
3. Select your region (important for cost + performance).
4. Under **Block Public Access**,
   🔥 **Uncheck** “Block all public access”
5. Confirm you acknowledge that the bucket will be public.
6. Click **Create bucket**.

---

## 2️⃣ Enable Static Website Hosting

1. Go to your bucket → **Properties** tab.
2. Scroll to **Static website hosting**.
3. Click **Edit**.
4. Choose:
   ✅ **Enable**
5. Enter:

   ```
   Index document: index.html
   Error document: index.html
   ```
6. Click **Save changes**.

---

## 3️⃣ Upload your React build

1. From your React project folder, run:

   ```bash
   npm run build
   ```

   This generates a `build/` folder.

2. In AWS Console, go to your bucket → **Objects → Upload**.

3. Click **Add files**, select everything **inside the build folder** (not the build folder itself), e.g.:

   * `index.html`
   * `static/`
   * `manifest.json`
     etc.

4. Click **Upload**.

---

## 4️⃣ Set bucket policy for public read

1. Go to **Permissions → Bucket policy**.
2. Paste this JSON (replace with your bucket name):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::feedback-app-bucket-<your-name>/*"
    }
  ]
}
```

3. Click **Save**.

---

## 5️⃣ Test the S3 website

1. Go to **Properties → Static website hosting**.
2. Copy the **Endpoint URL**, e.g.

   ```
   http://feedback-app-bucket-<your-name>.s3-website-us-east-1.amazonaws.com
   ```
3. Open it in your browser.
   ✅ Your app should load!

---

# 🌎 Part B: Set up CloudFront pointing to S3 website endpoint

(Important: **Use the S3 website endpoint, NOT the S3 bucket ARN.**)

---

## 6️⃣ Create a CloudFront distribution

1. Go to **AWS Console → CloudFront → Distributions → Create distribution**.

2. Under **Origin**:

   * **Origin domain**:
     Click inside the box.
     It will auto-suggest your bucket *twice*:

     * One ends with `.s3.amazonaws.com` (the REST API endpoint)
     * One ends with `.s3-website-<region>.amazonaws.com` (the website endpoint).

   ✅ Select the one ending with `.s3-website-<region>.amazonaws.com`.

3. Leave default **Origin path** blank.

4. Set:

   * **Viewer Protocol Policy**:

     ```
     Redirect HTTP to HTTPS
     ```

5. Under **Default cache behavior**:

   * **Cache policy**: CachingOptimized (default)

6. Set:

   * **Default root object**:

     ```
     index.html
     ```

7. Click **Create distribution**.

---

## 7️⃣ Wait for deployment

* It takes **5-15 minutes** for CloudFront to deploy.
* Once **Status → Enabled**, copy the **Domain name**, e.g.

  ```
  d1234abcd.cloudfront.net
  ```

---

## 8️⃣ Test via CloudFront

* Open `https://d1234abcd.cloudfront.net` in your browser.
  ✅ Your React app is now served globally via CDN!

---

# 💡 (Optional) Configure caching & invalidation

* Any time you upload new files to S3, you might have to **invalidate the cache** in CloudFront to force it to fetch new files.
* Go to CloudFront → your distribution → **Invalidations → Create invalidation** → enter `/*`.

---

# 🧪 Final Testing checklist

✅ **Direct S3 website URL**

* Still loads your app.

✅ **CloudFront URL**

* Loads via CDN (faster).

✅ Check network in browser dev tools → requests are served from CloudFront.

---

# 🚀 Summary (Quick steps)

✅ S3 bucket → Static website hosting enabled → files uploaded → public read policy.
✅ CloudFront → Origin set to S3 **website endpoint** → `index.html` root.
✅ App loads from global CDN.

---

















