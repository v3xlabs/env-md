# Virtual Private Networks

## What's a VPN?

A VPN is simply a network that is created over the internet. It is useful for a number of purposes, such as:

- Connecting to a remote network securely
- Bridging two networks together

> Some companies like to run VPN servers, which allow users to connect to and use their IP addresses as their default route to the Internet. These may be useful for changing your IP address to a different country or moving trust of unencrypted connections away from your ISP, but are often over-marketed. This is not the focus of this document.

## Plain ol' WireGuard

It is a simple, modern, and secure VPN that is easy to set up and use. WireGuard does not have a "client-server" model like OpenVPN, but rather a "peer-to-peer" model. This means that each device that you want to connect to the VPN must have a configuration file that tells it how to connect to the other devices. This is very flexible, for example when you want to connect two networks (with different IP ranges) together over the internet.

While it is beyond the scope of this document to explain how to set up WireGuard, below is a simple example of a configuration file that you might use to connect to a remote network. This file should be placed in `/etc/wireguard/wg0.conf`:

```ini
# The [Interface] section defines the local configuration
[Interface]
PrivateKey=YOUR_PRIVATE_KEY
Address=10.10.0.2/32
DNS=1.1.1.1

# You can add more [Peer] sections to connect to more devices, creating a mesh network
[Peer]
PublicKey=PEER_PUBLIC_KEY
PreSharedKey=OPTIONAL_SHARED_KEY
# 0.0.0.0/0 is a wildcard that tells the peer to route all traffic through this connection, see "Road Warrior VPN".
# You can also specify specific IP ranges to route through this connection, for example 192.168.1.0/24. This is useful for connecting two networks together.
# You can also specify single addresses, this is useful for connecting to a single device, like how Tailscale works.
AllowedIPs=10.10.0.1/32
Endpoint=REMOTE_IP:51820
```

WireGuard is very flexible and the possibilities are endless, however it may be finnicky to set up, especially if you are not familiar with networking, as configuration scales exponentially with the number of devices you want to connect. If you are looking for a simpler solution, you may want to consider Tailscale or ZeroTier.

## Tailscale

Tailscale runs on top of WireGuard, and its core functionality can easily be replicated with vanilla WireGuard. However, it is much easier to set up and use, especially for connecting multiple devices together. Tailscale is a commercial product, but it is free for personal use. It is also open source, so you can self-host it if you want {{footnote: See [Headscale](https://headscale.net/stable/about/faq/)}}

To set up Tailscale, you will need to create an account on their website, and then install the Tailscale client on your devices. You can then connect your devices together using the Tailscale web interface. Tailscale's client is available in your package manager, but can also be downloaded from their website.

Additional features:

- NAT traversal
- "Magic DNS" maps MACHINENAME.TAILNETNAME.ts.net to the VPN IP address of the machine. Tailscale can also provision trusted TLS certificates for these DNS names.
- "Funnels" provide ngrok-like functionality, allowing you to expose services on your machine to the internet without port forwarding. {{footnote: Be aware that all traffic is routed through Tailscale's servers when using this feature. Don't abuse it, or we can't have nice things}}

## ZeroTier

ZeroTier is also an "overlay" network, but instead of building on top of WireGuard it works like a virtual layer 2 ethernet switch. This is unlike Tailscale, which runs on the third layer of the OSI model.

**Advantages of ZeroTier's Layer 2:**

- Can bridge entire networks together, including non-IP protocols
- May work a bit better with legacy applications that expect devices to be on the same physical network

**Disadvantages compared to Tailscale:**

- Generally higher overhead due to additional encapsulation
- Might create larger attack surfaces by exposing more of the network stack

ZeroTier is more technically challenging to set up than Tailscale, but is also more flexible. ZeroTier is free for personal use. It is however not licensed under an open source license, which may be a dealbreaker for some, especially those having a commercial use case.
