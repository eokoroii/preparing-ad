<h1>Active Directory Lab 1: Domain Controller and Client Setup</h1>

<h2>Project Summary</h2>
<p>
  In this lab, I set up a basic Active Directory environment using two virtual machines in Azure. The first VM runs Windows Server 2022 and acts as a Domain Controller (<code>DC-1</code>), which will also function as a DNS server. The second VM runs Windows 10 Pro (<code>Client-1</code>) and will join the domain managed by <code>DC-1</code>. This setup simulates a real-world scenario where client machines rely on a domain controller for authentication and DNS resolution.
</p>

<ul>
  <li><strong>Key Components:</strong>
    <ul>
      <li>Resource Group: <code>Active-Directory-Lab</code></li>
      <li>Virtual Network: <code>Active-Directory-VNet</code></li>
      <li>Domain Controller VM: <code>DC-1</code> (Windows Server 2022 Datacenter)</li>
      <li>Client VM: <code>Client-1</code> (Windows 10 Pro)</li>
      <li>Static IP configuration and custom DNS settings</li>
    </ul>
  </li>
  <li><strong>Goal:</strong> 
    Establish a foundational Active Directory environment in which the client can successfully communicate with the domain controller using custom DNS settings.
  </li>
</ul>

<hr />

<h2>Steps &amp; Screenshots</h2>
<ol>
  <li>
    <strong>Create Resource Group &amp; Virtual Network</strong><br />
    In the Azure portal, I created a Resource Group named <code>Active-Directory-Lab</code> and a Virtual Network called <code>Active-Directory-VNet</code> in the East 2 region.
    <br /><br />
    <p align="center">
      <img src="https://i.imgur.com/ZWukHYp.png" alt="Azure Resource Group creation" width="600" />
      <img src="https://i.imgur.com/Nn3DdbK.png" alt="Azure Virtual Network creation" width="600" />
      <br />
      <em>Figure 1.1 – Resource Group and Virtual Network setup.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Deploy the Domain Controller VM (DC-1)</strong><br />
    I created the first virtual machine, <code>DC-1</code>, using the Windows Server 2022 Datacenter image with at least 2 vCPUs. I set the administrator username to <code>labuser</code> and the password to <code>Cyberlab123!</code>. During the setup, I ensured that <code>DC-1</code> was connected to the <code>Active-Directory-VNet</code>.
    <br /><br />
    <p align="center">
      <img src="https://i.imgur.com/wdM2XcF.png" alt="Domain Controller VM creation" width="600" />
      <br />
      <em>Figure 1.2 – Creating the Domain Controller VM (DC-1).</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Deploy the Client VM (Client-1)</strong><br />
    Next, I created a second virtual machine named <code>Client-1</code> using the Windows 10 Pro image. I used the same credentials as for <code>DC-1</code> and made sure that <code>Client-1</code> was also connected to the <code>Active-Directory-VNet</code>.
    <br /><br />
    <p align="center">
      <img src="https://i.imgur.com/CPc9Cxs.png" alt="Client VM creation" width="600" />
      <br />
      <em>Figure 1.3 – Creating the Client VM (Client-1).</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Set Static IP for the Domain Controller</strong><br />
    With both VMs deployed, I configured <code>DC-1</code>'s network interface to use a static private IP address. This step is critical because the client will use this IP as its DNS server.
    <br /><br />
    <p align="center">
      <img src="https://i.imgur.com/0GgvBRj.png" alt="Setting static IP for DC-1" width="600" />
      <br />
      <em>Figure 1.4 – Configuring the NIC for <code>DC-1</code> with a static IP.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Log into the Domain Controller and Disable Windows Firewall</strong><br />
    I connected to <code>DC-1</code> using Remote Desktop (via its Public IP). Once logged in, I opened the Server Manager, then launched the Run dialog (<code>Win + R</code>) and typed <code>wf.msc</code> to access Windows Firewall. I disabled the firewall on the Domain, Private, and Public profiles to avoid any connectivity issues during the lab.
    <br /><br />
    <p align="center">
      <img src="https://i.imgur.com/FIBMRKo.png" alt="Remote Desktop into DC-1" width="600" />
      <br />
      <em>Figure 1.5 – Remote Desktop session on <code>DC-1</code>.</em>
    </p>
    <p align="center">
      <img src="https://i.imgur.com/80dniD2.png" alt="Disabling Windows Firewall on DC-1" width="600" />
      <br />
      <em>Figure 1.6 – Disabling Windows Firewall on <code>DC-1</code>.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Configure Client-1 to Use DC-1 as its DNS Server</strong><br />
    <ol type="a">
      <li>
        In the Azure portal, navigate to <code>DC-1 &gt; Networking</code> and copy the Domain Controller’s Private IP address (e.g., <code>10.0.0.4</code>).
      </li>
      <li>
        Navigate to <code>Client-1 &gt; Networking</code> and select its network interface configuration.
      </li>
      <li>
        Under the <em>DNS Servers</em> setting, change the option from <em>Inherit from virtual network</em> to <strong>Custom</strong>.
      </li>
      <li>
        Enter the Domain Controller’s Private IP address as the DNS server and save the changes.
      </li>
      <li>
        Restart Client-1 from the Virtual Machine overview to apply the new DNS settings.
      </li>
    </ol>
    <p align="center">
      <img src="https://i.imgur.com/sfLxUbV.png" alt="Configuring DNS on Client-1" width="600" />
      <br />
      <em>Figure 1.6 – Setting Client-1 DNS to point to DC-1’s private IP.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Verify Connectivity Between Client-1 and DC-1</strong><br />
    <ol type="a">
      <li>
        Open <strong>Remote Desktop Connection</strong> and log into Client-1 using its Public IP.
      </li>
      <li>
        Launch <strong>Windows PowerShell</strong> as Administrator.
      </li>
      <li>
        Run the command: <code>ping [DC-1's Private IP]</code> (e.g., <code>ping 10.0.0.4</code>) to test connectivity.
      </li>
      <li>
        Run <code>ipconfig /all</code> to verify that the DNS server for Client-1 is set to DC-1's Private IP address.
      </li>
    </ol>
    <p align="center">
      <img src="https://i.imgur.com/9Qu1Gz7.png" alt="Ping test from Client-1" width="600" />
      <img src="https://i.imgur.com/x3JDgGO.png" alt="Ping test from Client-1" width="600" />
      <br />
      <em>Figure 1.7 – Verifying connectivity and DNS settings on Client-1.</em>
    </p>
  </li>
</ol>

<hr />

<h2>Conclusion</h2>
<p>
  In this lab, I established the foundational components for an Active Directory environment by creating a Domain Controller (DC-1)
  and a client machine (Client-1) in Azure. By setting a static IP on DC-1 and configuring Client-1 to use DC-1 as its DNS server,
  we ensured that the client can accurately resolve domain names. Connectivity was confirmed via ping tests and IP configuration checks.
</p>
<p>
  This setup paves the way for future labs where the client will join the domain, and additional Active Directory functionalities will be implemented.
</p>
