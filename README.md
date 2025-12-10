# 🌐 Proyecto de Redes Cisco – Empresa **JuanMark**
### 📌 Implementación de Infraestructura LAN para las Regiones NW y NE

Este repositorio documenta la planeación, configuración y validación de la infraestructura de red de la empresa **JuanMark**, organizada en regiones.  
El proyecto se desarrolló utilizando **Cisco Packet Tracer**, implementando tecnologías empresariales como VLAN, VTP, Router-on-a-Stick, DHCP, IPv6, WiFi y HSRP (alta disponibilidad).

---

## 🏢 Regiones Implementadas

| Región | Objetivo | Tecnologías Relevantes |
|--------|----------|------------------------|
| **NW – Noroeste** | Red con dual stack (IPv4 + IPv6) y DHCP | VLANs, Router-on-a-Stick, DHCP IPv4 y DHCPv6, SLAAC, VTP |
| **NE – Noreste** | Red redundante con WiFi y HSRP | HSRP balanceado, DHCP, VLANs, WiFi, VTP, Servicios centralizados |

📌 Por decisión de diseño final, **las regiones NO están interconectadas** (operación independiente).

---

## 📂 Documentación Incluida

| Documento | Región | Contenido |
|-----------|--------|----------|
| `DocumentacionNW.pdf` | NW | VLANs, VTP, DHCP, IPv6, pruebas y tablas |
| `DocumentacionNE.pdf` | NE | HSRP, DHCP, WiFi, VLANs, pruebas y tablas |
| `README.md` | Ambas | Resumen + guía de pruebas + descripción técnica |

---

## ⚙️ Tecnologías Implementadas

### 🧱 Switching
- Segmentación mediante **VLANs**
- **Trunking 802.1Q**
- **VTPv2** con Roles:
  - Server: 1 switch por región
  - Clients: resto de switches
- Uso de `switchport nonegotiate`
- **Spanning Tree portfast** solo en puertos de acceso
- VLAN 1 sin usuarios

### 🌐 Enrutamiento
| Región | Tecnología Principal |
|--------|----------------------|
| NW | Router-on-a-Stick + IPv4/IPv6 |
| NE | Router-on-a-Stick + HSRP balanceado |

### 📡 WiFi (Solo NE)
- Access Point para VLAN 100 (Servicios)
- Autenticación **WPA2 PSK**
- **DHCP habilitado para WiFi**
- Clientes reciben IP por `ipconfig /renew`

### 📥 DHCP
| Región | Características |
|--------|-----------------|
| NW | DHCPv4 + DHCPv6 + SLAAC |
| NE | DHCP solo para VLAN 100 + WiFi |

---

## 🗂 Tabla General de VLANs

| VLAN | Región | Nombre | Descripción |
|------|--------|--------|-------------|
| 1 | Ambas | default | No utilizada para hosts |
| 5 | NW | Marketing | Usuarios área MKT |
| 6 | NW | Ventas | Usuarios Ventas |
| 7 | NW | Compras | Usuarios Compras |
| 9 | NW | Gestión TI | Administración |
| 97–100 | NE | TI, Ventas, Compras, Servicios | Segmentación por área |
| 100 (WiFi) | NE | Servicios | Usuarios cableados + inalámbricos |

---

## 🔐 HSRP – Región NE

| VLAN | Virtual IP | Activo | Standby |
|------|------------|--------|---------|
| 97 | 148.60.97.1 | R-NE-CABRITO | R-NE-VACA |
| 98 | 148.60.98.1 | R-NE-CABRITO | R-NE-VACA |
| 99 | 148.60.99.1 | R-NE-VACA | R-NE-CABRITO |
| 100 | 148.60.100.1 | R-NE-VACA | R-NE-CABRITO |

📌 **Balanceo real:** Un router activo para la mitad de VLANs (no solo backup total).

---

## 🧪 Guía Oficial de Pruebas (NW + NE)

### 📍 1) Verificar VLANs y VTP en switches
```bash
show vlan brief
show vtp status
show interfaces trunk
```
✔ Debe existir el mismo conjunto de VLANs según región  
✔ VTP server único por región  
✔ No transportar VLAN 1 en trunk

---

### 📍 2) Verificar Router-on-a-Stick

#### 🔎 Región NW
```bash
show ip interface brief
show ipv6 interface brief
```
✔ Subinterfaces IPv4 + IPv6 operativas

#### 🔎 Región NE
```bash
show standby brief
```
✔ Debe indicar roles Active/Standby correctos

---

### 📍 3) Verificar DHCP

📌 En cualquier host (PC o Laptop):
```bash
ipconfig /release
ipconfig /renew
ipconfig
```
✔ Gateway correcto  
✔ DHCPv6 y SLAAC en NW  
✔ WiFi DHCP en NE

---

### 📍 4) Pruebas de Conectividad

#### 👨‍💻 Ping a Gateway virtual (NE)
```bash
ping 148.60.100.1
```

#### 🌍 Ping a Gateway normal (NW)
```bash
ping 148.60.5.1
```

#### 📡 WiFi
```bash
ping 8.8.8.8
ping 148.60.100.30   # Impresora NE
```

---

### 📍 5) Validación de impresoras
```bash
ping <IP impresora>
```
✔ Confirmar conexión de servicios

---

## 🏁 Conclusión

El proyecto implementa redes empresariales separadas con:
- Segmentación por VLAN
- Administración centralizada
- WiFi de servicios
- Alta disponibilidad mediante HSRP (NE)
- Dual Stack IPv4/IPv6 con DHCP completo (NW)

🔒 **Cada región es independiente y operativa**

💼 **Resultado:** Infraestructura corporativa estable, escalable y documentada profesionalmente.

---

### ✨ Autor
```
Autor: David Carmona
Ingeniería en Sistemas
```

---
