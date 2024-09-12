# Plakar: The Next-Generation Backup Solution

## Introduction

In the rapidly evolving digital landscape, data integrity, security, and efficient storage have become critical concerns for both enterprises and individuals. Current backup solutions often fail to meet the increasing demands for scalability, security, and flexibility. Plakar offers a robust, next-generation alternative, designed from the ground up to tackle modern backup challenges with a focus on reliability, efficiency, and control.

## The Problem

Traditional backup solutions typically suffer from one or more of the following issues:

- **Inefficiency**: Many backup systems create redundant data copies, consuming excessive storage space and resources.
- **Scalability**: Handling large amounts of data, particularly in distributed or enterprise environments, often leads to slow performance and poor scalability.
- **Security**: Weak security measures leave backup data vulnerable to unauthorized access, ransomware, or other malicious attacks.
- **Limited Flexibility**: Most backup solutions offer limited deployment options, either favoring SaaS or on-premise without catering to diverse infrastructure needs.

These issues point to a clear gap in the market for a backup system that addresses these pain points while providing a scalable, secure, and cost-effective solution.

## What is Plakar?

Plakar is an advanced backup solution engineered to overcome the limitations of existing tools. Leveraging state-of-the-art **content-defined deduplication** algorithms, Plakar ensures maximum storage efficiency by eliminating redundant data at a granular level. Whether deployed on-premise or in the cloud, Plakar offers complete flexibility to fit any infrastructure.

### Key Features

- **Content-Defined Deduplication**: Plakar employs cutting-edge algorithms to detect and remove redundant data chunks, reducing storage consumption without sacrificing performance.
  
- **Security by Design**: With 20+ years of expertise in security and open-source software, Plakar integrates secure encryption standards, ensuring that data remains protected in both storage and transit.

- **Scalability**: From small personal setups to large enterprise environments with thousands of virtual machines, Plakar is built to scale, providing optimal performance regardless of data volume.

- **Open-Source and Flexible**: Unlike proprietary solutions, Plakar gives users full control over their backup infrastructure. The open-source nature of the project ensures transparency, extensibility, and adaptability to specific needs.

- **SaaS and On-Premise Compatibility**: Whether you operate in a cloud-native environment or require a fully on-premise solution, Plakar adapts seamlessly to your infrastructure, giving you full control over your data.

## Technology Behind Plakar

Plakar's architecture is designed for both speed and reliability. At its core, it utilizes **deduplication** techniques inspired by **content-defined chunking**, significantly reducing storage requirements. The system breaks down files into variable-sized chunks, comparing them against previously stored data to avoid redundancy.

Plakar is implemented in **Go**, a language known for its simplicity and performance, making it robust and scalable for handling large datasets.

### Security Features

- **End-to-End Encryption**: All data is encrypted at the source, ensuring that it remains secure throughout its lifecycle.
- **Zero Trust Model**: Plakar operates on the principle that no system, even within a trusted network, should be implicitly trusted.

### Backup and Restore Performance

Plakar ensures **fast, incremental backups**, with only changed data being saved after the initial backup. The restore process is equally optimized to quickly reconstruct files from deduplicated chunks, minimizing downtime.


## Technical Details

### Distributed Repository Model
Plakar leverages a distributed repository model similar to Git, providing a highly flexible and decentralized approach to managing backups. Each repository in Plakar maintains its own copy of snapshots, giving users extensive control over how their data is stored and synchronized. This distributed design offers several key features:

- Flexible Backup Policies: Repositories can share only a subset of backups or be complete clones of each other, depending on the user’s needs. This allows for tailored multi-site backup strategies, ensuring critical data is backed up at various locations.
- Custom Configurations: Each repository can have its own configuration, enabling different setups such as encrypted versus cleartext backups. This flexibility allows for different security policies based on the sensitivity of the data or the security of the storage location.
- Multiple Locations: Repositories can be distributed across various physical or cloud locations, enhancing the redundancy and resilience of the backup infrastructure. This is particularly useful for ensuring off-site backups and improving disaster recovery capabilities.
-  Push and Pull Synchronization: Plakar supports both push and pull synchronization, making it easy to replicate data between repositories. This allows users to back up to multiple locations from a central source (push) or to synchronize repositories by pulling backups from remote sites.

This distributed model ensures that users can define multi-site, resilient backup policies tailored to their specific infrastructure and security requirements, all while maintaining maximum flexibility in how data is replicated and protected.

### Repository Abstraction and Extensibility
Plakar repositories are designed to be abstracted, allowing for the implementation of various backends to store backups. While the default backend is the filesystem, functional proof-of-concepts (PoCs) have been implemented for S3, SQLite, and other storage backends. This abstraction makes it easy to extend Plakar to support additional storage targets as needed.

By default, Plakar backs up data from the filesystem and restores it to the filesystem. However, the system is highly extensible, allowing importers and exporters to be written to enable backups from different sources and restoration to different targets. For instance, functional PoCs have been implemented for:

- Importers: PoCs for importing data from S3, SQLite, and IMAP.
- Exporters: PoCs for exporting data to S3 and SQLite.

This flexibility allows Plakar to adapt to various environments, ensuring that it can back up and restore data across a wide range of storage systems and protocols.

#### Local and Remote Repository Support
Plakar supports both local repositories and remote repositories, giving users flexibility in where their data is stored. Remote repositories can be accessed over various transport protocols such as TCP, SSH, or HTTP. Additionally, Plakar is designed to allow the implementation of custom transport protocols, making it easy to extend the system to suit different infrastructure needs.

This abstraction of repositories and transport protocols ensures that Plakar can integrate seamlessly into diverse environments, whether users are working with on-premise storage, cloud services, or hybrid solutions.


### Standalone or Agent-Based Operation
Plakar is designed to operate in two modes: as a standalone tool or as an agent controlled by a central control center. This flexibility allows it to adapt to a wide range of environments, from individual use cases to large-scale distributed systems.

- Standalone Mode: In this mode, Plakar functions as an independent backup tool, providing users with full control over backup and restore operations directly on the client.
- Agent-Based Mode: Plakar can also be deployed as an agent, managed by a central control center. In this configuration, administrators can oversee and control backups across a large number of agents from a single point of control. Each agent uses challenge-based authentication and a secure protocol to:

Retrieve a schedule of required backup tasks from the control center.
Report events and statuses, such as completed backups or errors, back to the control center.
This dual-mode capability makes Plakar a versatile solution that can scale from simple, single-instance backups to complex, distributed environments where centralized management is required.

### Content-Defined Deduplication (CDC)

Plakar uses Content-Defined Deduplication (CDC) to significantly improve data storage efficiency by breaking files into variable-sized chunks based on their content. Unlike traditional deduplication methods that rely on fixed-size chunks, CDC dynamically adjusts chunk sizes depending on the content within the file. This allows the system to identify and deduplicate similar portions of data even when the file has changed or when there are content shifts.

One of the key benefits of CDC is that it enables deduplication to occur even if a file has been modified. Since the chunking process is content-based rather than relying on exact block sizes or positions, Plakar can still detect and deduplicate unchanged parts of a file. This is particularly useful in scenarios where files evolve over time—such as incremental updates, appending logs, or document edits—because CDC ensures that only the changed portions are backed up, avoiding redundant data storage for the rest of the file.

Plakar is designed with flexibility in mind, allowing the chunking algorithm to be easily swapped or updated as new advances are made in the field of Content-Defined Deduplication (CDC). This ensures that Plakar remains at the forefront of deduplication technology as research and techniques evolve.

#### FastCDC and UltraCDC

To optimize performance, Plakar currently implements advanced CDC algorithms such as **FastCDC** and **UltraCDC**, which have been validated by recent scientific research. These algorithms improve both speed and accuracy compared to earlier CDC techniques:

- **FastCDC**: This algorithm enhances chunking speed by using an optimized rolling hash function and boundary detection method. FastCDC divides files into chunks of varying sizes while maintaining performance, making it suitable for large-scale data processing environments.
  
- **UltraCDC**: Building on the strengths of FastCDC, UltraCDC further refines chunking efficiency. It uses an advanced fingerprinting mechanism to increase deduplication accuracy without impacting performance, making it ideal for high-volume environments with diverse file types.

Both algorithms enable Plakar to process large amounts of data rapidly, reducing storage overhead while ensuring minimal redundancy.

As of this writing, our implementation of FastCDC is the most efficient opensource one available performance-wise, and our UltraCDC implementation is not only almost 3x times as efficient but is the only one we could find online:

```
Benchmark_Restic_Rabin_Next-8                          1        1926270208 ns/op         557.42 MB/s          1286 chunks
Benchmark_Askeladdk_FastCDC_Copy-8                     2         686870770 ns/op        1563.24 MB/s        105327 chunks
Benchmark_Jotfs_FastCDC_Next-8                         3         473524972 ns/op        2267.55 MB/s          1725 chunks
Benchmark_Tigerwill90_FastCDC_Split-8                  3         395610639 ns/op        2714.14 MB/s          2013 chunks
Benchmark_Mhofmann_FastCDC_Next-8                      2         592342938 ns/op        1812.70 MB/s          1718 chunks
Benchmark_PlakarLabs_FastCDC_Copy-8                    9         155380773 ns/op        6910.39 MB/s          3647 chunks
Benchmark_PlakarLabs_FastCDC_Split-8                   9         120039241 ns/op        8944.92 MB/s          3647 chunks
Benchmark_PlakarLabs_FastCDC_Next-8                    9         138454935 ns/op        7755.17 MB/s          3647 chunks
Benchmark_PlakarLabs_UltraCDC_Copy-8                  20          52055098 ns/op        20627.03 MB/s         4096 chunks
Benchmark_PlakarLabs_UltraCDC_Split-8                 24          48617734 ns/op        22085.39 MB/s         4096 chunks
Benchmark_PlakarLabs_UltraCDC_Next-8                  24          48486024 ns/op        22145.39 MB/s         4096 chunks
```

See [Plakar Labs' go-cdc-chunkers](https://github.com/PlakarLabs/go-cdc-chunkers) for more details.


### Transparent Compression

Plakar integrates **transparent compression** to further reduce data storage needs. The current implementation uses LZ4 compression, which is known for its excellent balance between low resource consumption and an efficient compression ratio. LZ4 ensures fast compression and decompression speeds, making it ideal for large-scale backup environments where performance is critical.

However, Plakar is designed with future-proof flexibility. The compression algorithm is swappable, allowing for easy updates as new advances are made in the field of compression. This means that should a more advanced algorithm emerge, Plakar can adapt to leverage better compression techniques without requiring significant changes to the system architecture.

### Transparent Encryption with Future-Proof Recommendations

Security is a core component of Plakar. It implements **transparent encryption**, ensuring that all data is encrypted at the source before it is backed up. This means that data is protected throughout its entire lifecycle, both in transit and at rest.

Plakar employs encryption algorithms recommended for long-term security, such as AES-256, which is considered **future-proof** against foreseeable computational advances. In addition, Plakar's design allows for encryption algorithms to be updated or replaced, ensuring it remains secure against evolving cryptographic challenges.

A similar approach is taken regarding the on-going implementation of cryptographic signatures.


### Technical Benefits of Client-Side CDC, Compression, and Encryption
Plakar performs content-defined deduplication (CDC), compression, and encryption on the client side before any data is transmitted or stored. This approach provides several key advantages:

- Security: By encrypting data at the source, Plakar ensures that sensitive information remains protected during transmission and at rest. End-to-end encryption guarantees that even if the storage backend is compromised, the data remains unreadable.
- Reliability: The use of CDC ensures that only changed portions of files are stored, making backups more reliable by reducing the risk of data duplication and corruption. Plakar's deduplication algorithms are robust and optimize storage, ensuring reliable long-term data integrity.
- Trust: Plakar operates under a zero-trust model. Since data is encrypted on the client side, trust is not required in the server or storage infrastructure. Clients retain full control of their data, with no risk of exposure on the server side.
- Bandwidth Efficiency: With deduplication and compression happening on the client side, significantly less data needs to be transmitted to the server. This results in reduced bandwidth consumption, making backups faster and more efficient, especially over slower networks.
- Reduced Disk Access on the Server: Since deduplicated and compressed data is smaller, the server requires fewer disk accesses to store and manage it. This reduces server load, improving performance and scalability.
- Higher Deduplication Ratio: CDC maximizes deduplication efficiency, especially in environments with incremental changes or similar files. A higher deduplication ratio reduces storage overhead and increases the overall effectiveness of the backup system.
- Optimal Use of Client Resources: By performing deduplication, compression, and encryption on the client side, Plakar leverages the client’s resources, ensuring that server-side throttling does not become a bottleneck. This allows clients to fully utilize their own processing power, speeding up backup operations.
- Scalability: Offloading resource-intensive tasks like compression and deduplication to the client side enables Plakar to scale more effectively, reducing the load on centralized infrastructure.
- Distributed Repositories: Plakar uses a distributed repository model, similar to Git, allowing users to push and pull backups to multiple destinations. This design makes it easier to maintain off-site copies and clones, improving disaster recovery capabilities and offering flexibility in managing multiple backup locations.

## Real-World Applications

### Enterprise

For enterprises managing thousands of virtual machines or handling petabytes of data, Plakar offers unmatched efficiency and scalability. It provides:

- **Cost-effective Storage**: Reduced storage needs due to deduplication.
- **Secure Archiving**: With encryption and access control, sensitive data remains protected.
- **Hybrid Deployments**: Operate across both cloud and on-premise environments without sacrificing performance or security.

### Individuals and SMBs

Smaller setups can also benefit from Plakar's minimal storage footprint, making it ideal for:

- **Personal Backups**: Efficiently store personal data without requiring large storage devices.
- **SMBs**: Affordable and scalable backup solutions for growing businesses.

## Why Choose Plakar?

- **Efficiency**: With up to 90% storage reduction thanks to deduplication.
- **Security**: End-to-end encryption ensures your data stays safe.
- **Flexibility**: Deploy in any environment, whether cloud or on-premise, and retain full control over your data.
- **Open-Source Transparency**: Plakar remains accessible and adaptable for the community.

## Conclusion

In an era where data is a critical asset, Plakar offers a comprehensive solution that addresses the key challenges of traditional backup systems: inefficiency, lack of scalability, security vulnerabilities, and inflexibility. Whether you're an enterprise looking to manage massive amounts of data, or an individual seeking a reliable backup solution, Plakar provides the tools you need to secure and manage your data effectively.

For more information and to get started with Plakar, visit [Plakar Website](https://plakar.io).
