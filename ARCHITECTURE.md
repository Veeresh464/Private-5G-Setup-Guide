# Private 5G Network Architecture

This repository contains a step-by-step guide to setting up a Private 5G/LTE Network using Open Source software and generic hardware.

## System Overview

The system implements a full 5G network stack, consisting of the following key components:

1.  **User Equipment (UE)**: A Commercial Off-The-Shelf (COTS) smartphone or modem with a programmable SIM card.
2.  **Radio Hardware**: USRP B210 (Software Defined Radio) acting as the RF frontend.
3.  **Radio Access Network (RAN)**: `srsRAN` (specifically `srsenb` or `srsgnb`) handling the base station logic.
4.  **Core Network (CN)**: `Open5GS` providing 5G Core functions (AMF, SMF, UPF, AUSF, UDM, etc.).
5.  **External Network**: The host machine's internet connection, accessed via NAT/IP Forwarding through the `ogstun` interface.

## Architecture Diagram

The following diagram illustrates the data flow and logical connections between components.

```mermaid
graph TD
    subgraph "User Domain"
        UE["COTS UE<br/>(Smartphone)"]
        SIM["Programmable SIM<br/>(sysmoISIM)"]
        UE --- SIM
    end

    subgraph "Radio Access Network (RAN)"
        USRP["USRP B210<br/>(SDR Hardware)"]
        srsRAN["srsRAN gNodeB<br/>(Software)"]

        UE -.->|"Radio Interface<br/>(Air)"| USRP
        USRP ---|"USB 3.0"| srsRAN
    end

    subgraph "Core Network (Open5GS)"
        AMF["AMF<br/>(Access & Mobility)"]
        UPF["UPF<br/>(User Plane Function)"]
        SMF["SMF<br/>(Session Management)"]
        MongoDB[("MongoDB")]

        srsRAN --"NGAP/SCTP"--> AMF
        srsRAN --"GTP-U/UDP"--> UPF
        AMF <--> SMF
        SMF <--> UPF
        AMF <--> MongoDB
    end

    subgraph "Data Network"
        TUN["ogstun<br/>(Tun Interface)"]
        NAT["NAT / IP Forwarding"]
        Internet["Internet<br/>(External Network)"]

        UPF <--> TUN
        TUN <--> NAT
        NAT <--> Internet
    end

    %% Styles
    classDef hardware fill:#f9f,stroke:#333,stroke-width:2px;
    classDef software fill:#bbf,stroke:#333,stroke-width:2px;
    classDef network fill:#dfd,stroke:#333,stroke-width:2px;

    class UE,USRP,SIM hardware;
    class srsRAN,AMF,UPF,SMF,MongoDB software;
    class TUN,NAT,Internet network;
```

## Key Configuration Parameters

Based on the documentation steps:

*   **MCC (Mobile Country Code)**: `001` (Test Network)
*   **MNC (Mobile Network Code)**: `01` (Test Network)
*   **TAC (Tracking Area Code)**: `7`
*   **Network Interface**: `ogstun` (Created by Open5GS for handling user traffic)
*   **Drivers**: UHD (Universal Hardware Driver) for USRP communication.

## Data Flow

1.  **Registration**: The UE communicates via the USRP to srsRAN, which forwards the request to the AMF in Open5GS for authentication (using SIM credentials stored in MongoDB).
2.  **Session Establishment**: Once authenticated, the SMF establishes a PDU session and configures the UPF.
3.  **Traffic Flow**: User data sent from the UE goes through the USRP -> srsRAN -> UPF -> `ogstun` -> NAT -> Internet.
