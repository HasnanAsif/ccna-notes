# **5.0 Security Fundamentals**

## **5.1 Define key security concepts (threats, vulnerabilities, exploits, and mitigation techniques)**

Threats 
- A potential danger that can exploit vulnerabilities to harm systems, data, or networks.
- Examples
  - Malware (viruses, worms, ransomware)
  - Phishing attacks (social engineering)
  - DDoS attacks (disrupting services)

Vulnerabilities 
- A weakness or flaw in hardware, software, or processes that can be exploited by a threat.
- Examples:
  - Unpatched software or outdated systems
  - Misconfigured firewalls or access control lists
  - Weak passwords or lack of encryption

Exploits 
- The use of a vulnerability by an attacker to gain unauthorized access or cause damage.
- Examples:
  - Buffer overflow attacks to crash a system or gain control
  - SQL injection to manipulate databases
  - Zero-day exploits targeting unknown vulnerabilities

Mitigation Techniques  
- Strategies to reduce risks and protect systems from threats and exploits.
  - Patch management: Regularly updating and fixing vulnerabilities.
  - Firewalls and IPS: Filtering traffic and blocking malicious activity.
  - Encryption: Protecting sensitive data in transit and at rest.
  - Strong authentication: Using multi-factor authentication (MFA).
  - Security policies and training: Educating users to identify phishing and social engineering.

## **5.2 Describe security program elements (user awareness, training, and physical access control)**

User Awareness 
- Educating users to recognize security threats and follow best practices.
- Purpose: Reduces human error and increases vigilance against common attacks.
- Examples:
  - Recognizing phishing emails and social engineering tactics.
  - Identifying suspicious links or attachments.
  - Following strong password policies.
  - Awareness of safe browsing habits.

- Training 
- Structured programs to teach employees proper security procedures and responses.
- Purpose: Ensures users and IT staff have the necessary skills to protect systems and data.
- Examples:
  - Hands-on training for incident response (e.g., ransomware attacks).
  - Workshops on using multi-factor authentication (MFA).
  - Simulated phishing campaigns to test and improve user awareness.
  - Regular security compliance training (e.g., HIPAA, PCI-DSS).

- Physical Access Control 
- Security measures to prevent unauthorized physical access to systems, devices, or locations.
- Purpose: Protects critical infrastructure and prevents tampering with equipment.
- Examples:
  - Access badges and biometric scanners for restricted areas.
  - Surveillance systems (CCTV) to monitor physical spaces.
  - Use of locked server rooms and data centers.
  - Implementing security guards or access logs.

## **5.3 Configure and verify device access control using local passwords**

- Set a Console Password
  - `line console 0`
  - `password pass123`
  - `login`

- Set VTY Line Password (Telnet/SSH)
  - `line vty 0 4`
  - `password pass123`
  - `login`

- Set the Enable (Privileged EXEC Mode) Password
  - `enable password pass123`

- Set the Enable Secret (Encrypted Password)
  - `enable secret pass123`

- Verify Device Access Control
  - `show running-config | include password | secret`
  - The enable secret will appear encrypted, while other passwords will remain visible if not encrypted

- Verification commands
  - `show running-config` : verifies passwords and access lines
  - `show users` : Displays active sessions on the device

## **5.4 Describe security password policies elements, such as management, complexity, and password alternatives (multifactor authentication, certificates, and biometrics)**

- Password Management : Practices and tools used to ensure passwords are secure and managed properly.
- Key Elements:
  - Password Expiration: Regularly require users to change passwords
  - Password History: Prevent reuse of previous passwords
  - Lockout Policies: Disable accounts temporarily after a set number of failed login attempts
  - Secure Storage: Hash and salt passwords to store securely
  - Password Management Tools: Use password managers for secure storage and generation

- Password Complexity : Policies that require strong, hard-to-guess passwords.
- Key Elements:
  - Length: Minimum of 8-12 characters
  - Composition: Require a mix of:
    - Uppercase letters (A-Z)
    - Lowercase letters (a-z)
    - Numbers (0-9)
    - Special characters (!, @, #, $)
  - Avoid Dictionary Words: Prevent passwords based on common words or predictable patterns.
  - Randomization: Encourage users to use complex and unique passwords

- Password Alternatives : To enhance security, alternatives like multi-factor authentication (MFA), certificates, and biometrics are used

  |Alternative|Definition|Examples|
  | :- | :- | :- |
  |Multifactor Authentication|Requires multiple forms of verification for access.|Password + SMS code, password + authenticator app.|
  |Certificates|Digital certificates act as credentials to verify identity and access rights.|SSL certificates for web login, VPN client certs.|
  |Biometrics|Use of unique biological traits for authentication.|Fingerprint scan, facial recognition, retina scan.|

## **5.5. Describe IPsec remote access and site-to-site VPNs**

- IPsec (Internet Protocol Security) is a suite of protocols used to secure communications over IP networks. It ensures confidentiality, integrity, and authentication of data through encryption.

- IPsec Remote Access VPN : Allows individual users (e.g., employees working remotely) to securely connect to a private network using IPsec.
- How it Works:
  - Users initiate the VPN connection using a VPN client.
  - The connection is encrypted via IPsec to ensure secure communication.
  - Authentication mechanisms like usernames, passwords, or certificates verify the user.
- Key Features:
  - Flexibility: Users can connect from any location.
  - Client-Based: Requires VPN client software.
  - Secure Encryption: Protects transmitted data.
  - Common Protocols: ESP (Encapsulating Security Payload) and ISAKMP/IKE (key exchange).
- A
- IPsec Site-to-Site VPN : Establishes a secure, permanent tunnel between two or more networks (e.g., branch offices and headquarters).
- How it Works:
  - Routers or firewalls on both ends of the connection are configured with IPsec settings.
  - Data passing between the two networks is encrypted using IPsec.
  - Tunnel is established automatically and remains active.
- Key Features:
  - Network-to-Network: Connects entire networks instead of individual clients.
  - Always-On: Tunnel is typically up continuously.
  - Secure Communication: Data encryption and authentication.
  - Scalability: Ideal for connecting multiple branch offices.
  - Common Protocols: IKE Phase 1 (secure key exchange) and IKE Phase 2 (data encryption).

- Benefits of IPsec VPNs
  - Data Confidentiality: Encrypts data to prevent unauthorized access.
  - Data Integrity: Ensures data has not been tampered with.
  - Authentication: Verifies the identities of communicating parties.
  - Cost-Effective: Leverages public internet instead of expensive private links.
  - Scalable: Supports remote users and multi-site connections.

- By using IPsec Remote Access VPNs for users and Site-to-Site VPNs for network-to-network communication, organizations can ensure secure and reliable data transmission over untrusted networks like the internet.

## **5.6 Configure and verify access control lists**

- Access Control Lists (ACLs) are used to control and filter network traffic by permitting or denying packets based on defined rules. ACLs can be applied to router interfaces to restrict access to resources or manage traffic flow.

- Standard ACLs:
  - Filter traffic based only on the source IP address.
  - Applied closest to the destination to minimize the impact on legitimate traffic.
  - Number range: 1-99 (legacy) or 1300-1999 (expanded).
- Extended ACLs:
  - Filter traffic based on multiple criteria: source and destination IP addresses, protocols, and ports.
  - Applied closest to the source to minimize unnecessary traffic.
  - Number range: 100-199 (legacy) or 2000-2699 (expanded).

- Standard ACL Configuration
  - Access-list 10 permit 192.168.1.0 0.0.0.255
  - Interface GigabitEthernet 0/0
  - Ip access-group 10 in
- Explanation
  - access-list 10: Creates a standard ACL numbered 10.
  - permit 192.168.1.0 0.0.0.255: Permits traffic from the 192.168.1.0/24 network.
  - ip access-group 10 in: Applies the ACL to inbound traffic on the interface.

- Extended ACL Configuration
  - Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 80
  - Access-list 101 deny any any
  - Interface GigabitEthernet 0/0
  - Ip access-group 101 out
- Explanation:
  - access-list 101: Creates an extended ACL numbered 101.
  - permit tcp 192.168.1.0 0.0.0.255 any eq 80: Allows HTTP traffic from the 192.168.1.0/24 network to any destination.
  - deny ip any any: Denies all other traffic.
  - ip access-group 101 out: Applies the ACL to outbound traffic on the interface.

- Named ACL Configuration
  - Ip access-list BLOCK\_TELNET
  - Deny tcp any any eq 23
  - Permit ip any any
  - Exit
  - Interface GigabitEthernet 0/0
  - Ip access-group BLOCK\_TELNET in
  - Exit
- Explanation
  - ip access-list extended BLOCK\_TELNET: Creates a named extended ACL.
  - deny tcp any any eq 23: Blocks Telnet traffic.
  - permit ip any any: Allows all other traffic.
  - ip access-group BLOCK\_TELNET in: Applies the ACL to inbound traffic.

- Verifying ACLs
  - Show access-lists : shows ACL configuration
  - Show ip interface gigabitEthernet 0/0 : shows interface with ACLs applied
  - Debug ip packet : shows traffic matched by ACL in real time (use with caution in production environments)

- Best Practices for ACLs
  - Place Standard ACLs close to the destination.
  - Place Extended ACLs close to the source.
  - Use the deny ip any any rule at the end to explicitly log unmatched traffic.
  - Always test ACLs in a lab before applying to production environments.
  - Use descriptive comments for named ACLs to improve clarity.

## **5.7 Configure and verify Layer 2 security features (DHCP snooping, dynamic ARP inspection, and port security)**

- Layer 2 security features protect against attacks such as spoofing, MAC flooding, and ARP poisoning. Key features include DHCP Snooping, Dynamic ARP Inspection (DAI), and Port Security.

- DHCP Snooping : Prevents rogue DHCP servers from assigning IP addresses by filtering DHCP messages based on trusted or untrusted ports.
- Configuration Example:
  - Ip dhcp snooping
  - Ip dchp snooping vlan 10
  - Interface GigabitEthernet 0/1
  - Ip dhcp snooping trust
  - Exit
  - Interface GigabitEthernet 0/2
  - Ip dhcp limit rate 10
  - Exit
- Explanation:
  - ip dhcp snooping: Enables DHCP Snooping globally.
  - ip dhcp snooping vlan 10: Enables DHCP Snooping on VLAN 10.
  - ip dhcp snooping trust: Marks the interface as trusted (for DHCP servers).
  - ip dhcp snooping limit rate 10: Limits DHCP packets to 10 per second on untrusted interfaces.
- Verification
  - Show ip dhcp snooping
  - Show ip dhcp binding

- Dynamic ARP Inspection (DAI) : Prevents ARP spoofing attacks by verifying ARP packets against the DHCP snooping binding table.
- Configuration Example:
  - Ip arp inspection vlan 10
  - Interface GigabitEthernet 0/1
  - Ip arp inspection trust
  - Exit
  - Ip arp inspection validate src-mac dst-mac ip
- Explanation:
  - ip arp inspection vlan 10: Enables ARP inspection for VLAN 10.
  - ip arp inspection trust: Marks the interface as trusted (e.g., DHCP server ports).
  - ip arp inspection validate src-mac dst-mac ip: Validates ARP packets based on source MAC, destination MAC, and IP.
- Verification
  - Show ip arp inspection
  - Show ip arp inspection vlan 10

- Port Security : Prevents MAC address flooding attacks by limiting the number of MAC addresses on a port.
- Configuration Example:
  - Interface GigabitEthernet 0/1
  - Switchport mode access
  - Switchport port-security
  - Switchport port-security maximum 2
  - Switchport port-security violation restrict
  - Switchport port-security mac-address sticky
- Explanation
  - switchport port-security: Enables port security.
  - switchport port-security maximum 2: Allows only 2 MAC addresses on the port.
  - switchport port-security violation restrict: Configures the action for violations (restrict traffic, log the event).
  - switchport port-security mac-address sticky: Dynamically learns MAC addresses and saves them to the running configuration.
- Verification
  - Show port-security
  - Show port-security interface GigabitEthernet 0/1
  - Show mac address-table secure

- Best Practices:
  - Enable DHCP Snooping on VLANs with DHCP clients.
  - Configure Dynamic ARP Inspection only on VLANs where DHCP Snooping is enabled.
  - Use Port Security on access ports connected to end devices.
  - Set violation modes carefully:
    - Protect: Drops unauthorized frames silently.
    - Restrict: Logs and drops unauthorized frames.
    - Shutdown: Disables the port on a violation.
  - Test configurations in a lab before deployment in production environments.

## **5.8 Compare authentication, authorization, and accounting concepts**

- Authentication, Authorization, and Accounting (AAA) are three fundamental security concepts used to control access to network resources and monitor usage.

- Authentication : Verifies the identity of a user or device attempting to access a system or network.
- Purpose : Ensures only legitimate users or devices are granted access.
- Examples:
  - Username and password.
  - Certificates.
  - Multi-factor authentication (MFA).

- Authorization : Determines what resources or actions an authenticated user or device is allowed to access.
- Purpose : Enforces permissions and policies based on roles or privileges.
- Examples:
  - Granting access to specific network segments.
  - Restricting file or folder access.
  - Allowing specific commands on network devices (e.g., in Cisco IOS).

- Accounting : Tracks and records user or device activity within the network for auditing and reporting purposes.
- Purpose : Provides logs for troubleshooting, billing, or compliance.
- Examples:
  - Logging connection times and durations.
  - Recording commands executed by users.
  - Monitoring bandwidth usage.

- Example with AAA Implementation (Using TACACS+ or RADIUS)
  - Authentication: A user logs in with a username and password, which is validated by the AAA server.
  - Authorization: Once authenticated, the AAA server checks the user’s permissions (e.g., can only view configs but cannot make changes).
  - Accounting: The AAA server logs the user’s activities (e.g., commands entered or changes made).

## **5.9 Describe wireless security protocols (WPA, WPA2, and WPA3)**

- Wireless security protocols are designed to protect data transmitted over Wi-Fi networks by encrypting communication and authenticating devices.

- WPA (Wi-Fi Protected Access) : Introduced as an improvement over the insecure WEP protocol.
  - Key Features:
    - Uses TKIP (Temporal Key Integrity Protocol) for encryption, which dynamically changes encryption keys.
    - Offers better security than WEP but still has vulnerabilities to modern attacks.
  - Weakness : Relies on outdated encryption mechanisms (TKIP), making it susceptible to exploits

- WPA2 : Replaced WPA and became the standard for wireless security.
  - Key Features:
    - Uses AES (Advanced Encryption Standard) for stronger encryption.
    - Operates in two modes:
      - Personal (PSK): Uses a pre-shared key for home/small networks.
      - Enterprise: Uses 802.1X with RADIUS for user authentication in business networks.
    - Supports CCMP (Counter Mode with Cipher Block Chaining Message Authentication Code Protocol) for data integrity.
  - Weakness : Vulnerable to brute-force or dictionary attacks if weak passwords are used.



- WPA3 : The latest and most secure wireless security protocol.
  - Key Features
    - Uses Simultaneous Authentication of Equals (SAE) for secure key exchange, replacing the PSK mechanism in WPA2.
    - Provides forward secrecy, ensuring past data cannot be decrypted even if a key is compromised.
    - Introduces 192-bit encryption for enterprise networks.
    - Protects against offline password-guessing attacks.
  - Weakness : Requires compatible hardware and software, limiting adoption on older devices.

- When to use
  - WPA: Legacy systems only (not recommended for secure networks).
  - WPA2: Suitable for most networks today, especially when strong passwords are used.
  - WPA3: Ideal for modern networks requiring the highest security.

## **5.10 Configure and verify WLAN within the GUI using WPA2 PSK**

- Configuring WLAN with WPA2 PSK:
1. Access Router/Access Point GUI
   1. Open a browser and enter the default gateway IP (usually 192.168.1.1 or 192.168.0.1) to access the router’s web interface. Log in with admin credentials.
1. Navigate to Wireless Settings
   1. Once inside the router GUI, locate “Wireless” section. This might vary depending on the router model but usually labeled as ‘Wireless Settings’ or ‘Wi-Fi Configuration’.
1. Set the SSID
   1. Enter a unique SSID to identify your WLAN. This is the name that will appear on devices trying to connect to the network.
1. Choose WPA2 PSK
   1. Under the security settings, select WPA2 PSK (sometimes labeled as WPA2 Personal) as the security mode. Ensure that WPA2 is chosen over WPA for stronger encryption.
1. Enter Pre-Shared Key (PSK)
   1. Create a strong passphrase for the pre-shared key (at least 8 characters). The PSK will be required by devices attempting to connect to the WLAN.
1. Save Settings
   1. After setting the SSID and WPA2 PSK, save the changes. The router will apply the configurations, which may cause a brief disconnection of devices from the network.

- Verifying WLAN Configuration:
1. Connect a Device
   1. Using a wireless device (e.g., laptop, smartphone), locate the SSID you configured. Select it and enter the PSK to establish a connection
1. Verify Network Security
   1. After successfully connecting, verify the encryption by access the network settings on a connected device. Ensure the connection is using WPA2 encryption.
1. Verify Internet Access
   1. Once connected to the WLAN, test the internet connection by opening a website or running a speed test to ensure full functionality.

