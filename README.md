# Shadow-Path: The Censorship-Resistant Distributed Network

![License: AGPLv3](https://img.shields.io/badge/License-AGPLv3-blue.svg)
![Language: D](https://img.shields.io/badge/Language-D-red.svg)
![Status: Planning](https://img.shields.io/badge/Status-Planning-orange.svg)

## Vision

Shadow-Path is an ambitious open-source project to create a decentralized, volunteer-operated network that provides anonymous, censorship-resistant communication and file storage. It allows users to securely share information by knowing only a secret link—a `.path` URL. The network is built by its users, who donate bandwidth and disk space to form a resilient "shadow" internet, resistant to surveillance and control.

## Core Principles

*   **Absolute Privacy:** No metadata leakage. No IP logging. No knowledge of content stored or accessed.
*   **Plausible Deniability:** Node operators cannot be held responsible for the encrypted data chunks they store. The stored data is indistinguishable from random noise.
*   **Radical Decentralization:** No central servers or authorities. The network is built and owned by its users.
*   **User Empowerment:** Users have transparent, fine-grained control over their donated resources (disk, bandwidth, CPU).
*   **Security First:** The application is rigorously isolated from the host system using modern Linux security primitives.

## How It Works (Conceptual Overview)

1.  **A User Publishes:**
    *   Drops a file into the Shadow-Path application.
    *   The client encrypts the file, breaks it into redundant chunks, and generates a unique `.path` URL (a cryptographic hash).
    *   These encrypted chunks are anonymously distributed across the volunteered storage ("Black Boxes") of other nodes on the network.

2.  **A User Retrieves:**
    *   Pastes a `.path` URL into their Shadow-Path application.
    *   The client anonymously queries the network to find the locations of the required chunks.
    *   Using a multi-layered routing protocol ("Lost Path"), it retrieves the chunks, verifies them, decrypts them, and reassembles the original file.
    *   The publisher and retriever remain anonymous to each other and to the nodes storing the data.

3.  **A User Volunteers:**
    *   Installs the `shadowd` daemon.
    *   Configures how much disk space to donate (their "Black Box") and bandwidth to contribute.
    *   The daemon runs securely in the background, helping to store data and route traffic for others, strengthening the entire network.

## Technology Stack & Why D?

We are building the core daemon (`shadowd`) and protocols in the **D programming language**.

*   **Performance:** Compiles to native code, offering bare-metal speed crucial for cryptographic operations and high-throughput networking.
*   **Safety:** Features like `@safe` memory-safe subsets eliminate whole classes of bugs common in C/C++, without a garbage collector penalty in critical paths.
*   ** productivity:** Modern syntax, powerful generics, built-in unit testing, and the `dub` build tool enable rapid and reliable development.
*   **Ecosystem:** Excellent C interoperability allows us to use battle-tested libraries like Libsodium via Deimos bindings.

## System Architecture

The system is composed of several integrated components:

| Component | Purpose |
| :--- | :--- |
| **`shadowd` Daemon** | The core Linux daemon that implements the protocol and manages the Black Box. Runs isolated under a dedicated user. |
| **Lost Path Protocol (LPP)** | A suite of protocols for routing, storage, and retrieval. |
| **LPP-DHT** | A Kademlia-based Distributed Hash Table for peer and chunk discovery. |
| **LPP-MIX** | A low-volume, high-latency mixnet for anonymous control messages and connection setup. |
| **LPP-TUN** | A fast, onion-routed tunnel protocol for high-speed data transfer. |
| **Black Box** | An isolated directory on a volunteer's disk storing encrypted data chunks. |
| **CLI/GUI Client** | The user interface for interacting with the network (publish/retrieve). |

## Project Roadmap

### Phase 0: Research & Specification (Current Phase)
-   [ ] Finalize the Lost Path Protocol (LPP) specification.
-   [ ] Define the `.path` URL standard and cryptographic formats.
-   [ ] Create detailed threat model and architecture diagrams.
-   [ ] **Deliverable:** Complete `docs/05-PROTOCOL.md` spec.

### Phase 1: Core Library & CLI (Months 1-4)
-   [ ] Develop `libshadow` core library in D (crypto, chunking, storage).
-   [ ] Build a simple CLI tool for local file encryption/decryption using the `.path` standard.
-   [ ] **Deliverable:** A functional CLI that can `add` and `get` files locally.

### Phase 2: The `shadowd` Daemon (Months 5-8)
-   [ ] Implement the LPP-DHT for peer discovery.
-   [ ] Build the `shadowd` daemon with systemd integration and sandboxing.
-   [ ] Develop initial AppArmor/SELinux profiles.
-   [ ] **Deliverable:** A secure daemon that can join a test network and respond to DHT queries.

### Phase 3: Networking & Anonymity (Months 9-12)
-   [ ] Implement LPP-MIX and LPP-TUN protocols for anonymous routing.
-   [ ] Integrate the full stack: CLI can publish/retrieve over the network.
-   [ ] Onboard initial testers to form "Shadow Net One."
-   [ ] **Deliverable:** A usable alpha for technical users.

### Phase 4: GUI & Ecosystem (Future)
-   [ ] Develop a cross-platform GUI application.
-   [ ] Enhance protocol performance and security.
-   [ ] Explore mobile clients and wider adoption.

## Getting Started for Developers

We welcome contributors! Here's how to dive in:

1.  **Get Familiar:** Read the `/docs` directory, starting with `01-VISION.md` and `05-PROTOCOL.md`.
2.  **Set Up Your Environment:**
    ```bash
    # Install the D compiler (ldc2 recommended) and dub
    git clone https://github.com/shadow-path/shadow.git
    cd shadow
    # Install dependencies (e.g., libsodium)
    dub build
    ```
3.  **Find an Issue:** Look for issues tagged `good first issue` or `help wanted` in the issue tracker.
4.  **Join the Conversation:** Discussion happens on our [Matrix Channel](#) (to be created) and in GitHub issues.

## Documentation

All documentation is in the `/docs` folder:

*   **[`ARCHITECTURE.md`](docs/02-ARCHITECTURE.md)** - System overview and component interaction.
*   **[`PROTOCOL.md`](docs/05-PROTOCOL.md)** - **The core spec.** Packet formats, handshakes, and routing logic.
*   **[`BUILD.md`](docs/07-BUILD.md)** - Guide for building the project from source.
*   **[`CONTRIBUTING.md`](docs/08-CONTRIBUTING.md)** - Guidelines for contributors.

## License

This project is licensed under the **GNU Affero General Public License v3.0 (AGPLv3)**. See the `LICENSE` file for details. This license ensures that all modifications and services based on Shadow-Path must also be open source.

## Disclaimer

This project is in the early planning and development stages. The architecture and specifications are subject to change. It is not yet suitable for real-world privacy or security use cases.

---

**The internet should belong to its users. Help us build it.**
