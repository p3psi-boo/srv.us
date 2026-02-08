# Backend Development

This guide explains how to run the `srv.us` backend locally.

## Prerequisites

- Go (1.20+ recommended)
- OpenSSL (for generating certificates)
- Make (optional, but convenient)

## Getting Started

1.  Navigate to the `backend` directory:
    ```bash
    cd backend
    ```

2.  Generate necessary keys and certificates:
    ```bash
    make fullchain.pem
    make ssh_host_rsa_key
    make ssh_host_ecdsa_key
    make ssh_host_ed25519_key
    ```
    This will generate self-signed certificates for HTTPS and host keys for SSH.

3.  Build the binary:
    ```bash
    make srvus
    ```

4.  Run the server:
    ```bash
    make run
    ```
    This command runs the server with the following default flags (defined in `Makefile`):
    - `-domain srvtest`: The domain to serve (default is `srv.us` in production, `srvtest` for local dev).
    - `-https-port 4443`: The HTTPS port.
    - `-ssh-port 2222`: The SSH port.
    - `-https-chain-path fullchain.pem`: Path to the certificate chain.
    - `-https-key-path privkey.pem`: Path to the private key.
    - `-ssh-host-keys-path .`: Path where SSH host keys are located.

## Testing

To test the tunnel, you can run:

```bash
ssh -p 2222 localhost -R 1:localhost:3000
```
(Make sure you have something running on port 3000 locally, or change the port).

Note: You might need to add `srvtest` to your `/etc/hosts` pointing to `127.0.0.1` to access the HTTPS endpoints properly if you want to test subdomain routing.
