# Privacy-Preserving Deep Learning Using Intel SGX

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This project demonstrates a proof-of-concept for privacy-preserving deep learning using Intel Software Guard Extensions (SGX). It features a lightweight Convolutional Neural Network (CNN) that performs both training and inference entirely within a secure SGX enclave, ensuring the confidentiality and integrity of the model and data.

---
## 📖 About The Project

As deep learning models are increasingly used with sensitive data, protecting privacy has become critical. This project tackles that challenge by leveraging a hardware-based Trusted Execution Environment (TEE) to create an isolated memory region (an enclave) for all machine learning operations.

The entire workflow—from loading data and training the model to running inference—is protected. Model weights are encrypted with AES before being exported from the enclave, ensuring they remain secure even when stored in an untrusted environment.

### Key Features

-   **End-to-End Security**: Both training and inference are executed securely inside the SGX enclave.
-   **Data Confidentiality**: Sensitive data and model parameters are protected from the host OS and other privileged software.
-   **Integrity Verification**: Remote attestation is used to verify the enclave's integrity before any operations begin.
-   **Secure Model Handling**: Learned model weights are encrypted using AES-GCM before leaving the enclave and decrypted upon re-entry.

---
### 🛠️ Built With

This project was built using the following technologies:

* **Core Framework**: Intel SGX SDK
* **Deep Learning Library**: Darknet (C-based library adapted for SGX)
* **Language**: C/C++
* **Operating System**: Ubuntu 20.04 LTS
* **Tools**: `gcc`, `make`, `cmake`

---
## 🚀 Getting Started

To get a local copy up and running, follow these steps.

### Prerequisites

-   **Hardware**: A laptop or server with a modern Intel CPU that supports SGX (e.g., Core i5/i7/i9). SGX must be enabled in the BIOS. The system used for this project had an Intel Core i7 10th generation processor.
-   **Software**:
    * Ubuntu 20.04 LTS (64-bit).
    * Intel SGX SDK and Driver installed.
    * Intel SGX Platform Software (PSW) and DCAP for attestation.

### Installation & Build

1.  **Clone the repo**
    ```sh
    git clone [https://github.com/your-username/your-repository-name.git](https://github.com/your-username/your-repository-name.git)
    cd your-repository-name
    ```
2.  **Build the project**
    *(You should add your specific build commands here. Below is a generic example.)*
    ```sh
    cd src/
    make
    ```

---
## 🖥️ Usage

The application demonstrates the full lifecycle of secure training and inference.

1.  **Host Initializes Enclave**: The host application first creates and initializes the secure SGX enclave using the `sgx_create_enclave` API.
2.  **Remote Attestation**: The host performs remote attestation to verify the enclave's authenticity.
3.  **Secure Training**:
    * The host calls `ecall_build_network` to construct the CNN architecture inside the enclave.
    * The host then calls `ecall_train_network` to begin the training process on the provided dataset, entirely within the enclave.
    * After training, the learned weights are encrypted and passed back to the untrusted host using `ocall_push_weights`.
4.  **Secure Inference**:
    * The host passes the encrypted weights back into the enclave.
    * The enclave uses an AES routine to decrypt the weights and validate their integrity tag.
    * The network is rebuilt with the decrypted weights, and `ecall_test_network` is called to perform inference on the test data.

---
## 📊 Results

Performance was compared between running the CNN inside the SGX enclave and running it on a standard CPU without SGX. The SGX environment introduces a measurable overhead due to memory encryption and other security features.

| Phase | Without SGX (s) | With SGX (s) |
| :--- | :--- | :--- |
| **Training** | 27.43 | 41.07 |
| **Testing** | 0.63 | 6.95 |


The ~50% overhead in training is acceptable for small-scale applications where data privacy is paramount.

---
## 🔭 Future Work

There are several promising directions for extending this work:

-   **Support for Larger Models**: Explore model compression, quantization, or enclave paging to train more complex networks within EPC memory limits.
-   **Data Streaming**: Implement data streaming and mini-batching to handle larger datasets efficiently.
-   **Side-Channel Defense**: Incorporate data-oblivious algorithms and constant-time execution to harden the system against side-channel attacks.
-   **Integration with Federated Learning**: Combine SGX with federated learning to enable secure, collaborative model training across multiple organizations without sharing raw data.

---
## 📜 License

Distributed under the MIT License. See `LICENSE` for more information.

---
## 🙏 Acknowledgments

* Dr. Akhilesh Tyagi
* Iowa State University
