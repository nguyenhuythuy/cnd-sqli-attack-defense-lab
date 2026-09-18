# Hands-on Lab: SQL Injection Exploitation & Defense Analysis

> **Module:** Network Attack and Defense Strategies (Certified Network Defender Lab)  
> **Target System:** Microsoft IIS 10.0 | ASP.NET | Microsoft SQL Server 2019  
> **Firewall Gateway:** pfSense  
> **Testing Platform:** Ubuntu Linux (Attacker Machine)  

---

## 1. Network Topology & Environment Specs

| Node Name | Role | OS / Platform | Key Software / Service |
| :--- | :--- | :--- | :--- |
| **pfSense-FW** | Gateway / Firewall | FreeBSD | Stateful Inspection, Traffic Routing |
| **Web-Target** | Vulnerable Target | Windows Server | IIS 10.0, ASP.NET, MSSQL Server 2019 |
| **Attacker-VM** | Security Testing | Ubuntu Linux | Firefox, sqlmap v1.6.4 |

---

## 2. Executive Summary

Lab thực hành phân tích chuyên sâu về cơ chế tấn công và phát hiện lỗ hổng **SQL Injection (SQLi)** ở tầng ứng dụng (Application Layer):
* **Tấn công thủ công (Manual Testing):** Sử dụng kỹ thuật Tautology-based bypass cơ chế phân quyền xem dữ liệu đơn hàng khách hàng.
* **Khai thác tự động (Automated Exploitation):** Sử dụng `sqlmap` trích xuất toàn bộ cấu trúc cơ sở dữ liệu và bảng xác thực người dùng.
* **Góc độ phòng thủ (Detection & Blue Team):** Nhận diện hành vi khai thác thông qua dấu hiệu bất thường từ mã lỗi HTTP 500 (165 lần) trên server log.

---

## 3. Step-by-Step Walkthrough

### Phase 1: Manual Exploitation (Authentication & Logic Bypass)
* **Vị trí kiểm thử:** Tham số `Id` tại endpoint `OrderDetail.aspx?Id=ORD-001`.
* **Payload sử dụng:**
  ```text
  ORD-001 ' or 1=1;--
