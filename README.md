# Privacy-Preserving Deep Learning Using Intel SGX

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[cite_start]This project demonstrates a proof-of-concept for privacy-preserving deep learning using Intel Software Guard Extensions (SGX)[cite: 4]. [cite_start]It features a lightweight Convolutional Neural Network (CNN) that performs both training and inference entirely within a secure SGX enclave, ensuring the confidentiality and integrity of the model and data[cite: 6, 40].

---
## 📖 About The Project

[cite_start]As deep learning models are increasingly used with sensitive data, protecting privacy has become critical[cite: 13]. [cite_start]This project tackles that challenge by leveraging a hardware-based Trusted Execution Environment (TEE) to create an isolated memory region (an enclave) for all machine learning operations[cite: 4, 18, 19].

[cite_start]The entire workflow—from loading data and training the model to running inference—is protected[cite: 6]. [cite_start]Model weights are encrypted with AES before being exported from the enclave, ensuring they remain secure even when stored in an untrusted environment[cite: 116, 117].

### Key Features

-   [cite_start]**End-to-End Security**: Both training and inference are executed securely inside the SGX enclave[cite: 6, 24].
-   [cite_start]**Data Confidentiality**: Sensitive data and model parameters are protected from the host OS and other privileged software[cite: 20, 21].
-   [cite_start]**Integrity Verification**: Remote attestation is used to verify the enclave's integrity before any operations begin[cite: 137, 201].
-   [cite_start]**Secure Model Handling**: Learned model weights are encrypted using AES-GCM before leaving the enclave and decrypted upon re-entry[cite: 116, 169, 170].

---
### 🛠️ Built With

This project was built using the following technologies:

* [cite_start]**Core Framework**: Intel SGX SDK [cite: 50]
* [cite_start]**Deep Learning Library**: Darknet (C-based library adapted for SGX) [cite: 55, 59]
* [cite_start]**Language**: C/C++ [cite: 59]
* [cite_start]**Operating System**: Ubuntu 20.04 LTS [cite: 49]
* [cite_start]**Tools**: `gcc`, `make`, `cmake` [cite: 55]

---
## 🚀 Getting Started

To get a local copy up and running, follow these steps.

### Prerequisites

-   [cite_start]**Hardware**: A laptop or server with a modern Intel CPU that supports SGX (e.g., Core i5/i7/i9)[cite: 45, 46]. [cite_start]SGX must be enabled in the BIOS[cite: 46]. [cite_start]The system used for this project had an Intel Core i7 10th generation processor[cite: 47].
-   **Software**:
    * [cite_start]Ubuntu 20.04 LTS (64-bit)[cite: 49].
    * [cite_start]Intel SGX SDK and Driver installed[cite: 50, 51].
    * [cite_start]Intel SGX Platform Software (PSW) and DCAP for attestation[cite: 52, 54].

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

1.  [cite_start]**Host Initializes Enclave**: The host application first creates and initializes the secure SGX enclave using the `sgx_create_enclave` API[cite: 137, 138].
2.  [cite_start]**Remote Attestation**: The host performs remote attestation to verify the enclave's authenticity[cite: 137, 201].
3.  **Secure Training**:
    * [cite_start]The host calls `ecall_build_network` to construct the CNN architecture inside the enclave[cite: 83, 143].
    * [cite_start]The host then calls `ecall_train_network` to begin the training process on the provided dataset, entirely within the enclave[cite: 84, 145].
    * [cite_start]After training, the learned weights are encrypted and passed back to the untrusted host using `ocall_push_weights`[cite: 117, 125].
4.  **Secure Inference**:
    * [cite_start]The host passes the encrypted weights back into the enclave[cite: 169].
    * [cite_start]The enclave uses an AES routine to decrypt the weights and validate their integrity tag[cite: 170, 171].
    * [cite_start]The network is rebuilt with the decrypted weights, and `ecall_test_network` is called to perform inference on the test data[cite: 173, 176].

---
## 📊 Results

[cite_start]Performance was compared between running the CNN inside the SGX enclave and running it on a standard CPU without SGX[cite: 253, 254]. [cite_start]The SGX environment introduces a measurable overhead due to memory encryption and other security features[cite: 257].

| Phase | Without SGX (s) | With SGX (s) |
| :--- | :--- | :--- |
| **Training** | 27.43 | 41.07 |
| **Testing** | 0.63 | 6.95 |
[cite_start][cite: 261]

[cite_start]The ~50% overhead in training is acceptable for small-scale applications where data privacy is paramount[cite: 268].

---
## 🔭 Future Work

There are several promising directions for extending this work:

-   [cite_start]**Support for Larger Models**: Explore model compression, quantization, or enclave paging to train more complex networks within EPC memory limits[cite: 273].
-   [cite_start]**Data Streaming**: Implement data streaming and mini-batching to handle larger datasets efficiently[cite: 274].
-   [cite_start]**Side-Channel Defense**: Incorporate data-oblivious algorithms and constant-time execution to harden the system against side-channel attacks[cite: 275, 276].
-   [cite_start]**Integration with Federated Learning**: Combine SGX with federated learning to enable secure, collaborative model training across multiple organizations without sharing raw data[cite: 278].

---
## 📜 License

Distributed under the MIT License. See `LICENSE` for more information.

---
## 🙏 Acknowledgments

* [cite_start]Dr. Akhilesh Tyagi [cite: 282]
* [cite_start]Iowa State University [cite: 282]
