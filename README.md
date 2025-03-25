First of all, we need to get to know our network. For this  , we see the Subnets in the Network and their information from the Interfaces section.
 
![image](https://github.com/user-attachments/assets/e1fde004-3b95-4b75-9635-d253cd30148e)

Then we see our Devices in the Network and their information from Dashboard>Status>DHCP.

![image](https://github.com/user-attachments/assets/0a9dc6a4-909f-446f-a42f-fa6d51f93db1)

After learning the Subnet IPs and  the IPs of the Devices and  drawing the Network Topology, we can proceed to the next step required to write a Policy.

![image](https://github.com/user-attachments/assets/d766f70f-0f61-4409-83e2-ef82fb942f85)
 
Before we start writing a Policy in the Firewall, we must define the Device or Address related to that Policy. If we have not already defined the data we will need when writing the Policy, the Policy cannot be fully created.

Now, in order for us to write Policy, these Subnet IPs and Device IPs must be registered in the Address book. We go to the Policy & Objects > Address section, we look at them, if they are not registered, we should not save them. 

### Adding an Address

To do this, we go to Policy & Objects > Adresses. We click on the Create New button at the top left.
 
![image](https://github.com/user-attachments/assets/a2b1eed4-b877-42e7-a23e-6d1ae118387f)

![image](https://github.com/user-attachments/assets/0aa5d427-9eb5-4139-b97f-2c0bc3a4d432)

![image](https://github.com/user-attachments/assets/deffa711-de81-4f86-a084-fef108c47c74)

In this way, we identify our devices using IP addresses.

 ![image](https://github.com/user-attachments/assets/8e3eb6b8-c51d-430d-93bd-bb305b20ac1e)

After all our Interface, Subnet and Device definitions are finished, we are ready to write Policy.

![image](https://github.com/user-attachments/assets/d0f3d0e5-d121-40a1-84ba-dfa9d555590f)

- Incoming Interface -> Source Subnet
- Outgoing Interface -> Destination Subnet
- Source -> Devices in the Source 
- Destination -> Devices in the Destination Subnet
- Schedule -> We set the time when this Policy will run
- - Service -> Services that are required for this Policy to work
  - If the service we need is not on the list
  - For this:
In the Services section, we say Create new and create it as we want.

![image](https://github.com/user-attachments/assets/0e995bb1-d918-4afc-98a6-a5ff2e40f211)

- Action : ACCEPT

- NAT -> No need to open NAT if this Policy is not going to go to the Internet.
- Security Profiles -> If we want to use the Modules or Filters here, we activate it. By default, there is 1 defined, but we can create an optional one by going to the relevant tab in the Security Profiles section on  the left and clicking the Create New  button.
 
 ![image](https://github.com/user-attachments/assets/0ab8cea4-203d-4d44-8d46-7d8e66ff03a6)

 ![image](https://github.com/user-attachments/assets/ceb24fb9-9e7f-4ecf-9b95-5a375d3cbd07)

![image](https://github.com/user-attachments/assets/2f3961a1-6cfc-4e3b-8df4-d4b746b0f99e)

 ![image](https://github.com/user-attachments/assets/e7e6f9d8-54a9-4787-ba47-6b4d45493b6c)

 ![image](https://github.com/user-attachments/assets/31a5e65e-f7c8-40ac-8fca-cca1feb57b68)

![image](https://github.com/user-attachments/assets/a1b052e9-31f5-4eda-8832-5b6809e2dbc9)

After setting this Module or Filters, we need to set the SSL Inspection  option in the Policy.

![image](https://github.com/user-attachments/assets/15247b20-bfa6-4b65-8957-7d59938033ff)

When creating a Policy in the FortiGate firewall, SSL Inspection options are used to analyze whether the traffic is encrypted (HTTPS, TLS, SSL) and to examine its content when necessary. 

Using SSL Inspection, FortiGate can detect malicious activity, threats within encrypted traffic, and inappropriate content.

## FortiGate SSL Inspection Options and Their Meaning
### no-Inspection
- SSL traffic is not analyzed at all.
- Connections encrypted with HTTPS or TLS are forwarded directly.
- Disadvantage: It can't detect encrypted threats or inappropriate content.

### Certificate Inspection
- It does not fully open SSL/TLS traffic, it only checks the certificate information.
- It can prevent invalid or malicious certificates by performing certificate validation.
- Disadvantage: Traffic content is not analyzed, encrypted malicious content can be missed.

### deep-Inspection
- It fully analyzes the content by opening inbound and outbound SSL/TLS traffic .
- Security policies such as web filtering, IPS, DLP, antivirus also work on SSL traffic.
- Disadvantage: It can cause a loss of performance.
- An "Untrusted Certificate" error may occur on the user side, so the FortiGate CA certificate must be distributed to clients. Otherwise, an "Insecure Connection" error may appear in browsers.

### Custom-Inspection
- Traffic-specific inspection is carried out with user-defined settings.
- Detailed controls such as bypass for specific websites or applications, full SSL analysis for certain protocols can be provided.
- It provides flexibility, but  if not configured correctly, security vulnerabilities can occur.

![image](https://github.com/user-attachments/assets/30c9f543-c81f-41a7-9316-9726e4e23cdb)

The next step is to set which log record will be kept. There  are two options here: Security Event and All Sessions. If we choose Security Event, if there is a security-related situation while this Policy is running, it will only keep its Logs. The All Sessions option will keep all Logs.

Finally,  before pressing the OK button, if we want to use this Policy immediately, we need to activate the Enable this policy option at the bottom, otherwise the Policy will occur but it will not work.

We can check whether the Policies we have created are working correctly by checking their Logs. For this, we look at the Log & Report  section on the left.

The Memory, Disk, and FortiGate Cloud  options in the FortiGate > Log & Report section determine where the logs are stored and how they are managed.

### Memory
- Logs are kept on FortiGate's RAM.  
- It is generally used for temporary logging. When the device restarts, the logs are deleted.  
- Advantage: Provides fast access, no disk usage required.  
- Disadvantage: Logs are not permanent, they disappear if the device shuts down.  
- When to use it?
  - To analyze short-term events (e.g., live traffic monitoring).  
  - If no log will be sent to the disk or external log server.

### Disk
- Logs are saved on the internal disk of the FortiGate device.  
- Advantage: Logs are permanent and are not deleted even if the device restarts.  
- Disadvantage: On low-capacity devices, disk space can fill up quickly.  
  - Intensive logging can affect disk performance.  
- When to use it?
  - When the device needs to keep long-term logs in itself.  
  - When a central server is not used for log analysis.  
Note: Not all FortiGate models support disk logging. It can be used on models with disk capacity.

### FortiGate Cloud
- Logs are saved on FortiGate Cloud (Fortinet's cloud-based log management service).  
-Advantage:  
  - Logs are remotely accessible and stored securely.  
  - It is suitable for long-term analysis without hardware limitations.  
  - Even if the FortiGate device fails, access to the logs continues.  
- Disadvantage: It is dependent on the internet connection. When the connection is lost, logs may not be saved instantly.  
  - Paid FortiGate Cloud plans may be required.  
- When to use it? 
  - When central log management is required.  
  - When the physical capacity of the device is not enough (there is no disk or memory is limited).  
  - To collect the logs of multiple FortiGate devices in one place.
