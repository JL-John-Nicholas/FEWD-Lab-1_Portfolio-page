## Experiment 1:
Establish an AWS account. Use the AWS Management Console to launch an EC2 instance and connect to it from PUTTY.
Note: Create Instance name as External_Rollno, PEM File as External_Rollno
1. Write a Java code for Fibonacci series in AWS
2. Print Fibonacci series in Putty Console.

Steps in detail:

## 1. Establish an AWS Account

1. Go to [https://aws.amazon.com/](https://aws.amazon.com/) and sign up for a free account.
2. Provide your email, password, and account details.
3. Add credit/debit card details for verification.
4. Complete phone verification.
5. Log in to the [AWS Management Console](https://console.aws.amazon.com/).

---

## 2. Launch an EC2 Instance

### a. Navigate to EC2

1. From the AWS Management Console, search for **EC2** in the search bar and open the **EC2 Dashboard**.

### b. Launch an Instance

1. Click **Launch Instance**.
2. Set **Instance Name** as `External_Rollno`.
3. Select **Amazon Machine Image (AMI)**: Choose `Amazon Linux 2 AMI (HVM), SSD Volume Type`.
4. Choose **Instance Type**: Select `t2.micro` (free tier eligible).
5. Click **Key pair (login)** > **Create new key pair**.

   * Name it: `External_Rollno`
   * File format: `.pem`
   * Download the `.pem` file and keep it safe. You’ll need this for SSH.
6. Leave other defaults. Click **Launch Instance**.

### c. Allow SSH Traffic

1. In the **Security group settings**, ensure **SSH (port 22)** is open to your IP or anywhere (`0.0.0.0/0`) for testing.

---

## 3. Connect to EC2 from PUTTY

### a. Convert PEM to PPK

1. Download and install **PuTTY** and **PuTTYgen** from [https://www.putty.org/](https://www.putty.org/).
2. Open **PuTTYgen**.
3. Click **Load**, select `All Files (*.*)`, and open your `External_Rollno.pem`.
4. Click **Save private key**, save as `External_Rollno.ppk`.

### b. Connect Using PuTTY

1. Open **PuTTY**.
2. In **Host Name**, enter: `ec2-user@<public-ip>`

   * Replace `<public-ip>` with your instance's **Public IPv4 address** from the EC2 console.
3. In **Connection > SSH > Auth**, browse for `External_Rollno.ppk`.
4. Click **Open**.
5. Accept the prompt. You are now logged into your EC2 instance.

---

## 4. Write Java Code for Fibonacci

### a. Install Java

In the PuTTY console, run:

```
sudo yum update -y
sudo yum install java-1.8.0-openjdk -y
```

### b. Create Java Program

1. Create a file:

```
nano Fibonacci.java
```

2. Paste the following Java code:

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

3. Press `Ctrl+O`, `Enter` to save, then `Ctrl+X` to exit.

---

## 5. Compile and Run Java Program

1. Compile the program:

```
javac Fibonacci.java
```

2. Run it:

```
java Fibonacci
```

You will see the output:

```
Fibonacci Series up to 10: 0 1 1 2 3 5 8 13 21 34
```

---


