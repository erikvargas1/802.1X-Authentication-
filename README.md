# 802.1X-Authentication


**What 802.1X Is?**

802.1X is an IEEE standard that defines port-based Network Access Control (PNAC).

• It provides a framework for authenticating devices (users or machines) before they are granted access to a wired or wireless LAN.

• Think of it as a "gatekeeper" that ensures only authenticated devices can communicate through a switch port (wired) or access point (wireless).
How It Works

-----

**🌟 It involves three main components:**

**Supplicant** – the client device (like a laptop or phone) requesting access.

**Authenticator** – the network device (like a switch or access point) that controls the physical access. 

<details>

By Physical access, we mean it controls whether the network port or radio channel is allowed to carry traffic. Until authentication succeeds, the authenticator keeps that port or channel in a blocked/unauthorized state—no normal data packets can pass through. Only authentication-related traffic (EAP over LAN or EAPOL) is allowed.

**Example Scenarios**

**Wired LAN: **Imagine plugging your laptop into a company’s network port. The switch port you connect to will not let you reach the LAN or Internet until your device successfully authenticates (e.g., with a username/password or certificate).

**Wi-Fi:** When you try to join a secure corporate Wi-Fi (WPA2-Enterprise), your access point acts as the gatekeeper. It won’t forward your traffic until your credentials are validated by the authentication server.

</details>

**Authentication Server** – usually a RADIUS server, which verifies credentials/certficate (Identity) and grants or denies access.

The authenticator blocks all non-802.1X traffic until authentication succeeds. Once authenticated, the port is opened for normal traffic.

------

**Where It Applies?**

Wired networks → securing switch ports so rogue devices can’t just plug in.

Wireless networks → often used with WPA2-Enterprise or WPA3-Enterprise, where users authenticate with usernames/passwords or certificates rather than a shared Wi-Fi password.

------

How EAP-TLS Works (802.1X with Certificates)

<details>

802.1X itself is just a framework — it doesn’t define how authentication happens, only when and where (i.e., at the network port). 802.1X enforces that authentication must happen at the network entry point (before you send traffic), but it doesn’t care whether that authentication is via password, certificate, smart card, or something else — that’s EAP’s job

**1. Device Starts Connection**

• A client device (supplicant) tries to join the secure Wi-Fi (e.g., WPA2-Enterprise or WPA3-Enterprise).

• The access point (authenticator) says: “Hold on, prove who you are first.”

**2. EAP Negotiation via 802.1X**

• The authenticator only allows EAPOL traffic (EAP over LAN).

• The supplicant and the authentication server (RADIUS) begin an EAP session.

• The authenticator just passes messages back and forth.

**3. Certificate-Based Authentication**

• The client presents its digital certificate (issued by a trusted Certificate Authority that the company manages).

• The RADIUS server validates:

  1) The certificate is signed by the trusted CA.
  2) The certificate is still valid (not expired or revoked).
  3) It matches an identity (user or machine) in the directory (like Active Directory).

• The RADIUS server also presents its own certificate (so the client knows it’s talking to the real server, not an imposter).

• This is called mutual authentication.

**4. Secure Session Established**

• Once both sides trust each other, they establish a secure TLS tunnel (like HTTPS).

• Keys are derived to encrypt the wireless session.

• The authenticator opens the port, and the client gets access to the network.


**🌟 Why EAP-TLS with Certificates is So Good**

1) Strong Security
• Certificates are very hard to steal compared to passwords.
• No shared Wi-Fi password floating around.
• Resistant to dictionary/brute-force attacks (unlike some EAP methods).

2) Mutual Authentication
• Both the client and server prove their identity.
• Prevents rogue access points or fake RADIUS servers.

3) Scalability and Manageability
• Each user/device gets its own certificate.If someone leaves the company, just revoke their certificate — no impact on others.
• Works seamlessly with enterprise PKI (Public Key Infrastructure).

4) Integration with Enterprise Infrastructure
• Works with Active Directory, LDAP, or any identity system tied to the CA.
• Supports policies (e.g., different VLANs depending on the device type or group).

</details>

--------








