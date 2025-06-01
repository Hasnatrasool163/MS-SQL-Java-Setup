
# Configure MS SQL Server on Windows (7–11) for Java Applications

This guide explains how to set up MS SQL Server for Java applications with Windows Authentication, ensuring you can connect to your database using `integratedSecurity` mode.

---

## Prerequisites

- **Windows OS:** 7, 8, 10, or 11.
- **MS SQL Server:** Installed and running.
- **Java:** Installed (JDK/JRE).
- **Admin rights** on your computer to modify system files and firewall settings.

---

## Step 1: Download the MS SQL Server JDBC Driver

1. Visit the [official Microsoft JDBC Driver for SQL Server](https://learn.microsoft.com/en-us/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server) page.
2. Select a version that matches your Java runtime (e.g., 12.6.3 or similar).
3. Download the `.zip` file.

---

## Step 2: Extract the JDBC Driver

1. Unzip the downloaded file to a folder of your choice.
2. Inside the extracted folder:
   - Locate the `.jar` file (e.g., `mssql-jdbc-12.6.3.jre8.jar`).
   - Locate the `auth` folder (contains `auth.dll`).

---

## Step 3: Add the JDBC Driver to Your Java Project

- **Option 1:** Add the `.jar` file to your project’s classpath manually.
- **Option 2 (Recommended):** Use Maven:

```xml
<dependency>
  <groupId>com.microsoft.sqlserver</groupId>
  <artifactId>mssql-jdbc</artifactId>
  <version>12.6.3.jre8</version> <!-- Adjust version as needed -->
</dependency>
```

---

## Step 4: Copy the DLL for Integrated Security

1. From the `auth` folder, copy the appropriate `auth.dll` (x64 or x86 depending on your system).
2. Paste it into your Java installation’s `bin` directory:
   - Example: `C:\Program Files\Java\jdk-17\bin\`

---

## Step 5: Configure SQL Server Network Settings

1. Open **SQL Server Configuration Manager** (search in Start Menu).
2. Navigate to:
   - **SQL Server Network Configuration** > **Protocols for MSSQLSERVER**.
3. Enable **TCP/IP**.
4. Right-click **TCP/IP**, select **Properties**, and:
   - **IP Addresses Tab**: 
     - Set **TCP Port** to **1433** for **IPAll** and other relevant IPs.
     - Ensure **127.0.0.1** is listed for local connections.

---

## Step 6: Allow Firewall Access

1. Open **Control Panel** > **System and Security** > **Windows Defender Firewall** > **Advanced Settings**.
2. Add a **New Inbound Rule**:
   - Type: **Port**
   - Protocol: **TCP**
   - Port: **1433**
   - Allow the connection.
   - Apply the rule to **Domain**, **Private**, and **Public**.

---

## Step 7: Update Your Connection String

Use the following format in your Java code to connect using Windows Authentication:

```java
String connectionUrl = 
    "jdbc:sqlserver://localhost:1433;databaseName=YourDatabaseName;"
  + "integratedSecurity=true;"
  + "trustServerCertificate=true;";
```

- **`integratedSecurity=true;`** enables Windows Authentication.
- **`trustServerCertificate=true;`** allows self-signed certificates (for development).

---

## Additional Notes

- Always verify that your firewall and network configurations allow SQL Server traffic.
- If using Maven, ensure the driver version matches your Java version.
- If experiencing authentication issues, double-check that the `auth.dll` is placed correctly and matches your architecture (x64/x86).

---

## Contributing

- Fork this repository.
- Create a new branch: `git checkout -b fix/configure-mssql`.
- Add your changes.
- Commit your changes: `git commit -m "Added clear README for configuring MS SQL Server on Windows"`.
- Push to the branch: `git push origin fix/configure-mssql`.
- Submit a Pull Request.
