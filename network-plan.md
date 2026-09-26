# Network Plan

## VPC

VPC CIDR: 10.0.0.0/16

## Availability Zones

- Availability Zone A
- Availability Zone B

## Subnets

| Name | CIDR | Type | Availability Zone |
|---|---|---|---|
| Public Subnet A | 10.0.1.0/24 | Public | AZ-A |
| Public Subnet B | 10.0.2.0/24 | Public | AZ-B |
| Private Subnet A | 10.0.11.0/24 | Private | AZ-A |
| Private Subnet B | 10.0.12.0/24 | Private | AZ-B |

To be designed.

## Internet Connectivity

### Internet Gateway

An Internet Gateway will be attached to the VPC to provide internet connectivity.

### Public Route Table

Public subnets will use:

0.0.0.0/0 → Internet Gateway

### Private Route Table

Private subnets will NOT have a direct route to the Internet Gateway.
## Routing

### Public Route Table

Name: public-route-table

Routes:

| Destination | Target | Purpose |
|---|---|---|
| 10.0.0.0/16 | Local | Communication within the VPC |
| 0.0.0.0/0 | Internet Gateway | Internet-bound IPv4 traffic |

Associated subnets:

- public-subnet-a
- public-subnet-b

### Private Subnets

The private subnets do not have a direct route to the Internet Gateway.

## Current Architecture

Internet
    |
Internet Gateway
    |
VPC - 10.0.0.0/16
    |
    +-- us-east-1a
    |   +-- public-subnet-a  - 10.0.1.0/24
    |   +-- private-subnet-a - 10.0.11.0/24
    |
    +-- us-east-1b
        +-- public-subnet-b  - 10.0.2.0/24
        +-- private-subnet-b - 10.0.12.0/24