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
      <img src="YOUR_IMAGE_URL_HERE" alt="Azure Resource Group and Virtual Network creation" width="600" />
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
      <img src="YOUR_IMAGE_URL_HERE" alt="Domain Controller VM creation" width="600" />
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
      <img src="YOUR_IMAGE_URL_HERE" alt="Client VM creation" width="600" />
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
      <img src="YOUR_IMAGE_URL_HERE" alt="Setting static IP for DC-1" width="600" />
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
      <img src="YOUR_IMAGE_URL_HERE" alt="Remote Desktop into DC-1" width="600" />
      <br />
      <em>Figure 1.5 – Remote Desktop session on <code>DC-1</code>.</em>
    </p>
    <p align="center">
      <img src="YOUR_IMAGE_URL_HERE" alt="Disabling Windows Firewall on DC-1" width="600" />
      <br />
      <em>Figure 1.6 – Disabling Windows Firewall on <code>DC-1</code>.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Configure Client DNS Settings to Use the Domain Controller</strong><br />
    For <code>Client-1</code> to join the domain, its DNS settings need to point to <code>DC-1</code>. First, I retrieved the private IP address of <code>DC-1</code> from the Azure portal (e.g., <code>10.0.0.4</code>). Then, I navigated to <em>Client-1 &gt
