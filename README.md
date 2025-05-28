# 🔐 Automatic TLS with cert-manager and Let's Encrypt in Kubernetes

This repository documents how HTTPS is automatically enabled in a self-hosted Kubernetes environment using **cert-manager**, **Let’s Encrypt**, and **Cloudflare (Full Strict Mode)**.

A `ClusterIssuer` is created using [cert-manager](https://cert-manager.io/) to define how the Kubernetes cluster interacts with Let’s Encrypt via the ACME protocol. When an `Ingress` resource is annotated to reference this issuer, cert-manager automatically initiates the certificate issuance process.

As part of domain ownership validation, cert-manager provisions a temporary HTTP route under the domain, typically at `/.well-known/acme-challenge/<token>`. This route is served through the ingress controller. Let’s Encrypt sends an HTTP request to this challenge URL—passing through **Cloudflare**, the **router**, and the **Kubernetes ingress controller**—to verify that the correct token is accessible at the expected path.

If the challenge is successfully validated, Let’s Encrypt issues a TLS certificate. cert-manager stores the certificate and its private key in a Kubernetes TLS `Secret` object. The ingress controller then uses this secret to terminate incoming HTTPS connections, enabling secure communication between **Cloudflare** and the **Kubernetes ingress**. Once decrypted, traffic is forwarded internally to the appropriate pods, typically over HTTP unless otherwise configured.

## 🚣 TLS Flow Diagram

<img width="845" alt="Screenshot 2025-05-28 at 12 36 47 AM" src="https://github.com/user-attachments/assets/757ce417-84dc-4d30-9030-20b6ba479fc5" />

## ✅ Technologies Used

- Kubernetes
- cert-manager
- Let’s Encrypt
- NGINX Ingress Controller
- Cloudflare (Full Strict SSL Mode)
