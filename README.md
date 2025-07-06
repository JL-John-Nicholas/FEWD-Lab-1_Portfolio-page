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








