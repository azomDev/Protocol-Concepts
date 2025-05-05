### **Privacy-Preserving Non-Interactive One-Time Token Authentication Protocol**

## **1. Client Side**

1. **One-Time Token**: A cryptographically random value is created to serve as the token. (does not have to be a nonce because in the very rare case of a collision, the server will simply reject the request).

2. **Sign Data**: The **request body**, **timestamp**, **token**, and **user ID** are signed using the client’s private key, preventing tampering.

3. **Transmit to Server**: The client sends the **signed request body**, along with the **token**, **timestamp**, and **user ID**, to the server.

## **2. Server Side**

1. **Receive Request**: The server receives the **signed request**, including the **token**, **timestamp**, and **user ID**.

2. **Timestamp Validation**: The server checks if the **timestamp** is within an acceptable window (last **X** seconds). If it's outdated, the request is denied to protect against Message Delaying Attacks.

3. **Signature Verification**: The server verifies the **signature** to ensure the request data hasn't been tampered with.

4. **Token Lookup**: The server checks if the **token** exists in the **short-term memory database (STMD)**, which is global and stores anonymized tokens from all users. This prevents identifying which user the **token** belongs to. If the **token** is found, the request is denied, as the **token** has already been processed. We do not need to worry about collisions due to the high entropy of the **tokens** and their lifetime being short.

5. **Token Expiration and Cleanup**: **Tokens** are stored in the **STMD** with a **cleanup index** of (**current index** + 2) mod 3. The server runs a cleanup process every **Y** seconds, removing expired **tokens** at the **current index**. The index wraps around at 3 (mod 3) to maintain privacy, preventing a temporary malicious local observer from knowing information such as the server’s uptime.

6. **Authorization**: If the **token** is not found in the **STMD**, the server **accepts the request**, processes it, and sends a response back to the client.

---

### **Assumptions**
- The **server** and **client** are **not compromised**.
- TODO: assumption about time sync between client and server (depending on approx, > or <, it can either be good to use, not working or unsafe)

### **Definitions**
- Let **X** be the number of seconds a request is valid for.
- Let **Y** be the number of seconds for which the server holds and cleans up expired tokens, where **Y > X** to prevent replay attacks.

### **Known Issues**
1. **Token Reset on Server Restart**: If the server is restarted and the tokens are "reset," an attacker could potentially send a **replay attack** immediately after the restart.
