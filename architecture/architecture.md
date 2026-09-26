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
            PrivateA["Private Subnet A<br/>10.0.11.0/24"]

            EC2["EC2 Web Server<br/>Amazon Linux 2023<br/>Nginx"]

            PublicA --> EC2

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
- Public subnets use a route to the Internet Gateway.
- The EC2 web server currently runs in Public Subnet A.
- Nginx serves the Cloud Infrastructure Lab website.
- Private subnets do not currently have direct internet access.