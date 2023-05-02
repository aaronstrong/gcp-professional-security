# Data Protection

**Table of contents:**
    - [Distributed Denial of Service Attacks (DDoS) Mitigation](#ddos-mitigation)
        - [Cloud Armor](#cloud-armor)
        - [Cloud Security Scanner](#cloud-security-scanner)


<a id="ddos-mitigation"></a>
# [Distributed Denial of Service Attacks (DDoS) Mitigation](https://cloud.google.com/files/GCPDDoSprotection-04122016.pdf)

A DDoS is an an attempt to render a service or application unavailable to its end users. Attackers use multiple resources (often a large number of comprised hosts/instances) to orchestrate large-scale attacks against targets.

- Reduce the attack surface for you GCE deployment
    - Isolate infra with VPCs
    - Use subnets, tagging, firewall rules, and IAM
    - GCP provides anti-spoofing fo the private network by default
    - GCP automatically provides isolation between VPCs
- Isolate your internal traffic from the external world
    - Deploy instances without Public IP
    - Use Cloud NAT and SSH bastions to limit number of instances exposed to the internet
    - Use internal load balancers
- Enable Proxy-based load balacing
    - When you enable HTTP(S) LBs or SSL Proxy LBs, Google infrastructure mitigates and absorbs many Layer 4 and below attacks like SYN floods, IP fragment floods, port exhaustion, etc.
    - If you have HTTP(S) LB with instances in mutliple regions, you are able to disperse your attack across instances around the globe.
- Scale to absorb the attack
- Protection with CDN offloading
    - CNS acts as a proxy between your clients and your origin servers.
    - If you use CDN interconnect, you can leverage the additional DDoS protection provided by your CDN interconnect partners
- Deploy third-party DDoS protection solutions
- App Engine Deployment
    - App Engine sits behind the GFE which mitigates and absorbs many Layer 4 and below attacks.
    - Specifiy a set of IPs/IP networks via a `dos.yaml` file to block them from accessing your apps.
- Google Cloud Storage
    - Signed URLs
- API rate limiting
    - Define the number of requests that can be made to the Compute Engine API


<a id="cloud-armor"></a>
## Cloud Armor

Works with Global HTTP(S) Load Balancer to provide built-in defenses against infrastructure DDoS attacks. The service uses security policies that are made up of rules that allow or prohibit traffic from IP addresses or ranges defined in the rule.
- Security policies
    - Deny list and allow list rules
    - Can be specified for backend services
- IP address allow list and deny list rules
    - Deny or allow list for IP address / CIDR range
    - IPv4/IPv6
    - Designate the order of the rules
- Preview mode
- Logging
    - Policy name, matched rule priority, associated action, and related info

<a id="cloud-security-scanner"></a>
## Cloud Security Scanner

Cloud Security Scanner is a web security scanner for common vulnerabilities in App Engine, Compute Engine, and GKE

- Automated vulnerabililty scanning
- Scan and detect common vulnerabilities
- Crawls your application
    - Follows all links from starting URL
    - Attempts to exercise as many user inputs and event handlers as possible
- Use with cuation in Live Environment
    - Pushes buttons
    - Clicks llinks
    - Posts test strings as comments
- Block individual UI elements
- Exclude URLs
- Very low false positive rates
- Immediate or scheduled scans
- Use by Google or non-Google accounts