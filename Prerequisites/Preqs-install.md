## Prerequisites and Install Process

1. **Prerequisites**  
- This guide assumes a moderate level of Linux, OS and Grafana experience.
- Familiarity with Cockpit for system configuration and VM management is helpful.
- The guide includes instructions for using Cockpit, but other tools or bare-metal setups can be used as well.
- If you're new to Linux or Cockpit, I suggest checking out the provided [links](../Additional-Resources/Cockpit-link.md).

Starting with bare metal servers for the Testpoints and Archive hosts is straight forward. You can then use the **_[3by3.json](../3by3.json)_** file to quickly get a working Grafana grid.

If you don't have a pile of hardware available but you'd like to see how the perfSONAR-Grafana tools work together, the rest of this is for you.

2. **System Requirements**  
- A host system- 4 cores, 16GB RAM, and 300GB of available storage.
- The system should have internet access at 1G or more for downloading dependencies.
- Install your preferred, modern, up-to-date OS. (Doesn't have to be on the PS list!)
- Install Cockpit and components (cockpit-machines) to manage virtual machines

3. **Overview of the Setup Process**  
Here's a high-level outline of the steps we’ll follow:
- Create a base VM with a supported OS (from the perfSONAR list).
- Clone the base VM to create one Testpoint node. Ensure it's fully configured and tested.
- Clone the known good Testpoint VM two more times, for a total of three Testpoint VMs.
- Use the Base VM to clone a new VM for the Archive and Grafana setup.
- Configure the Archive node and Grafana, then add the **_[3by3.json](../3by3.json)_**_ file to all nodes, via the 'psconfig remote add' command.
- Verify the system and ensure the Grafana dashboard starts populating.

4. **Set Up the Virtual Machines**  
(Review the [Network Setup](../Network-Details/Network-spec.md) page for needed details.)
- Create a "Base" VM on your host machine. The base VM runs a supported OS and 4 GB of RAM allocated.  Configure networking for _Direct attachment_ in Cockpit, ensuring that each VM can communicate with others on the same host. _This is critical for Testpoints to interact with the Archive and Grafana VMs._
- Clone the Base VM to create a Testpoint node. This will be (**ps01**).
- Once cloned, [install the Testpoint package](../Build-TP-Archive-Grafana-systems/Install-Testpoint.md), poweroff the VM.  
- Clone the Testpoint VM (**ps01**) two more times to create **ps02** and **ps03**.  Ensure each cloned VM is configured with the appropriate MAC address and IP as well as _Direct attachement_ status.
- Clone the Base VM again to create the Archive/Grafana VM (archive).  Increase the allocated RAM to 8GB, configure with the appropriate MAC address and IP as well as _Direct attachement_ status.
- Install the [Archive and Grafana components](../Build-TP-Archive-Grafana-systems/Install-Archive-Grafana.md)

When completed, visit the Grafana dashboard at `https://archive.test.net/grafana/dashboards`. If everything is set up correctly, you should begin seeing data populate into the dashboard. The latency and RTT grids populate first on the Grafana dashboard. The Grafana default historical time is 24 hours, so adjust it to 5-10 minutes for a quicker view of the data.

