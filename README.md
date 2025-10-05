

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




#region Using declarations
using System;
using System.Text;
using System.Net.Sockets;
using System.Threading;
using NinjaTrader.NinjaScript;
using NinjaTrader.Gui.Chart;
using NinjaTrader.Data;
using System.Windows.Media;
#endregion

namespace NinjaTrader.NinjaScript.Indicators
{
	public class AA_SalamSender_v2 : Indicator
	{
	    private TcpClient tcpClient;
        private NetworkStream networkStream;
        private Timer sendDataTimer;
        private string serverIp = "127.0.0.1";
        private int serverPort = 6060;

		protected override void OnStateChange()
		{
			if (State == State.SetDefaults)
			{
		 		Description = @"ارسال کلمه سلام هر 1 ثانیه از طریق TCP Socket.";
		        Name = "AA_SalamSender_v2";
		        Calculate = Calculate.OnEachTick;
		        IsOverlay = false;
				AddPlot(Brushes.Lime, "DummyPlot");
			}
			else if (State == State.DataLoaded)
			{
		        try
		        {
		            Print("Connecting to TCP server...");
		            tcpClient = new TcpClient(serverIp, serverPort);
		            networkStream = tcpClient.GetStream();
		            sendDataTimer = new Timer(SendSalamToServer, null, 0, 1000);
		            Print("Connected successfully. Sending 'سلام' every 1 second...");
		        }
		        catch (Exception ex)
		        {
		            Print("Connection error: " + ex.Message);
		        }
			}
			else if (State == State.Terminated)
			{
		        try
				{
					if (sendDataTimer != null)
					    sendDataTimer.Dispose();

					if (networkStream != null)
					    networkStream.Close();

					if (tcpClient != null)
					    tcpClient.Close();
		            Print("Terminated: resources closed.");
		        }
		        catch (Exception ex)
		        {
		            Print("Cleanup error: " + ex.Message);
		        }
			}
		}

		private void SendSalamToServer(object state)
		{
		    try
		    {
		        if (tcpClient != null && tcpClient.Connected && networkStream != null)
		        {
		            string message = "Hello";
		            byte[] messageBytes = Encoding.UTF8.GetBytes(message);
		            networkStream.Write(messageBytes, 0, messageBytes.Length);
		            Print("Sent: " + message);
		        }
		        else
		        {
		            Print("Socket not connected.");
		        }
		    }
		    catch (Exception ex)
		    {
		        Print("Error sending data: " + ex.Message);
		    }
		}

		protected override void OnBarUpdate()
		{
			// No logic here
		}
	}
}

#region NinjaScript generated code. Neither change nor remove.

namespace NinjaTrader.NinjaScript.Indicators
{
	public partial class Indicator : NinjaTrader.Gui.NinjaScript.IndicatorRenderBase
	{
		private AA_SalamSender_v2[] cacheAA_SalamSender_v2;
		public AA_SalamSender_v2 AA_SalamSender_v2()
		{
			return AA_SalamSender_v2(Input);
		}

		public AA_SalamSender_v2 AA_SalamSender_v2(ISeries<double> input)
		{
			if (cacheAA_SalamSender_v2 != null)
				for (int idx = 0; idx < cacheAA_SalamSender_v2.Length; idx++)
					if (cacheAA_SalamSender_v2[idx] != null &&  cacheAA_SalamSender_v2[idx].EqualsInput(input))
						return cacheAA_SalamSender_v2[idx];
			return CacheIndicator<AA_SalamSender_v2>(new AA_SalamSender_v2(), input, ref cacheAA_SalamSender_v2);
		}
	}
}

namespace NinjaTrader.NinjaScript.MarketAnalyzerColumns
{
	public partial class MarketAnalyzerColumn : MarketAnalyzerColumnBase
	{
		public Indicators.AA_SalamSender_v2 AA_SalamSender_v2()
		{
			return indicator.AA_SalamSender_v2(Input);
		}

		public Indicators.AA_SalamSender_v2 AA_SalamSender_v2(ISeries<double> input )
		{
			return indicator.AA_SalamSender_v2(input);
		}
	}
}

namespace NinjaTrader.NinjaScript.Strategies
{
	public partial class Strategy : NinjaTrader.Gui.NinjaScript.StrategyRenderBase
	{
		public Indicators.AA_SalamSender_v2 AA_SalamSender_v2()
		{
			return indicator.AA_SalamSender_v2(Input);
		}

		public Indicators.AA_SalamSender_v2 AA_SalamSender_v2(ISeries<double> input )
		{
			return indicator.AA_SalamSender_v2(input);
		}
	}
}

#endregion
