# Tutorial on stress testing With Apache JMeter

This tutorial will guide you through downloading and setting up JMeter for load testing, including installing Java 17, configuring JAVA_HOME, and adding the Stepping Thread Group plugin. By the end, you’ll be able to conduct a load test on a chosen game webpage to analyze error rates and response times under stress.

---

## Step 1: Install Java 17

JMeter requires Java to run, so we’ll start by installing Java 17 and configuring the JAVA_HOME environment variable.

### For Windows:
1. **Download Java 17**:
   - Visit [Oracle's Java 17 Download Page](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html).
   - Choose the Windows installer (`.exe` file).
2. **Install Java 17**:
   - Run the downloaded `.exe` file and follow the installation instructions.
3. **Set JAVA_HOME**:
   - Open the Start menu, search for "Environment Variables," and open it.
   - Click on **Environment Variables**.
   - Under **System variables**, click **New**.
     - Variable name: `JAVA_HOME`
     - Variable value: Path to your Java installation directory (e.g., `C:\Program Files\Java\jdk-17`)
   - Click **OK** to save.
4. **Add Java to the PATH**:
   - In the **Environment Variables** window, find the **Path** variable under **System variables**, click **Edit**.
   - Add a new entry with `%JAVA_HOME%\bin`.
   - Click **OK** to save.

5. **Verify Java installation**:
   - Open Command Prompt (Win + R, then type `cmd` and press Enter).
   - Type `java -version` and `javac -version` to verify that Java is correctly installed and that it shows version 17.

### For macOS:
1. **Install Java 17 via Command Line**:
   - Open the Terminal.
   - Use the following command to install Java via Homebrew (if you have it installed):
     ```bash
     brew install --cask temurin
     ```
   - Alternatively, download the `.dmg` installer from [Oracle's Java 17 Download Page](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html), open it, and follow the installation instructions.
2. **Set JAVA_HOME**:
   - In Terminal, use the following command to set JAVA_HOME for Java 17:
     ```bash
     echo 'export JAVA_HOME=$(/usr/libexec/java_home -v 17)' >> ~/.zshrc
     source ~/.zshrc
     ```
3. **Verify Java installation**:
   - In Terminal, type `java -version` and `javac -version` to confirm that Java is installed and shows version 17.

---

## Step 2: Download and Install JMeter

1. **Download JMeter**:
   - Go to the official JMeter download page: [https://jmeter.apache.org/download_jmeter.cgi](https://jmeter.apache.org/download_jmeter.cgi).
   - Under **Binaries**, select the correct download link for your system:
     - **Windows**: Download the `.zip` file.
     - **macOS**: Download the `.tgz` file.

2. **Extract JMeter**:
   - **Windows**: Right-click the downloaded `.zip` file, select **Extract All**, and choose a location.
   - **macOS**: Use the following command to extract in Terminal:
     ```bash
     tar -xzf apache-jmeter-<version>.tgz
     ```
   - Replace `<version>` with the actual version number of JMeter.

### Additional Step: Install Stepping Thread Group Plugin

To add Stepping Thread Group to JMeter, you need to install an additional plugin.

1. **Download the Plugin**:
   - Go to [JMeter Plugins Downloads](https://jmeter-plugins.org/downloads/old/).
   - Download `JMeterPlugins-Standard-1.4.0.zip`.

2. **Install the Plugin**:
   - Extract the ZIP file.
   - Copy the `JMeterPlugins-Standard.jar` file from the extracted folder to your JMeter installation directory:
     - Paste it into `apache-jmeter-<version>/lib/ext`.

---

## Step 3: Running JMeter

### For Windows:
1. **Open the JMeter bin directory**:
   - Navigate to `apache-jmeter-<version>/bin`.
2. **Launch JMeter GUI**:
   - Double-click `jmeter.bat` to start the JMeter graphical user interface (GUI).
3. **Using the command line (optional)**:
   - ```bash
     cd apache-jmeter-<version>/bin
     ```
    - Run the following command:
     ```bash
     ./jmeter.bat
     ```

### For macOS:
1. **Open Terminal and navigate to the JMeter bin directory**:
   - ```bash
     cd apache-jmeter-<version>/bin
     ```
2. **Launch JMeter GUI**:
   - Run the following command:
     ```bash
     ./jmeter
     ```

---

## Step 4: Set Up a Load Test with Stepping Thread Group

### 1. Find a Game Webpage

Choose a game webpage that you’d like to test under load. This could be any interactive webpage that you have permission to test.

### 2. Configure the Stepping Thread Group

1. In JMeter, right-click **Test Plan** > **Add** > **Threads (Users)** > **jp@gc - Stepping Thread Group**.
   
2. Configure the Stepping Thread Group with the following settings:
   
![image](https://github.com/user-attachments/assets/f00ea521-2517-4f9a-8ea1-b139e4f4e0e6)
   - **This group will start**: Set the initial number of threads (users) that you want to start with. In this example, set it to `10`.
   - **First, wait for**: Set the initial delay before starting the threads. Set it to `0` seconds to start immediately.
   - **Then start**: Leave this set to `0` if you don't want an additional batch of threads to start after a delay.
   - **Next, add**: Set this to `1`, which means adding 1 new user at each interval.
   - **Threads every**: Set to `6` seconds, so a new user is added every 6 seconds.
   - **Using ramp-up**: Set this to `1` second, which specifies that each added user will ramp up over 1 second.
   - **Then hold load for**: Set to `60` seconds. This means once all threads are running, they will continue running for 1 minute (steady load phase).
   - **Finally, stop**: Set this to `1` thread every `6` seconds, which specifies that after the steady load phase, the threads will gradually stop at a rate of 1 user every 6 seconds.

### 3. Add an HTTP Request to Test the Webpage

1. In the Stepping Thread Group, right-click > **Add** > **Sampler** > **HTTP Request**.
2. Configure the HTTP Request:
   - **Server Name or IP**: Enter the domain of the game webpage (e.g., `example.com`).
   - **Path**: Enter the path of the game webpage (e.g., `/game` if the full URL is `https://example.com/game`).
   - Leave **Port** empty unless you need to specify a port number.

### 4. Add a Duration Assertion

1. Right-click the HTTP Request > **Add** > **Assertions** > **Duration Assertion**.
2. Set **Duration in milliseconds** to `15000` (15 seconds).
   - This will flag any request taking longer than 15 seconds as an error.

### 5. Add Listeners to View Results

1. Right-click the Stepping Thread Group > **Add** > **Listener** > **Summary Report**.
   - This report will show the **average response time** and **error rate** for each test.
2. Optionally, add a **View Results Tree** listener to see detailed request/response data.
3. **Add Simple Data Writer**:
   - To save detailed information for each request, add **Simple Data Writer**:
     - In the **Simple Data Writer** configuration, specify a file path to save results (e.g., `results.csv`).
     - Click **Configure** and select fields like **Time Stamp**, **Elapsed Time**, **Label**, **Response Code**, and **Success**.

---

## Step 5: Run the Test and Analyze Results

1. **Run the Test**:
   - Click the green **Start** button in the JMeter toolbar.

2. **Monitor Results**:
   - **Summary Report**: This will display the average response time, error rate, throughput, and other statistics for the load test.
     ![image](https://github.com/user-attachments/assets/3b399aaa-85cb-4abf-8f11-51ad22c7afbd)

   - **View Results Tree**: Use this to view detailed request and response information.
     ![image](https://github.com/user-attachments/assets/dc5436b3-98e5-408a-a640-6eaa52239668)

   - **Simple Data Writer**: Open the CSV file created by Simple Data Writer to view each request’s details, including timestamp, response time, success status, and any errors.
     ![image](https://github.com/user-attachments/assets/836cd11b-a6ed-49d3-b9bb-3d2858237ddd)




By following these steps, you’ll be able to run a load test on a webpage and analyze how it handles concurrent users, using JMeter in GUI.
