# Complete CloudSim Setup Guide — IntelliJ IDEA (Mac)

---

## PART 1 — Download Everything You Need

### Step 1: Download Java JDK

1. Go to **https://www.oracle.com/java/technologies/downloads/**
2. Select **Java 21** (latest LTS) — macOS — **ARM64 DMG** (M1/M2 Mac) or **x64 DMG** (Intel Mac)
3. Download and open the `.dmg` file
4. Double click the `.pkg` inside → click through the installer → enter your Mac password when asked
5. Verify in Terminal:
```bash
java -version
```
Should print:
```
java version "21.x.x" ...
```

> ✅ CloudSim 3.0.3's JAR is pre-compiled Java 8 bytecode. Java 21's JVM runs it fine because Java is always backwards compatible. You do NOT need to install Java 8 separately.

---

### Step 2: Download IntelliJ IDEA

1. Go to **https://www.jetbrains.com/idea/download/**
2. Select **macOS**
3. Download **Community Edition** (free) — `.dmg` file
4. Open the `.dmg` → drag IntelliJ IDEA into your Applications folder
5. Open it from Applications
6. On first launch — click **Don't Send** on data sharing prompt
7. Select UI theme (your preference) → click **Next** → click **Start using IntelliJ IDEA**

---

### Step 3: Download CloudSim

1. Go to **https://github.com/Cloudslab/cloudsim/releases/tag/cloudsim-3.0.3**
2. Scroll down to **Assets**
3. Download **Source code (zip)**
4. Go to your Downloads folder → double click the zip → it extracts to a folder called `cloudsim-3.0.3`
5. Move this folder somewhere permanent — do NOT leave it in Downloads (e.g. move to Desktop or Documents)

---

### Step 4: Locate the JAR Files

Open Terminal and run:
```bash
ls ~/Desktop/cloudsim-3.0.3/jars/
```

You should see:
```
cloudsim-3.0.3.jar
cloudsim-3.0.3-sources.jar
cloudsim-examples-3.0.3.jar
cloudsim-examples-3.0.3-sources.jar
```

**You only need these 2:**
- ✅ `cloudsim-3.0.3.jar`
- ✅ `cloudsim-examples-3.0.3.jar`
- ❌ anything ending in `-sources.jar` — ignore these (they're just for reading source code)

---

### Step 5: Download the Missing commons-math3 JAR

CloudSim depends on this library but it's not included in the zip. Download it manually:

```bash
curl -o ~/Desktop/cloudsim-3.0.3/jars/commons-math3-3.2.jar \
  https://repo1.maven.org/maven2/org/apache/commons/commons-math3/3.2/commons-math3-3.2.jar
```

Verify it downloaded:
```bash
ls ~/Desktop/cloudsim-3.0.3/jars/
```

Now you should see `commons-math3-3.2.jar` in the list.

**Final list of JARs you will import:**
1. `cloudsim-3.0.3.jar`
2. `cloudsim-examples-3.0.3.jar`
3. `commons-math3-3.2.jar`

---

## PART 2 — Create the IntelliJ Project

### Step 6: Create a New Project

1. Open IntelliJ IDEA
2. Click **New Project** on the welcome screen
   - If a project is already open: **File → New → Project**
3. Fill in the form:
   - **Name:** `CloudSimProject` (or whatever you like)
   - **Location:** choose where to save it (e.g. Desktop)
   - **Language:** `Java`
   - **Build system:** `IntelliJ` ← important, do NOT choose Maven or Gradle
   - **JDK:** click the dropdown → select the Java 21 you installed
     - If it doesn't appear, click **Add JDK** → navigate to `/Library/Java/JavaVirtualMachines/` → select the JDK folder
4. **Uncheck** "Add sample code" if the checkbox exists
5. Click **Create**

IntelliJ creates the project and opens it. You'll see a `src` folder in the left panel.

---

### Step 7: Understand the Project Structure

After creation, your project looks like this:
```
CloudSimProject/
    ├── src/              ← all your Java code goes here
    ├── CloudSimProject.iml
    └── .idea/            ← IntelliJ settings (ignore this)
```

---

## PART 3 — Import the JAR Files

### Step 8: Open Project Structure

1. Go to **File → Project Structure**
   — OR press **Cmd + ;** (semicolon)

This opens the Project Structure window.

---

### Step 9: Add JARs as Dependencies

1. In the left sidebar of Project Structure, click **Modules**
2. Click the **Dependencies** tab (top right area)
3. Click the **+** button at the bottom of the dependencies list
4. Select **JARs or Directories...**
5. In the file picker that opens:
   - Navigate to your jars folder — e.g. `Desktop/cloudsim-3.0.3/jars/`
   - Select these 3 files (**hold Cmd to select multiple**):
     - `cloudsim-3.0.3.jar`
     - `cloudsim-examples-3.0.3.jar`
     - `commons-math3-3.2.jar`
   - Click **Open**
6. All 3 JARs appear in the dependencies list
7. Check that the **Scope** column says **Compile** for each one
   - If any say **Test** or **Runtime**, click that word and change it to **Compile**
8. Click **Apply** → click **OK**

---

### Step 10: Verify the JARs are Loaded

In the left panel of IntelliJ, scroll down to the bottom. You should see:

```
External Libraries
    ├── cloudsim-3.0.3
    ├── cloudsim-examples-3.0.3
    └── commons-math3-3.2
```

If you see all 3 there, the JARs are successfully imported.

---

## PART 4 — Configure IntelliJ for CloudSim

### Step 11: Set Java Language Level

1. Go to **File → Project Structure → Project** (in the left sidebar)
2. Set **SDK:** to your Java 21
3. Set **Language level:** to `8 - Lambdas, type annotations etc.`
   - CloudSim was written for Java 8. Setting this level prevents IntelliJ from showing false errors about compatibility
4. Click **Apply → OK**

---

### Step 12: Set Compiler Target to Java 8

1. Go to **File → Settings** (on Mac: **IntelliJ IDEA → Preferences**)
2. Navigate to **Build, Execution, Deployment → Compiler → Java Compiler**
3. Under **Project bytecode version** — set to `8`
4. Click **Apply → OK**

---

### Step 13: Add JVM Flags (Very Important for Java 9+)

CloudSim was written before Java introduced the module system in Java 9. On Java 21, without these flags, you'll get `InaccessibleObjectException` errors at runtime.

1. Go to **Run → Edit Configurations...**
2. If no configuration exists yet:
   - Click **+** (top left) → select **Application**
   - Give it a name, e.g. `Main`
3. Find the **VM options** field
   - If you don't see it, click **Modify options** → check **Add VM options**
4. Paste this into the VM options field:
```
--add-opens java.base/java.lang=ALL-UNNAMED
--add-opens java.base/java.util=ALL-UNNAMED
--add-opens java.base/java.io=ALL-UNNAMED
--add-opens java.base/java.lang.reflect=ALL-UNNAMED
--add-opens java.base/java.text=ALL-UNNAMED
--add-opens java.base/java.util.concurrent=ALL-UNNAMED
```
5. In the **Main class** field — click the folder icon and select your main class (you'll set this up in Part 5)
6. Click **Apply → OK**

---

### Step 14: Set Encoding to UTF-8

1. Go to **IntelliJ IDEA → Preferences → Editor → File Encodings**
2. Set all three dropdowns to **UTF-8**:
   - Global Encoding: `UTF-8`
   - Project Encoding: `UTF-8`
   - Default encoding for properties files: `UTF-8`
3. Click **Apply → OK**

---

## PART 5 — Write Your First CloudSim Program

### Step 15: Create a Package

1. In the left panel, right-click the **src** folder
2. **New → Package**
3. Type a package name — use lowercase, no spaces (e.g. `simulation`)
4. Press Enter

---

### Step 16: Create a Java Class

1. Right-click your new package → **New → Java Class**
2. Type the class name — use PascalCase (e.g. `MySimulation`)
3. Press Enter

IntelliJ creates the file and opens it:
```java
package simulation;

public class MySimulation {
}
```

---

### Step 17: The Minimum CloudSim Program That Runs

Every CloudSim program must have these elements at minimum. This is the base template you build any simulation on top of:

```java
package simulation;

import org.cloudbus.cloudsim.*;
import org.cloudbus.cloudsim.core.CloudSim;
import org.cloudbus.cloudsim.provisioners.*;

import java.util.*;

public class MySimulation {

    public static void main(String[] args) throws Exception {

        // 1. ALWAYS first — initialise the CloudSim engine
        int numUsers = 1;
        Calendar calendar = Calendar.getInstance();
        boolean traceFlag = false;
        CloudSim.init(numUsers, calendar, traceFlag);

        // 2. Create a datacenter — required for simulation to run
        Datacenter datacenter = createDatacenter("Datacenter_0");

        // 3. Create a broker — required even if you don't submit tasks
        DatacenterBroker broker = new DatacenterBroker("Broker_0");

        // 4. ── YOUR SIMULATION LOGIC GOES HERE ──
        // e.g. create VMs, submit cloudlets, build topology etc.

        // 5. ALWAYS last — start and stop the engine
        CloudSim.startSimulation();
        CloudSim.stopSimulation();

        // 6. Print your results here
        System.out.println("Simulation complete.");
    }

    private static Datacenter createDatacenter(String name) throws Exception {

        // Create hosts
        List<Host> hostList = new ArrayList<>();

        List<Pe> peList = new ArrayList<>();
        peList.add(new Pe(0, new PeProvisionerSimple(1000)));

        Host host = new Host(
                0,
                new RamProvisionerSimple(2048),
                new BwProvisionerSimple(10000),
                1_000_000,
                peList,
                new VmSchedulerTimeShared(peList)
        );
        hostList.add(host);

        // Datacenter specs
        DatacenterCharacteristics characteristics = new DatacenterCharacteristics(
                "x86", "Linux", "Xen", hostList,
                0.0, 0.001, 0.005, 0.0001, 0.0
        );

        return new Datacenter(
                name, characteristics,
                new VmAllocationPolicySimple(hostList),
                new LinkedList<>(), 0
        );
    }
}
```

---

### Step 18: Run the Program

1. Click the **green ▶ button** in the top right toolbar
   — OR right-click inside the code → **Run 'MySimulation.main()'**
   — OR press **Ctrl + Shift + R**

2. The **Run panel** opens at the bottom. You should see:
```
Simulation complete.
```

If you see this — CloudSim is working perfectly.

---

## PART 6 — What To Do Next for Any Specific Task

Once the base template above runs, here is what you add depending on your task:

---

### For Topology Simulation (Mesh, Star, Ring etc.)

- Change `Datacenter` → `NetworkDatacenter`
- Change `Host` → `NetworkHost`
- Add these imports:
```java
import org.cloudbus.cloudsim.network.datacenter.*;
```
- After creating the datacenter, write your topology loop

---

### For VM Creation

Add this after the broker:
```java
List<Vm> vmList = new ArrayList<>();

Vm vm = new Vm(
    0,              // VM id
    broker.getId(), // owner
    1000,           // MIPS
    1,              // number of CPUs
    512,            // RAM in MB
    1000,           // bandwidth
    10000,          // storage
    "Xen",          // VMM
    new CloudletSchedulerTimeShared()
);

vmList.add(vm);
broker.submitVmList(vmList);
```

---

### For Submitting Tasks (Cloudlets)

```java
List<Cloudlet> cloudletList = new ArrayList<>();

Cloudlet cloudlet = new Cloudlet(
    0,          // cloudlet id
    10000,      // length in MI (million instructions)
    1,          // number of CPUs needed
    300,        // input file size in bytes
    300,        // output file size in bytes
    new UtilizationModelFull(),
    new UtilizationModelFull(),
    new UtilizationModelFull()
);

cloudlet.setUserId(broker.getId());
cloudletList.add(cloudlet);
broker.submitCloudletList(cloudletList);
```

---

### For Getting Results After Simulation

```java
List<Cloudlet> finishedCloudlets = broker.getCloudletReceivedList();

for (Cloudlet c : finishedCloudlets) {
    System.out.println("Cloudlet " + c.getCloudletId()
        + " | Status: " + c.getStatus()
        + " | Start: " + c.getExecStartTime()
        + " | Finish: " + c.getFinishTime());
}
```

---

## PART 7 — Troubleshooting Every Common Error

| Error | Cause | Fix |
|---|---|---|
| `Cannot resolve symbol 'CloudSim'` | JAR not imported | Redo Step 9 |
| `InaccessibleObjectException` | Java 9+ module restriction | Add JVM flags from Step 13 |
| `ClassNotFoundException` | Wrong JAR selected | Make sure `cloudsim-3.0.3.jar` not the sources one |
| `NullPointerException` on init | CloudSim.init() not called first | Move `CloudSim.init()` to the very first line of main |
| Red underlines everywhere | Language level mismatch | Redo Step 11 |
| Program runs but prints nothing | Wrong class selected to run | Right-click the correct class → Run |
| `NoClassDefFoundError: commons` | commons-math3 JAR missing | Redo Step 5 and Step 9 |
| Build successful but no output | Missing `startSimulation()` | Add `CloudSim.startSimulation()` before your print statements |

---

## Quick Reference — The Order That Never Changes

```
SETUP (once only):
  Download JDK → Download IntelliJ → Download CloudSim zip
  → Get commons-math3 JAR → Create IntelliJ project
  → Import all 3 JARs → Set language level to 8
  → Add JVM flags → Set encoding UTF-8

EVERY PROGRAM (in this exact order):
  1. CloudSim.init()
  2. createDatacenter()
  3. new DatacenterBroker()
  4. Your specific logic (VMs, cloudlets, topology)
  5. CloudSim.startSimulation()
  6. CloudSim.stopSimulation()
  7. Print results
```

This order is not optional — CloudSim's internal engine enforces it. Anything out of order will throw an exception.
