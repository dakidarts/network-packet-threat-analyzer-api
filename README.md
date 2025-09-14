
# 🔗 Network Packet Threat Analyzer API

Detect malicious or suspicious network traffic from **PCAP** or raw packet data.
Supports detection of:

* **Malicious / Suspicious traffic**
* **Protocol anomalies**
* **C2 beacon detection**
* **Entropy-based payload analysis**
* **Suspicious port usage**

Useful for **SOC automation, enterprise monitoring, and forensic investigations**.

![Network Packet Threat Analyzer API](https://res.cloudinary.com/ds64xs2lp/image/upload/v1757827596/network_packets_dn1mul.gif)

* [Get Started on RapidAPI](https://rapidapi.com/dakidarts-dakidarts-default/api/network-packet-threat-analyzer-api)
* [API Docs on Dakidarts](https://dakidarts.com/api/network-packet-threat-analyzer-api/)

---

## Base URL

```
https://network-packet-threat-analyzer-api.p.rapidapi.com
```

---

## Endpoints

### 🔍 `/analyze`

Analyze PCAP or raw packet data.

#### Method

* `POST`

---

### 🛠️ Request Parameters

#### **POST**

Supports three content types:

1. **Multipart Form-Data**

   * `pcap` (file, required): PCAP file to analyze.

2. **JSON Body**

   ```json
   {
     "pcap_b64": "&lt;base64 encoded PCAP&gt;"
   }
   ```

3. **Raw Bytes**

   * `Content-Type: application/octet-stream`
   * Body: raw PCAP file bytes

---


### ✅ Response Format

```json
{
  "status": "ok",
  "summary": {
    "packets_analyzed": 142,
    "unique_src_count": 3,
    "unique_dst_count": 5,
    "duration_seconds": 12.4,
    "threat_level": "medium"
  },
  "detections": [
    {
      "type": "suspicious_port",
      "port": 4444,
      "count": 8,
      "reason": "suspicious/listed port observed"
    },
    {
      "type": "beacon_behavior",
      "beacons": [
        {
          "endpoints": ["192.168.1.10", "203.0.113.45"],
          "samples": 12,
          "avg_interval_seconds": 10.2,
          "variance": 0.3
        }
      ],
      "reason": "regular periodic connections detected"
    }
  ],
  "metrics": {
    "file_size_bytes": 25874,
    "packets": 142,
    "unique_src_ips": 3,
    "unique_dst_ips": 5,
    "duration_seconds": 12.4,
    "top_protocols": [["tcp", 85], ["udp", 57]],
    "top_ports": [[80, 50], [4444, 8]],
    "average_payload_entropy": 6.9,
    "analysis_time_seconds": 0.237
  },
  "threat_score": 55
}
```

---

### ⚠️ Error Responses

| Code | Message                                   | Cause                                  |
| ---- | ----------------------------------------- | -------------------------------------- |
| 400  | `{"error": "invalid base64 in pcap_b64"}` | Bad base64 input                       |
| 400  | `{"error": "No pcap provided"}`           | Missing input file/data                |
| 404  | `{"error": "No sample found on server"}`  | `sample=true` but no sample configured |
| 413  | `{"error": "Uploaded file is too large"}` | PCAP &gt; 25MB                            |
| 500  | `{"error": "internal server error"}`      | Unexpected server failure              |

---

### 📂 Example Requests

🔹 **Analyze Packets**

`/analyze`

#### Methods

* **POST** → Production use (upload live PCAP / raw traffic).
* **GET** → Testing only (loads local `test_capture.pcap` included with the API).

---

### 🔹 Request (POST)

**1. Multipart Form Upload**

```bash
curl -X POST \
  -F "pcap=@/path/to/capture.pcap" \
  -H "x-rapidapi-key: YOUR_RAPIDAPI_KEY" \
  https://network-packet-threat-analyzer-api.p.rapidapi.com/analyze
```

**2. Raw Bytes Upload**

```bash
curl -X POST \
  --data-binary @capture.pcap \
  -H "Content-Type: application/octet-stream" \
  -H "x-rapidapi-key: YOUR_RAPIDAPI_KEY" \
  https://network-packet-threat-analyzer-api.p.rapidapi.com/analyze
```

**3. JSON Base64 Upload**

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -H "x-rapidapi-key: YOUR_RAPIDAPI_KEY" \
  -d '{"pcap_b64": "<base64_string>"}' \
  https://network-packet-threat-analyzer-api.p.rapidapi.com/analyze
```

---

### 🔹 Request (GET — **Testing Only**)

Runs analysis against the built-in `test_capture.pcap` in the project folder.

```bash
curl -X GET \
  -H "x-rapidapi-key: YOUR_RAPIDAPI_KEY" \
  https://network-packet-threat-analyzer-api.p.rapidapi.com/analyze
```

---


## ⚡ Key Features

* Detects **malicious or suspicious traffic**
* Identifies **protocol anomalies**
* Flags **C2 beacon patterns**
* Provides **threat score (0–100)**
* Supports **multipart upload, raw bytes, JSON base64**
* Built-in **GET test mode** for analysts

---

## ⚠️ Notes

* **GET is for testing only** with `test_capture.pcap`.
* For production SOC integration, always use **POST**.
* Ensure PCAP file size is within RapidAPI limits.

