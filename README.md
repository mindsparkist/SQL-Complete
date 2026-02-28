To install MySQL on Windows in 2026, the most professional and "expert-approved" method is using the **MySQL Installer**. This tool acts as a central hub for managing the Server, the Shell, and the GUI (Workbench).

---

## 1. Preparation & Download

Before you start, ensure you have **Administrator** rights on your Windows machine.

1. **Go to the Official Portal:** Visit the [MySQL Community Downloads](https://dev.mysql.com/downloads/installer/) page.
2. **Select the Installer:** You will see two options. Choose the larger "web-community" or "community" MSI installer (usually around 400MB+). This ensures you have all the components offline.

---

## 2. The Step-by-Step Installation

### Step A: Choosing Setup Type

When the installer launches, you'll be asked for a setup type.

* **Developer Default:** Installs everything (Server, Workbench, Shell, Connectors). **Recommended for most.**
* **Server Only:** If you only want the database engine (common for production servers).
* **Custom:** Allows you to pick specific versions.

### Step B: Type and Networking

* **Config Type:** Choose **"Development Computer"** (uses minimal memory) or **"Server Computer"** (uses more RAM for better performance).
* **Connectivity:** Ensure **TCP/IP** is checked. The default Port is **3306**. Open this in your Windows Firewall if you need remote access.

### Step C: Authentication Method (Crucial)

MySQL 8.0+ and newer versions use **Strong Password Encryption**.

* **Recommendation:** Use the recommended "Strong Password Authentication."
* **Legacy Note:** Only choose "Legacy" if you are connecting via very old applications that don't support the new SHA-256 based encryption.

### Step D: Accounts and Roles

* **Root Password:** Set a complex password for the `root` user. **Do not lose this.**
* **User Accounts:** It is a security best practice to create a "Standard User" for your daily coding tasks instead of using the `root` account for everything.

### Step E: Windows Service

* **Run as Windows Service:** Check this so MySQL starts automatically when you turn on your PC.
* **Service Name:** Default is `MySQL80` (or `MySQL90` depending on the current version).

---

## 3. Post-Installation: Environment Variables

To run `mysql` commands directly from your Windows Terminal or PowerShell, you must add the path to your System Variables.

1. Find your MySQL `bin` folder (typically: `C:\Program Files\MySQL\MySQL Server X.X\bin`).
2. Search Windows for **"Edit the system environment variables"**.
3. Click **Environment Variables** > Find **Path** in System Variables > Click **Edit**.
4. Click **New** and paste the path to the `bin` folder.
5. Restart your Terminal and type `mysql -u root -p` to verify.

---

## 4. Verification with Workbench

Once installed, open **MySQL Workbench**.

1. Click the `+` icon to create a new connection.
2. Enter "Local Instance" as the name.
3. Use `127.0.0.1` for Hostname and `3306` for Port.
4. Click **Test Connection**. If it asks for a password, your installation was successful!

---

## Expert Tip: The "Invisible" Alternative

If you prefer a lightweight, "zero-install" environment, many experts now use **Docker**.
`docker run --name mysql-dev -e MYSQL_ROOT_PASSWORD=yourpassword -p 3306:3306 -d mysql:latest`
This keeps your Windows Registry clean and allows you to delete the database entirely in seconds.

To create and populate a database using a `.sql` script like the one you've provided, you generally follow a standard workflow in an RDBMS like **MySQL**.

Based on the file you uploaded, you are setting up four distinct schemas: `sql_invoicing`, `sql_store`, `sql_hr`, and `sql_inventory`.

---

## 1. Using MySQL Workbench (GUI Method)

This is the most common method for developers on Windows.

1. **Open MySQL Workbench** and connect to your local instance.
2. **Open the Script:** Go to `File` > `Open SQL Script...` and select your `create-databases.sql` file.
3. **Execute:** Click the **Lightning Bolt icon** (Execute the entire script) in the toolbar.
4. **Refresh:** In the "Schemas" tab on the left, right-click and select **Refresh All**. You will now see all four databases listed.

---

## 2. Using Command Line (Terminal Method)

If you have configured your environment variables, this is the fastest "Expert" way.

1. **Open PowerShell or CMD.**
2. **Navigate** to the folder where your `.sql` file is saved.
3. **Run the following command:**
```bash
mysql -u root -p < create-databases.sql

```


4. **Enter your password** when prompted. The `<` symbol tells MySQL to take the contents of the file and "pipe" them into the database engine as commands.

---

## 3. Analysis of Your Specific Script

Your uploaded file uses a very safe and professional structure. Here is what is happening behind the scenes:

### **Data Integrity Layers**

* **Idempotency:** The script uses `DROP DATABASE IF EXISTS`. This allows you to run the script multiple times without errors; it simply wipes the old version and creates a fresh one.
* **Engine Specification:** It specifies `ENGINE=InnoDB`, which ensures that your database supports **ACID transactions** and **Foreign Keys**.
* **Constraints:** It establishes relationships. For example, in your `invoices` table, it uses `CONSTRAINT FK_client_id` to ensure an invoice cannot exist without a valid client.

### **The Resulting Schema Structure**

Once created, your database environment will look like this:

| Database | Primary Tables | Purpose |
| --- | --- | --- |
| **`sql_invoicing`** | `clients`, `invoices`, `payments` | Financial tracking and billing. |
| **`sql_store`** | `products`, `customers`, `orders` | E-commerce/Retail management. |
| **`sql_hr`** | `employees`, `offices` | Corporate hierarchy and staff tracking. |
| **`sql_inventory`** | `products` | Stock level and unit price management. |

---

## 4. Verification Step

Once the process is complete, you should run a quick query to ensure the data is there. Type this into your query window:

```sql
USE sql_store;
SELECT * FROM customers WHERE points > 2000;

```

If you see names like **Babara MacCaffrey** and **Freddi Boagey**, your database is live and correctly populated.

