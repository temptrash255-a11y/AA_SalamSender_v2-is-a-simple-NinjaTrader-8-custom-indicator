

---

## 📘 README — AA_SalamSender_v2 (NinjaTrader 8 Indicator)

### 🧠 Overview

`AA_SalamSender_v2` is a simple **NinjaTrader 8 custom indicator** that demonstrates how to send periodic TCP messages from NinjaTrader to an external server or app.

Once loaded, it connects to a TCP server and sends the word **“Hello”** every **1 second** using a socket connection.

---

### ⚙️ Features

✅ Connects to a TCP server automatically
✅ Sends a message every 1 second
✅ Handles disconnections gracefully
✅ Cleans up all resources on termination
✅ 100% compatible with NinjaTrader 8 and C# 7

---

### 🧩 How It Works

When you add the indicator to a chart:

1. It connects to the TCP server at:

   ```
   IP:   127.0.0.1  
   Port: 6060
   ```
2. Every 1 second, it sends the message:

   ```
   Hello
   ```
3. When you remove it from the chart or close NinjaTrader, it closes all connections automatically.

---

### 📂 Installation

1. Copy the file `AA_SalamSender_v2.cs` to:

   ```
   Documents\NinjaTrader 8\bin\Custom\Indicators\
   ```
2. Open **NinjaTrader 8 → Tools → NinjaScript Editor**
3. Click **Compile** (F5)
4. Add the indicator to any chart:

   ```
   Indicators → AA_SalamSender_v2
   ```

---

### 🖥️ Example TCP Server (for testing)

If you want to test it locally, you can create a simple TCP server in Python:

```python
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("127.0.0.1", 6060))
server.listen(1)
print("Server listening on port 6060...")

conn, addr = server.accept()
print("Connected by", addr)

while True:
    data = conn.recv(1024)
    if not data:
        break
    print("Received:", data.decode("utf-8"))
```

---

### 🧹 Cleanup

The indicator automatically disposes of:

* `Timer`
* `NetworkStream`
* `TcpClient`

on `State.Terminated`, ensuring no open sockets remain.

---

### 🛠️ Code Reference

Key method used for periodic sending:

```csharp
sendDataTimer = new Timer(SendSalamToServer, null, 0, 1000);
```

Message transmission:

```csharp
byte[] messageBytes = Encoding.UTF8.GetBytes("Hello");
networkStream.Write(messageBytes, 0, messageBytes.Length);
```

---

### 🧑‍💻 Author

**Created by:** temper
**Purpose:** Educational example for NinjaTrader socket communication
**Version:** 2.0
**License:** Free to use and share

---

آیا مایل هستی من نسخه فارسی همین README رو هم برات بنویسم تا در انجمن‌های فارسی مثل MQL5 یا NinjaTrader.ir منتشرش کنی؟
