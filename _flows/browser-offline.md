---
metadata:
  id: "browser-offline"
  title: "Browser Showing Offline"
  description: "Diagnostic flow for when a Chromium-based browser shows offline while loading web pages."
  type: "Network Connectivity Issues"
steps:
  start:
    title: "Check Physical Connection"
    body: "Ask the user to verify their network cable is securely plugged in and the link lights are active. If using Wi-Fi, ensure they are connected to the correct corporate SSID."
    options:
      - label: "Connected properly"
        target: "check_ip_dhcp"
        type: "primary"
      - label: "Not connected"
        target: "fix_physical"
        type: "danger"

  fix_physical:
    title: "Restore Physical Connection"
    body: "Have the user plug in the cable or reconnect to the Wi-Fi. Ask them to reload the webpage."
    options:
      - label: "Issue Resolved"
        target: "resolved"
        type: "success"
      - label: "Still Offline"
        target: "check_ip_dhcp"
        type: "primary"

  check_ip_dhcp:
    title: "Verify IP Address (DHCP)"
    body: "Open Command Prompt and run `ipconfig`. Verify the machine has a valid corporate IP address and is not using a 169.254.x.x (APIPA) address."
    options:
      - label: "Valid IP Address"
        target: "check_dns"
        type: "primary"
      - label: "169.254.x.x / No IP"
        target: "troubleshoot_dhcp"
        type: "danger"

  troubleshoot_dhcp:
    title: "Troubleshoot DHCP / Network Port"
    body: "Run `ipconfig /release` then `ipconfig /renew`. If it still fails, check the switch port configuration or escalate to the Network team."
    options:
      - label: "IP Obtained, Resolved"
        target: "resolved"
        type: "success"
      - label: "Escalate to Network"
        target: "escalate_network"
        type: "danger"

  check_dns:
    title: "Check DNS Resolution"
    body: "Run `nslookup google.com` or `ping google.com` to confirm DNS is successfully resolving hostnames to IP addresses."
    options:
      - label: "DNS Resolves"
        target: "check_proxy"
        type: "primary"
      - label: "DNS Resolution Fails"
        target: "flush_dns"
        type: "danger"

  flush_dns:
    title: "Flush DNS Cache"
    body: "Run `ipconfig /flushdns`. Verify the primary and secondary DNS server IPs are correct for your site."
    options:
      - label: "DNS Fixed, Resolved"
        target: "resolved"
        type: "success"
      - label: "Still Failing"
        target: "escalate_network"
        type: "danger"

  check_proxy:
    title: "Verify Proxy Settings"
    body: "Open Windows Proxy Settings or the Chromium browser settings. Check if an incorrect manual proxy or PAC script is enforced."
    options:
      - label: "No Proxy Issues"
        target: "check_edr"
        type: "primary"
      - label: "Proxy Misconfigured"
        target: "fix_proxy"
        type: "danger"

  fix_proxy:
    title: "Correct Proxy Configuration"
    body: "Disable the manual proxy or correct the PAC URL according to standard corporate policy, then restart the browser."
    options:
      - label: "Issue Resolved"
        target: "resolved"
        type: "success"
      - label: "Still Offline"
        target: "check_edr"
        type: "primary"

  check_edr:
    title: "Review EDR / Security Software"
    body: "Check the local security agent (e.g., CrowdStrike, SentinelOne, or local firewall). Verify if the web browser's traffic is being blocked by a security policy."
    options:
      - label: "No Blocks Found"
        target: "escalate_tier2"
        type: "danger"
      - label: "Blocks Found"
        target: "escalate_security"
        type: "help"

  resolved:
    title: "Resolved"
    body: "The issue has been resolved. Document the steps taken and close the ticket."
    options: []

  escalate_network:
    title: "Escalate to Network Team"
    body: "Document the troubleshooting steps, attach `ipconfig /all` output, and assign the ticket to the Network Engineering queue."
    options: []

  escalate_security:
    title: "Engage Security Team"
    body: "Gather EDR logs and exact timestamps. Escalate the ticket to the Security Operations Center (SOC) to request a policy review or exclusion."
    options: []

  escalate_tier2:
    title: "Escalate to Tier 2 / Desktop Support"
    body: "The standard Tier 1.5 checks are complete. Reassign the ticket to Tier 2 Desktop Support for advanced OS-level troubleshooting."
    options: []
---
