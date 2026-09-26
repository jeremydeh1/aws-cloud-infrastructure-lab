# AWS Cloud Infrastructure Architecture

This diagram represents the current architecture of the AWS Cloud Infrastructure Lab.

```mermaid
flowchart TB

    Internet((Internet))
    IGW[Internet Gateway]

    Internet --> IGW

    subgraph VPC["VPC — 10.0.0.0/16"]

        subgraph AZA["Availability Zone — us-east-1a"]

            PublicA["Public Subnet A<br/>10.0.1.0/24"]
            WebEC2["Public EC2<br/>Amazon Linux 2023<br/>Nginx Reverse Proxy"]

            PrivateA["Private Subnet A<br/>10.0.11.0/24"]
            PrivateApp["Private EC2<br/>Application Server<br/>Python :8080 + systemd"]

            PublicA --> WebEC2
            WebEC2 -->|HTTP :8080| PrivateApp
            PrivateA --> PrivateApp

        end

        subgraph AZB["Availability Zone — us-east-1b"]

            PublicB["Public Subnet B<br/>10.0.2.0/24"]
            PrivateB["Private Subnet B<br/>10.0.12.0/24"]

        end

    end

    IGW --> PublicA
    IGW --> PublicB
```

## Current Design

- The VPC uses the `10.0.0.0/16` CIDR range.
- Public and private subnets span two Availability Zones.
- Public subnets route internet traffic through an Internet Gateway.
- A public Amazon Linux EC2 instance runs Nginx as a reverse proxy.
- Nginx receives HTTP requests on port 80 and forwards `/app/` traffic to the private application server.
- The private Amazon Linux EC2 instance runs inside `private-subnet-a` with no public IPv4 address.
- The private application runs on TCP port `8080`.
- The private application's security group only allows port `8080` traffic from the public web server's security group.
- The application is managed by `systemd` so it starts automatically at boot and restarts if the process fails.
- The private subnet does not have direct internet access.