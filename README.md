*This project has been created as part of the 42 curriculum by yzhang2.*

# NetPractice

## Description
NetPractice is a general practical exercise designed to introduce the basics of computer networking. The goal is to solve networking problems by configuring IP addresses, subnet masks, and routing rules to make a simulated network function properly. 

Through 10 levels of increasing difficulty, this project covers essential TCP/IP concepts, including how packets are forwarded through routers and how gateways connect different subnets.

## Instructions

### How to run the training interface
You must use a local web server to access the NetPractice pages.
- **Option A:** Run the provided `run.sh` script in your terminal.
- **Option B (Manual):** If the script doesn't work, run `python3 -m http.server 49242` and navigate to `http://localhost:49242` in your web browser.

### Solving and Exporting
- Enter your intranet login in the **Training** tab to load your personal configuration.
- Adjust the configuration fields (white boxes) until the network status shows it is working correctly.
- Use the **[Check again]** button to verify your solution.
- Once a level is completed, you **must** click the **[Get my config]** button to export your configuration file.

### 3. Submission Requirements
- You are required to submit **10 configuration files** (one per level: `level1.json` to `level10.json`).
- These 10 files must be placed at the **root** of your Git repository.
- A `README.md` (this file) must also be present at the root.
  
### Common log messages (what they usually mean)

- **`invalid IP address`**  
  You used a network/broadcast address, or the IP does not fit the interface mask.

- **`no interface for gateway X`**  
  Your chosen gateway is not on any directly-connected subnet of that device.

- **`destination does not match any route`**  
  The routing table has no matching route (missing default route or missing specific route).

- **`multiple interface match`**  
  Overlapping subnets across interfaces cause ambiguity; fix masks/subnet plan.

- **`loop detected`**  
  Packet is bouncing due to wrong/ambiguous routing; check default routes and overlaps.


## How I approached each level 

### Step A — Split the diagram into subnets
- Every **switch LAN** (devices on the same switch/broadcast domain) should be **one subnet**.
- Every **router-to-router link** should be its own subnet (**/30** is often the simplest).
- Every **router-to-internet link** is its own subnet.

### Step B — Validate IP addressing inside each subnet
For each subnet:
- All devices in the subnet share the **same mask**.
- No **duplicate IPs**.
- Do not use:
  - **Network address** (host bits all `0`)
  - **Broadcast address** (host bits all `1`)

### Step C — Set gateways on hosts
- If the destination is outside the host subnet, the host uses its **routing table**.
- Usually: set a **default route**:
  - `0.0.0.0/0 => <gateway IP>`
- The gateway IP must be in the **same subnet** as the host interface, otherwise the host cannot reach the gateway.

### Step D — Add router routes
Routers forward between subnets using their routing tables:
- **Specific routes** for internal networks (e.g., `146.9.141.128/26 => <next hop>`)
- A **default route** for everything else (e.g., `0.0.0.0/0 => <upstream gateway>`)
- When multiple routes match, the router chooses the **most specific** (longest prefix).

### Step E — Verify return path (reverse way)
- Forward reachability is not enough.
- If you can send packets out but replies never come back, you are missing a **return route**.
- In NetPractice, “Internet” often only knows specific return routes (given in the Internet box). Your internal addressing must be consistent with that.


## Resources

### Key Networking Concepts
The following concepts were studied and applied during this project:
- **TCP/IP Addressing:** IPv4 structure and host identification.
- **Subnet Masks:** Defining network boundaries and calculating host ranges.
- **Default Gateways:** Configuring the route for traffic leaving the local network.
- **Routers and Switches:** Understanding Layer 3 (routing) and Layer 2 (switching) operations.
- **OSI Layers:** Overview of the standard model for network communication.

### Articles / References
- Default gateway (Wikipedia): https://en.wikipedia.org/wiki/Default_gateway
- Subnetting & hosts formula (TechTarget): https://www.techtarget.com/searchnetworking/tip/IP-addressing-and-subnetting-Calculate-a-subnet-mask-using-the-hosts-formula

### Videos
- https://www.youtube.com/watch?v=s_Ntt6eTn94
- https://www.youtube.com/watch?v=JOomC1wFrbU
- https://www.youtube.com/watch?v=pCcJFdYNamc
- https://www.bilibili.com/video/BV1xu411f7UW/
- https://www.bilibili.com/video/BV1HZ4y1v7xP/

### AI Usage 
AI was utilized in this project to assist with:
- **Technical Explanations:** To clarify networking theory (CIDR, subnet ranges, network/broadcast rules, default gateway behavior). Explaining the mathematical logic behind subnet masks and CIDR notation.
- **Troubleshooting:** Interpreting error logs such as "destination does not match any route" or "loop detected" to identify configuration gaps.
