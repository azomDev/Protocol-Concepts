### **Privacy-Preserving Interactive One-Time Token Authentication Protocol**

## **1. Client Side**

1. **Fetch challenge**: The client sends their **user ID** to the server on the **challenge endpoint**.

## **2. Server Side**

1. **Receive Request**: The server receives the **user ID** on the **challenge endpoint**.

2. **Check User**: The server checks if the **user ID** exists in the database.

3. **Generate Challenge**: The server generates a **challenge** containing a **random nonce** and the **current server timestamp**.

4. **Store Challenge**: The server stores the **challenge** in a **short-term memory database (STMD)** with the **user ID** as the key.

5. **Send Challenge**: The server sends the **challenge** to the client.

## **3. Client Side**

1. **Receive Challenge**: The client receives the **challenge** from the server.

2. **Sign Challenge**: The **request body**, **challenge** and **user ID** are signed using the client’s **private key**, preventing tampering.

3. **Sign Data**: The client creates **sign data** containing the **signature** and the **user ID**.

4. **Transmit to Server**: The client sends the **request body** and **sign data** to the server on the choosen endpoint.

## **4. Server Side**

1. **Receive Request**: The server receives the **request body** and **sign data** on the choosen endpoint.

2. **Extract Data**: The server extracts the **signature** and **user ID** from the **sign data**.

3. **Retrieve Challenge**: The server retrieves the **challenge** from the **STMD** using the **user ID**.

4. **Timestamp Validation**: The server checks if the **timestamp** is within an acceptable window (last **X** seconds). If it's outdated, the request is denied to protect against Message Delaying Attacks.

5. **Verify Signature**: The server verifies the **signature** to ensure the request data hasn't been tampered with.

6. **Authorization**: If the **signature** is valid, the server **accepts the request**, processes it, and sends a response back to the client.

---

### **Optional**
- **Challenge Cleanup**: If the **challenge** is not used after a certain time, it can be removed from the **STMD** for an additional layer of privacy.

### **Assumptions**
- The **server** and **client** are **not compromised**.

### **Definitions**
- Let **X** be the number of seconds a request is valid for.
