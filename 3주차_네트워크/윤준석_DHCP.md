# **DHCP**

## **๐ DHCP๋?**

- Dynamic Host Configuration Protocol์ ์ฝ์๋ก Host IP ๊ตฌ์ฑ ๊ด๋ฆฌ๋ฅผ ๋จ์ํํ๋ IP ํ์ค ํ๋กํ ์ฝ์.
- ์ฝ๊ฒ ๋งํด์ DHCP ์๋ฒ๊ฐ ํด๋ผ์ด์ธํธ์ IP์ฃผ์, ์๋ธ๋ท ๋ง์คํฌ, ๊ฒ์ดํธ์จ์ด, DNS ์๋ฒ ๋ฑ์ ์ ๋ณด๋ฅผ ์๋์ผ๋ก ํ ๋นํด์ฃผ๋ ํ๋กํ ์ฝ์ด๋ผ๊ณ  ์๊ฐํ๋ฉด ๋จ.
- ๋คํธ์ํฌ ๊ด๋ฆฌ์๊ฐ ํด์ผ ํ  ์์์ ๊ฐ์ํํ๋ฉฐ, DHCP ์์ด๋ ์๋์ผ๋ก IP ์ฃผ์๋ฅผ ํ ๋นํด์ผ ํจ.

## **๐ DHCP ๋์ ๋ฐฉ์**

- ํด๋ผ์ด์ธํธ๊ฐ ๋คํธ์ํฌ์ ์ฐ๊ฒฐ ์ IP์ฃผ์๋ฅผ ์์ฒญํจ.
- IP์ฃผ์ ์์ฒญ์ DHCP ์๋ฒ๋ก ์ ๋ฌ๋๋ฉฐ ์๋ฒ๋ ์ฃผ์๋ฅผ ํ ๋นํ๊ณ  ๋ชจ๋ํฐ๋งํ๋ฉฐ ์ฐ๊ฒฐ ํด์  ์ ์ฃผ์๋ฅผ ๋ค์ ๊ฐ์ ธ์ด.
- ํด๋น IP์ฃผ์๋ ๋ค๋ฅธ ํด๋ผ์ด์ธํธ์ ์ฌํ ๋นํ  ์ ์์.
- ํด๋ผ์ด์ธํธ๋ IP์ฃผ์๋ฅผ ์ด์ฉํด ๋ด๋ถ ๋ฐ ๊ณต์ฉ ๋คํธ์ํฌ์ ํต์ ํ  ์ ์์.

---

- DHCP์ ๋์ ๋ฐฉ์์๋ Lease(์๋), Renewal(๊ฐฑ์ ), Release(ํด์ )๊ฐ ์์.
  - DHCP๋ฅผ ํตํ IP์ฃผ์ ํ ๋น์ ์๋ ๊ฐ๋์.
  - DHCP ์๋ฒ๊ฐ ํด๋ผ์ด์ธํธ์๊ฒ ์๊ตฌ์ ์ผ๋ก IP์ฃผ์๋ฅผ ํ ๋นํ๋ ๊ฒ์ด ์๋๋ผ ์๋ ๊ธฐ๊ฐ์ ์ ํ์ฌ ๊ทธ ๊ธฐ๊ฐ ๋์๋ง ์ฌ์ฉํ๊ฒ ํจ.
  - ์๋๊ธฐ๊ฐ ์ดํ์ ํด๋น IP์ฃผ์๋ฅผ ์ฌ์ฉํ๋ ค๋ฉด ๊ฐฑ์ (Renewal)์ ํด์ผ ํจ.
  - ๋ ์ด์ ํด๋น IP์ฃผ์๊ฐ ํ์ํ์ง ์๋ค๋ฉด ํด์ (Release)๋ฅผ ํด์ผ ํจ.

### **Lease - DHCP DORA Process**


![dhcp_lease](https://www.cisco.com/c/dam/en/us/support/docs/switches/catalyst-9300-series-switches/217366-configure-dhcp-in-ios-xe-evpn-vxlan-01.png)

1. DHCP Discover
   - ํด๋ผ์ด์ธํธ๊ฐ ๋คํธ์ํฌ์ ์ฐ๊ฒฐ ์ `DISCOVER` ๋ฉ์์ง๋ฅผ ๋ธ๋ก๋์บ์คํธ๋ก ์ ์กํจ.
   - ํด๋ผ์ด์ธํธ๋ DHCP ์๋ฒ์ ์ฃผ์๋ฅผ ๋ชจ๋ฅด๊ธฐ ๋๋ฌธ์ ๋ธ๋ก๋์บ์คํธ๋ก ์ ์กํ์ฌ DHCP ์๋ฒ๋ฅผ ์ฐพ์.
2. DHCP Offer
   - DHCP ์๋ฒ๋ `OFFER` ๋ฉ์์ง๋ฅผ ์ ๋์บ์คํธ(๋๋ ๋ธ๋ก๋์บ์คํธ)๋ก ์ ์กํจ.
   - DHCP ์๋ฒ๋ `DISCOVER` ๋ฉ์์ง๋ฅผ ์์ ํ๋ฉด `OFFER` ๋ฉ์์ง๋ฅผ ํด๋ผ์ด์ธํธ์๊ฒ ์ ์กํจ.
   - ๊ตฌ์ฑ ๋งค๊ฐ๋ณ์(์: IP ์ฃผ์, ์๋ธ๋ท ๋ง์คํฌ, ๊ฒ์ดํธ์จ์ด, DNS ์๋ฒ ๋ฑ)๋ฅผ ํฌํจํ์ฌ ์ ์กํจ.
3. DHCP Request
   - ํด๋ผ์ด์ธํธ๊ฐ `REQUEST` ๋ฉ์์ง๋ฅผ ๋ธ๋ก๋์บ์คํธ๋ก ์ ์กํจ.
   - DHCP ์๋ฒ์ ์กด์ฌ๋ฅผ ์์๊ธฐ ๋๋ฌธ์ `REQUEST` ๋ฉ์ธ์ง๋ฅผ ํตํด ํ๋์ DHCP ์๋ฒ๋ฅผ ์ ํํจ.
   - ์ ํํ DHCP ์๋ฒ์๊ฒ ํด๋ผ์ด์ธํธ๊ฐ ์ฌ์ฉํ  ๋คํธ์ํฌ ๊ตฌ์ฑ ๋งค๊ฐ๋ณ์๋ฅผ ์์ฒญํจ.
4. DHCP Ack
   - DHCP ์๋ฒ๋ `ACK` ๋ฉ์์ง๋ฅผ ์ ๋์บ์คํธ(๋๋ ๋ธ๋ก๋์บ์คํธ)๋ก ์ ์กํจ.
   - ํด๋ผ์ด์ธํธ๊ฐ DHCP ์๋ฒ๋ก ๋ณด๋ธ ์์ฒญ ๋คํธ์ํฌ ๊ตฌ์ฑ ๋งค๊ฐ๋ณ์๋ฅผ ํ์ธํ ๋ค ํด๋น ๋คํธ์ํฌ ์ ๋ณด๋ฅผ ์ ๋ฌํด ์ค.

### **Renewal**

![dhcp_renewal](https://crnetpackets.files.wordpress.com/2017/04/dhcp-basics-renew4.png)

- ์๋ ๊ธฐ๊ฐ์ด ๋๋ฌ์ ๋ IP์ฃผ์ ํ ๋น์ ํด์ ํ๊ณ  ๋ค์ ์๋ํ๊ฒ ๋๋ฉด ๋ถํ์ํ ๋ธ๋ก๋์บ์คํธ ํธ๋ํฝ์ด ๋ฐ์ํจ.
- ์ด๋ฅผ ์ํด ์๋ ๊ธฐ๊ฐ์ด ๋๋ฌ์ ๋ ์๋ ๊ธฐ๊ฐ์ ์ฐ์ฅํ๋ Renewal ๊ณผ์ ์ด ์กด์ฌํจ.
- ๊ฐฑ์  ๊ณผ์ ์์๋ ํด๋ผ์ด์ธํธ๊ฐ DHCP ์๋ฒ์ ์กด์ฌ๋ฅผ ์๊ณ  ์๊ธฐ ๋๋ฌธ์ Discover & Offer ๊ณผ์ ์ด ๋ถํ์ํจ.

### **Release**

- ์๋ ๊ธฐ๊ฐ์ด ๋๋ฌ์ ๋ ๋ ์ด์ ํด๋น IP์ฃผ์๊ฐ ํ์ํ์ง ์๋ค๋ฉด ํด์ (Release)๋ฅผ ํด์ผ ํจ.
- DHCP Release ๊ณผ์ ์ ํตํด ํด์ ํ  ์ ์์.
  - ํด๋ผ์ด์ธํธ๊ฐ `RELEASE` ๋ฉ์์ง๋ฅผ ์ ๋์บ์คํธ๋ก ์ ์กํ๊ณ  IP์ฃผ์๋ฅผ ๋ฐํํจ.

## **๐จ DHCP Relay Agent**

### **L2, L3, L4 Switch**

| Switch | Description |
| --- | --- |
| L2 Switch | MAC ์ฃผ์๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ๋ฐ์ดํฐ๋ฅผ ์ ๋ฌํ๋ ์ค์์น, Layer2 Switch |
| L3 Switch | ๊ณต์ ๊ธฐ๋ผ๊ณ  ์๊ฐํ๋ฉด ๋จ, ํธ๋ํฝ ๋ชจ๋ํฐ๋ง, VLAN ๋ฑ์ ๊ธฐ๋ฅ์ ํ๋ ์ค์์น, Layer3 Switch |
| L4 Switch | ๋ก๋๋ฐธ๋ฐ์ฑ ๊ธฐ๋ฐ์ผ๋ก ํธ๋ํฝ ๋ถ์ฐ์ ์ํ ์ค์์น, Layer4 Switch |

---

![dhcp_relay_server](https://www.cisco.com/c/dam/en/us/support/docs/switches/catalyst-9300-series-switches/217366-configure-dhcp-in-ios-xe-evpn-vxlan-02.png)

- ์ DORA Process๋ DHCP ์๋ฒ๊ฐ ๊ฐ์ ์๋ธ๋ท์ ์กด์ฌํด์ผ ๊ฐ๋ฅํ๋ฉฐ ์ผ๋ฐ์ ์ผ๋ก DHCP ์๋ฒ๋ ๋ค๋ฅธ ์๋ธ๋ท์ ์กด์ฌํจ.
- ํด๋ผ์ด์ธํธ๋ L2 ๋คํธ์ํฌ์ ์กด์ฌํ๋๋ฐ, L2 ๋คํธ์ํฌ๋ง๋ค DHCP ์๋ฒ๊ฐ ์กด์ฌํ๋ ๊ฒ์ ๋นํจ์จ์ ์.
- ๋ณดํต DHCP ์๋ฒ๋ L3 ๋คํธ์ํฌ์ ์กด์ฌํ๋ฉฐ, ์ด๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด L2 ๋คํธ์ํฌ์๋ DHCP Relay Agent๊ฐ ์กด์ฌํจ.
- ๊ฐ์ ์ฉ์์ ๊ณต์ ๊ธฐ๋ฅผ ์ฌ์ฉํ์ฌ IP์ฃผ์๋ฅผ ํ ๋น๋ฐ๋ ๊ฒฝ์ฐ ๊ณต์ ๊ธฐ ๋ด์ DHCP ์๋ฒ๊ฐ ์กด์ฌํ๋ฏ๋ก ๋ค๋ฅธ ๋คํธ์ํฌ์ ์์ฒญํ  ํ์๊ฐ ์์ง๋ง, ๊ธฐ์์ ๊ฒฝ์ฐ DHCP๊ฐ ๋ผ์ฐํฐ ๋ฐ์ ์กด์ฌํ๋ ๊ฒฝ์ฐ๊ฐ ์์.

## **๐ DHCP ์ฅ์ **

- IP์ฃผ์ ๊ด๋ฆฌ ์ฉ์ด
  - DHCP๊ฐ ์๋ ๋คํธ์ํฌ์์๋ IP์ฃผ์๋ฅผ ์๋์ผ๋ก ์ง์ ํด์ค์ผ ํจ.
  - ์ถฉ๋์ด ๋ฐ์ํ  ์ ์์ผ๋ฉฐ, IP์ฃผ์๋ฅผ ์ฌ์ฌ์ฉํ๊ธฐ ์ํด IP์ฃผ์๋ฅผ ํด์ ํด์ผ ํจ.
  - ํด๋ผ์ด์ธํธ๊ฐ ๋ค๋ฅธ ๋คํธ์ํฌ๋ก ์ด๋ํ๊ฒ ๋๋ฉด ํด๋น ํด๋ผ์ด์ธํธ๋ฅผ ์๋์ผ๋ก ์์ ํด์ค์ผ ํจ.
- IP์ฃผ์ ๊ตฌ์ฑ ์ ๋ขฐ์ฑ
  - ๋์ผํ IP์ฃผ์๋ฅผ ์ด์ฉํ๋ ํด๋ผ์ด์ธํธ ์ฌ์ด์ ์ถฉ๋์ ๋ฐฉ์งํจ.
  - ์ถฉ๋์ด ๋ฐ์ํ๋ ๊ฒฝ์ฐ ๋ ํด๋ผ์ด์ธํธ ๋ชจ๋ ์ธํฐ๋ท์ ์ฐ๊ฒฐํ์ง ๋ชปํ๊ฒ ํจ.
- ๋์ ์ด๋์ฑ
  - ํด๋ผ์ด์ธํธ๊ฐ ๋ค๋ฅธ ๋คํธ์ํฌ๋ก ์ด๋ํ๋๋ผ๋ IP์ฃผ์๋ฅผ ๋ณ๊ฒฝํ  ํ์๊ฐ ์์.

## **๐ DHCP ๋จ์ **

- IP์ฃผ์ ํ ๋น์ด DHCP ์๋ฒ์ ์์กด์ ์.
  - DHCP ์๋ฒ๊ฐ ๋ค์ด๋๋ฉด ํด๋ผ์ด์ธํธ๋ ์ธํฐ๋ท์ ์ฐ๊ฒฐํ  ์ ์์.
- DHCP๋ก ์ธํ ๋ค์ํ ๋ณด์ ๋ฌธ์ ๊ฐ ๋ฐ์ํจ.
  - ์น์ธ๋ฐ์ง ์์ DHCP ์๋ฒ๊ฐ ์๋ชป๋ ์ ๋ณด๋ฅผ ํด๋ผ์ด์ธํธ์๊ฒ ์ ๊ณตํ  ์ ์์.
  - ์น์ธ๋ฐ์ง ์์ ํด๋ผ์ด์ธํธ๊ฐ DHCP ์๋ฒ๋ฅผ ๊ฐ๋ก์ฑ ๋ฆฌ์์ค์ ๋ํ ์ ๊ทผ ๊ถํ์ ์ป์ ์ ์์.
  - ์์ฑ ํด๋ผ์ด์ธํธ๊ฐ DHCP ๋ฆฌ์์ค๋ฅผ ์๋ชจ์ํฌ ์ ์์.

## **DHCP ๋ณด์ ๋ฌธ์ **

### **DHCP Spoofing**

- DHCP๋ UDP ๊ธฐ๋ฐ์ผ๋ก ๋์ํ๋ฏ๋ก ํด๋น DHCP ์๋ฒ๊ฐ ์ธ์ฆ ๋ ์๋ฒ์ธ์ง ํ์ธํ  ์ ์์.
- DHCP ํ๋กํ ์ฝ์ด ์ธ์ฆ์ด ๋ถ๊ฐ๋ฅํ ์ทจ์ฝ์ ์ ์ด์ฉํ์ฌ DHCP ์๋ฒ๋ก ์์ฅํ์ฌ ์กฐ์๋ ์ ๋ณด๋ฅผ ์ ์กํ๋ ๊ณต๊ฒฉ๋ฒ.

![dhcp_spoofing](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9947BD3359EC4B7B20)

- DHCP Snooping์ ํตํด DHCP Spoofing ๊ณต๊ฒฉ์ ๋ฐฉ์ดํ  ์ ์์.
  - DHCP ์๋ฒ๋ก๋ถํฐ ์ ๋ฌ๋ฐ์ DHCP ํจํท์ ๊ฒ์ฌํ์ฌ ์์ฅ๋ DHCP ์๋ฒ๋ก๋ถํฐ ์ ๋ฌ๋ฐ์ ํจํท์ธ์ง ํ์ธํ๋ ๊ธฐ๋ฅ์.

### **DHCP Starvation Attack**

![dhcp_starvation_attack](https://ars.els-cdn.com/content/image/1-s2.0-S0045790612001140-fx1.jpg)

- DHCP๋ MAC์ฃผ์ ๊ธฐ๋ฐ์ผ๋ก ํด๋ผ์ด์ธํธ๋ค์๊ฒ IP์ฃผ์๋ฅผ ํ ๋นํจ.
- ์ด ์ ์ ์ด์ฉํ์ฌ MAC์ฃผ์๋ฅผ ๋๋คํ๊ฒ ๋ณ๊ฒฝํ๋ฉฐ DHCP ์๋ฒ์ ์์ฒญ์ ๋ณด๋ด IP์ฃผ์๋ฅผ ํ ๋น๋ฐ๋ ๊ณต๊ฒฉ๋ฒ.
- DHCP ์๋ฒ๊ฐ IP์ฃผ์๋ฅผ ํ ๋นํ  ์ ์๋ ์ต๋ ๊ฐ์๋ฅผ ๋์ด์๋ ์์ฒญ์ ๋ณด๋ด๋ฉด์ ์๋ฒ์ ์์์ ์๋ชจ์ํค๊ณ  ๊ฐ์ฉ IP์ฃผ์๋ฅผ ๋ชจ๋ ํ ๋นํ๊ฒ ๋๋ฉด ์ ์ ํด๋ผ์ด์ธํธ๋ IP์ฃผ์๋ฅผ ํ ๋น๋ฐ์ง ๋ชปํจ.
- DoS ๊ณต๊ฒฉ์ด๋ผ๊ณ  ํ  ์ ์์.
