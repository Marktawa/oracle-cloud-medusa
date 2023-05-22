# Deploying Medusa with Oracle Cloud

## How to Deploy a Medusa Commerce Backend Server using Oracle Cloud Compute Infrastructure

In this tutorial you will learn how to deploy a live server of the Medusa backend in the cloud using Oracle Cloud Infrastructure (OCI)

## Introduction

[Medusa](https://medusajs.com/readme/) is a modular commerce engine that helps developers build rich digital commerce applications. It is free, open source, and uses Node.js. With Medusa, a developer can quickly build advanced commerce solutions without coding from scratch.

Medusa is very flexible and customizable. It can be hosted in the cloud using IAAS and PAAS.

[Oracle Cloud](https://www.oracle.com/cloud/) provides virtual machines suitable enough to host a Medusa backend.

This guide will teach you how to deploy a live server of the Medusa backend in the cloud using OCI.

## Why Oracle Cloud?

Oracle Cloud offers a very generous "Always-Free" tier for their cloud services. You can host the Medusa server application within the OCI all for free.

The free services of interest that OCI provides of interest to our app are:
- An ARM-based VM with:
    - Upto 8 CPUs
    - Linux
    - 24GB of memory
    - 50GB boot volume for storage
- Up to  200GB of block storage for your VM with 5 backups
- Free Load Balancer with max bandwidth set to 10Mbps
- Up to 10TB per month of Outbound Data Transfer
- Unlimited Inbound Data Transfer

For more information about the all the free services offered Oracle Cloud check out the [Always Free Resources](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm) page.

## Prerequisites

- A Medusa backend server app. [Follow the Quickstart guide to create one](https://docs.medusajs.com/development/backend/install)
- A GitHub account. [Sign up for one here]()
- An Oracle Cloud account. [Sign up for Oracle Cloud account here](https://signup.oraclecloud.com)

The first few steps will involve creating a Linux instance.

## Part A: Create OCI Linux Instance

## Step 1: Create a key pair

> NOTE: 
>
> If you have OpenSSH installed or already have an [SSH-2 RSA key pair](https://docs.oracle.com/en-us/iaas/Content/General/Concepts/credentials.htm#Instance) in your computer, you can skip this step.
> If you are using a Windows system and don't have OpenSSH, download and install the [PuTTY Key Generator](http://www.putty.org/)

Follow these [instructions on Creating an SSH Key Pair on Windows Using PuTTY Key Generator](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/creatingkeys.htm#Creating_a_Key_Pair:~:text=Creating%20an%20SSH%20Key%20Pair%20on%20Windows%20Using%20PuTTY%20Key%20Generator)


## Step 2: Create a compartment for your resources

Create a compartment to control and organize the resources you will use for your Medusa backend server. You will only need one compartment to manage all the project resources.

[Sign in](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/signingin.htm#Signing_In_to_the_Console) to your Oracle Cloud Console.

Click on the navigation menu icon and select **Identity & Security**. Choose **Compartments** under the **Identity** option.

![Select Compartments on Navigation Menu](https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/select-compartments-on-nav-menu_kdcijv.png)

Click **Create Compartment**.

Enter the following:
- **Name:** Enter "Medusa"
- **Description:** Enter "Compartment for Medusa backend server"
- **Parent Compartment:** Select the default root compartment

![Click Create Compartment](https://res.cloudinary.com/craigsims808/image/upload/v1684746912/articles/oracle-medusa/create-compartment_ev0cqw.png)

Select **Create Compartment** and refresh to see your compartment displayed in the list of compartments.

![List of Compartments](https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/list-of-compartments_tjoqyd.png)

Click on **Networking** in the navigation menu and select **Virtual cloud networks**.

![Select Virtual Cloud Networks](https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/select-vcn-nav-menu_josftr.png)

Select the **Medusa** compartment you have just created from the compartment list on the left.

![Select Medusa Compartment](https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/select-medusa-compartment_dpvs7g.png)

For more information check out these [instructions on Choosing and Creating a Compartment for your project](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/choosingcompartments.htm#Choosing_a_Compartment).

## Step 3: Create a cloud network

Select **Start VCN Wizard**

![Select Start VCN Wizard](https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/select-start-vcn-wizard_z2kp8l.png)

Click **Create VCN with Internet Connectivity**, and then click **Start VCN Wizard**

![Create VCN with Internet Connectivity](https://res.cloudinary.com/craigsims808/image/upload/v1684746912/articles/oracle-medusa/create-vcn-with-internet-connectivity_zquzqm.png)

Enter these details:
- **VCN Name**: medusa
- **Compartment**: Select the Medusa compartment
- **VCN CIDR Block**: 10.0.0.0/16
- **Public Subnet CIDR Block**: 10.0.0.0/24
- **Private Subnet CIDR Block**: 10.0.1.0/24
- Leave all the other details as is.

Select **Next**. 

![Details for VCN](https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/details-for-vcn_yofjhe.png)

Review the details for your VCN and click on **Create**. Wait for a few seconds for your VCN to be created, then select **View VCN**.

![View Virtual Cloud Network](https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/view-virtual-cloud-network_xjkodl.png)

For more information check out [Creating a Virtual Cloud Network](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/creatingnetwork.htm#Creating_a_Virtual_Cloud_Network)

## Step 4: Launch your instance

Select **Instances** under **Compute** in the navigation menu of your Oracle Dashboard

![select instances in nav menu](https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/select-instances-in-nav-menu_eec1da.png)

Click **Create Instance** to take you to the **Create compute instance** page.

![create instance](https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/click-create-instance_rrlnh2.png)

Enter the name `medusa-server` for your instance and the compartment should be `Medusa`.

![name and  compartment](https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/name-and-compartment_sr5rlv.png)

For the **Placement** section, leave the default **Availability domain** as is.

The following **Image and shape** section details should be added:
- **Image:** `Ubuntu 22.04`
- **Shape:** `VM.Standard.A1.Flex`

Follow these [instructions](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/launchinginstance.htm#Launching_a_Linux_Instance:~:text=In%20the%20Image%20and%20shape%20section%2C%20make%20the%20following%20selections%3A) to change the image and shape for your `medusa-server` VM.

![final image and shape](https://res.cloudinary.com/craigsims808/image/upload/v1684746912/articles/oracle-medusa/change-image-and-shape_jxc5ag.png)

For the **Networking** section, add the following configurations:
- **Primary network**: `Select existing virtual cloud network`
- **Virtual cloud network in `medusa`**: `medusa`
- **Subnet**: `Select existing subnet`
- **Subnet in `medusa`**: `medusa`
- Select **Assign a public IPv4 address**

![networking section](https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/networking-options_qbjrm4.png)

In the **Add SSH key** section:

- Choose **Generate a key pair for me**

![generate key pair](https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/generate-key-pair_yo3p2c.png)

OCI will generate an SSH key for your instance

- Select **Save Private Key** to save the private key in your computer as a `.key` file.

![save private key](https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/save-private-key_tybj36.png)

If you prefer other options for establishing an SSH connection use [these instructions](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/launchinginstance.htm#Launching_a_Linux_Instance:~:text=Upload%20public%20key%20files%20(.pub))

For the **Boot volume** section, leave the default options.

Click **Create** and wait for your instance to be provisioned.

![click create](https://res.cloudinary.com/craigsims808/image/upload/v1684746912/articles/oracle-medusa/click-create_sjiglv.png)

Wait for a few minutes for your instance to be created and you will see it the Console. When the status of the instance has changed to `RUNNING` you can now connect to it.

![vm running](https://res.cloudinary.com/craigsims808/image/upload/v1684755320/articles/oracle-medusa/vm_running_zz5gcg.png)

For more information check out [Launching a Linux Instance](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/launchinginstance.htm#Launching_a_Linux_Instance) in the Oracle Cloud Documentation.

## Step 5: Connect to your instance

Note down the **Public IP address** and **Username** for your running instance.

![ip address for instance](https://res.cloudinary.com/craigsims808/image/upload/v1684755320/articles/oracle-medusa/vm_details_amrggp.png)

Follow these instructions to apply the appropriate permissions to your SSH private key so that you only you can read it:
- [Unix-Style System](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/testingconnection.htm#connecting__linux-from-unix)
- [Windows System with OpenSSH](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/testingconnection.htm#connecting__linux-from-windows-openssh)

After applying the permissions, use the following SSH command to access the instance:

```bash
ssh -i <private_key_file> <username>@<public-ip-address>
```

Where:

`<private_key_file>` is the path and name of the private SSH key file in your computer associated with your medusa server instance.
`<username>` is the default username you noted down in the previous step. In this case it is `ubuntu`.
`<public-ip-address>` is the IP address for your instance you noted down in a previous step.

If your SSH connection is successful, you should be logged in to your medusa server instance as the default `ubuntu` user.

![successful ssh login](https://res.cloudinary.com/craigsims808/image/upload/v1684755320/articles/oracle-medusa/successful-ssh-connection_w7ofcz.png)

## PART B: Set up and Configure Environment

## Step 6: Install Node.js

Medusa supports LTS versions of Node.js (v16 or greater).

Install Node.js in your Medusa server.

```bash
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install nodejs
node -v && npm -v
```

The last command should give you `v18.x.x` for node and `9.x.x` for npm to confirm successful installation.

[Resolve the access permissions](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally) for npm using the following command:

```bash
mkdir ~/.npm-global && npm config set prefix '~/.npm-global' && echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.profile && source ~/.profile
```

## Step 7: Install Git

Git comes preinstalled in Ubuntu. You can confirm if it is installed by running the following command:

```bash
git --version
```

If it is not installed follow these instructions on [how to install Git on Ubuntu 22.04](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

Configure the global `username` and `email` for your [Git Identity](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup).

## Step 8: Setup Medusa CLI

Install the [Medusa CLI](https://docs.medusajs.com/cli/reference) to manage your Medusa server.

```bash
npm install @medusajs/medusa-cli -g
```
Confirm successful installation by running:

```bash
medusa -v
```

## Step 9: Setup and configure PostgreSQL Database.

When deploying Medusa, it is recommended to use a PostgreSQL database. In this step, you will install PostgreSQL and create a database for your Medusa server.

Download and install PostgreSQL:

```bash
sudo sh -c \
  'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - \
  https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql
```

Check if PostgreSQL installation was successful.

```bash
psql -V
```

Access the PostgreSQL console.

```bash
sudo -u postgres psql
```

Create a new user named `medusa_admin`.

```sql
CREATE USER medusa_admin WITH PASSWORD 'medusa_admin_password';
```

Create a new database named `medusa_db` and make `medusa_admin` the owner.

```sql
CREATE DATABASE medusa_db OWNER medusa_admin;
```

Grant all privileges to `medusa_admin` and exit the PostgreSQL console.

```sql
GRANT ALL PRIVILEGES ON DATABASE medusa_db TO medusa_admin;
```

```sql
exit
```

## Step 10: Setup Medusa app on local machine

Create a [GitHub repository](https://github.com/new) to store your Medusa server app and [clone it](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) to your local machine.

In the work folder of the GitHub repo, set up a Medusa server app using the [Medusa Server Quickstart Guide](https://docs.medusajs.com/development/backend/install) instructions.

If you successfully followed the instructions, you should have a directory named `my-medusa-store` containing your Medusa server app and a server running on port `9000`.

Stop the server app on your local machine, then open up `medusa.config.js` in the root of your app folder. 

Update `medusa.config.js` by commenting out the line `database_database: "./medusa-db.sql",` and adding `database_url: DATABASE_URL,` in the `projectConfig` section.

```js
/** @type {import('@medusajs/medusa').ConfigModule["projectConfig"]} */
const projectConfig = {
  jwtSecret: process.env.JWT_SECRET,
  cookieSecret: process.env.COOKIE_SECRET,
  //database_database: "./medusa-db.sql",
  database_url: DATABASE_URL,
  database_type: DATABASE_TYPE,
  store_cors: STORE_CORS,
  admin_cors: ADMIN_CORS,
  // Uncomment the following lines to enable REDIS
  // redis_url: REDIS_URL
}
```

Open up `package.json` in the root of your app folder and change the `start` script as follows:

```json
  "scripts": {
    "clean": "cross-env ./node_modules/.bin/rimraf dist",
    "build": "cross-env npm run clean && tsc -p tsconfig.json",
    "watch": "cross-env tsc --watch",
    "test": "cross-env jest",
    "seed": "cross-env medusa seed -f ./data/seed.json",
    "start": "medusa migrations run && medusa start",
    "start:custom": "cross-env npm run build && node --preserve-symlinks index.js",
    "dev": "cross-env npm run build && medusa develop",
    "build:admin": "cross-env medusa-admin build"
  },
```

Commit and push the changes you made to GitHub.

```bash
git commit -a -m "Updated medusa.config.js & package.json" 
git push
```

## Step 11: Run Medusa Server on Oracle Cloud VM

Clone your Medusa server app repo to your Oracle Cloud instance. Change directories to your Medusa server app repo.

Install the dependencies.

```bash
cd my-medusa-store
npm install
```

Create a `.env` file in the root of `my-medusa-store` and add the following code:

```
PORT=9000
JWT_SECRET=something
COOKIE_SECRET=something
DATABASE_TYPE=postgres
DATABASE_URL=postgres://medusa_admin:medusa_admin_password@localhost:5432/medusa_db
```

> **NOTE:**
>
> Use other values for `JWT_SECRET` and `COOKIE_SECRET` besides `something` for better security.

Seed your database.

```bash
npm run seed
```

## PART C: Test Server

# Step 12: Start your Server

Run the following command to start your Medusa app server.

```bash
npm run start
```
Your medusa server app should start running but it is not accessible over the internet yet.

# Step 13: Open Access to Server

In this step you will add an ingress rule to the `medusa` subnet to allow internet connections on port `9000`.

Go to your Oracle Cloud console, open the navigation menu and select **Networking** and click on **Virtual Cloud Networks**.

![Select Virtual Cloud Networks](https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/select-vcn-nav-menu_josftr.png)

Select the `medusa` VCN.

![Select medusa vcn](https://res.cloudinary.com/craigsims808/image/upload/v1684746915/articles/oracle-medusa/select-medusa-vcn_ckuuhy.png)

Select **Security List** link and click on the **Default Security List for medusa** link.

![Select Default Security List](https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/select-default-security-list_hx75zv.png)

Select **Add Ingress Rules** to display the **Add Ingress Rules** modal. Fill in the ingress rule as follows:
- **Stateless**: `Checked`
- **Source Type**: `CIDR`
- **Source CIDR**: `0.0.0.0/0`
- **IP Protocol**: `TCP`
- **Source port range**:
- **Destination Port Range**: `9000`
- **Description**: `Allow HTTP connections`

![Add Ingress Rules](https://res.cloudinary.com/craigsims808/image/upload/v1684746914/articles/oracle-medusa/select-add-ingress-rules_xtsjij.png)

Click **Add Ingress Rules**. Now your server is accessible via HTTP.

![List of Ingress Rules](https://res.cloudinary.com/craigsims808/image/upload/v1684746913/articles/oracle-medusa/list-of-ingress-rules_wn0jsq.png)


## Step 14: Configure Ubuntu Firewall

Follow these steps to configure the firewall settings for your Medusa server. In a new terminal session, ssh into your medusa server. 

Run the following commands to update the `iptables` configuration to allow HTTP traffic.

```bash
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 9000 -j ACCEPT
sudo netfilter-persistent save
```

## Step 15: Test in your browser

Visit `http://x.x.x.x:9000/health` in your browser, where `x.x.x.x` is your server's IP address. You should see an `OK` message. This confirms that your server is working.

![health check](https://res.cloudinary.com/craigsims808/image/upload/v1684755319/articles/oracle-medusa/health-check_wwfpfq.png)

Visit `http://x.x.x.x:9000/store/products` and you should see a list of products in your database.

![products check](https://res.cloudinary.com/craigsims808/image/upload/v1684755320/articles/oracle-medusa/products-check_ctkes1.png)

## Considerations

This tutorial showcased a minimal way of deploying a Medusa server on Oracle Cloud. Here are some additional configurations you can make:

- [Install the MinIO plugin](https://docs.medusajs.com/plugins/file-service/minio) to handle image uploads to your Medusa backend.
- [Install](https://redis.io/docs/getting-started/installation/) and [configure Redis](https://docs.medusajs.com/development/backend/configurations#redis) if you want scheduled jobs.
- [Install PM2](https://pm2.io/docs/runtime/guide/installation/) to manage and ensure availability of your Medusa server.
- [Install Nginx](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/) to configure a domain name.
- Add an SSL certificate for your domain name.
- Change the URL in your Medusa storefront and/or admin based on the URL of your server.
- Update your Medusa server with the Admin and storefront URLs.
- Set up a CI/CD pipeline between your development environment and your deployment
- Secure your server.

## Conclusion

Oracle Cloud Free Tier provides a convenient way for developers to test and deploy Medusa ecommerce apps. I hope this tutorial has provided sufficient knowledge on how you could host a Medusa server app on Oracle Cloud.

If there are any issues or questions, feel free to comment with your query.






