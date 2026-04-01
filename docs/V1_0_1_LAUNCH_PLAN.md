# Nuengdeaw AI v1.0.1 Launch Plan

เอกสารนี้เป็นแผนงานสำหรับ `v1.0.1` โดยโฟกัสที่การเปิดใช้งาน `api.sanom.ai`,
การจัดแพ็กเกจเชิงพาณิชย์, และการเตรียมหน้า landing/contact flow เพื่อรองรับการเริ่มรับลูกค้า
โดยยังไม่ถือเป็น release note และยังไม่ใช่การเปิด production release จริง

## เป้าหมายของ v1.0.1

- เตรียมความพร้อมสำหรับการเปิด `api.sanom.ai`
- จัดแพ็กเกจราคาให้ใช้งานเชิงพาณิชย์ได้จริง
- เตรียมหน้า landing page และ flow การรับ lead
- เปิด soft launch อย่างเสี่ยงต่ำ
- วางตัวชี้วัดรอบแรกเพื่อใช้ตัดสินใจเรื่องการขยายระบบและการขาย

## 1. แผนเปิด api.sanom.ai แบบ step-by-step

### 1.1 เตรียม production host

- ใช้ VPS 1 เครื่องก่อนก็พอ เช่น `2-4 vCPU` และ `4-8 GB RAM`
- ติดตั้ง Docker และ Docker Compose
- เหตุผล:
  Docker Compose เหมาะกับการรันหลาย service ใน stack เดียว และง่ายต่อการควบคุม production path รอบแรก

### 1.2 ตั้ง DNS สำหรับ api.sanom.ai

- สร้าง `A record` หรือ `CNAME` ให้ชี้ไป IP ของ server
- ถ้าใช้ Cloudflare ให้เปิด DNS ก่อน แล้วค่อยพิจารณา proxy ทีหลัง

### 1.3 ใช้ reverse proxy หน้า API

- ใช้ Caddy เป็น reverse proxy หลัก
- แผนการเชื่อมต่อ:
  - `api.sanom.ai -> Caddy`
  - `Caddy -> Node app ภายใน`
  - `Redis -> internal service`
- ใช้ automatic HTTPS ของ Caddy สำหรับ production path รอบแรก

### 1.4 แยก environment production

- ใช้ `.env.production`
- ใส่ค่าหลักอย่างน้อย:
  - `ADMIN_API_KEY`
  - `TENANTS_FILE`
  - `REDIS_URL`
  - `CORS_ALLOW_ORIGIN`
  - `STRICT_BOOTSTRAP_VALIDATION=true`
- ไม่ควรใส่ secret ตรงใน compose file

### 1.5 เปิดระบบขั้นต่ำก่อนรับลูกค้าจริง

ระบบขั้นต่ำที่ต้องพร้อม:

- `/v1/health`
- `/v1/metrics`
- tenant auth
- billing export
- pricing / quote / invoice endpoints
- logs และ backup session store

### 1.6 ทดสอบก่อนเปิดจริง

ชุดทดสอบขั้นต่ำ:

- เรียก health
- สร้าง session
- apply Phasa Tawan
- tick
- export invoice
- ทดสอบจาก Postman และ SDK

### 1.7 เปิด soft launch

- ยังไม่ยิงโฆษณาก่อน
- รับลูกค้ากลุ่มแรก `3-5 ราย`
- เก็บ feedback เรื่อง:
  - rate limit
  - latency
  - use case จริง

### 1.8 วัด KPI ชุดแรก

- `sessions/day`
- `active tenants`
- `average ticks/session`
- `quote to paid conversion`
- `support tickets/use case`

## 2. แพ็กเกจราคาที่พร้อมขายจริง

### 2.1 Research

- เหมาะกับแล็บ, มหาวิทยาลัย, นักพัฒนาเดี่ยว
- ราคา: ฟรี หรือราคาต่ำมาก
- ได้ self-host / community + limited hosted sandbox
- ไม่มี SLA

### 2.2 Starter API

- ราคา: `1,490 บาท/เดือน`
- เหมาะกับทีมเล็ก
- จำกัด sessions / rate limit / units
- ได้ basic support

### 2.3 Pro API

- ราคา: `3,990 บาท/เดือน`
- เหมาะกับ startup, UX lab, clinic sandbox
- ได้ billing export, language context, และ richer scenario usage

### 2.4 Business

- ราคา: `9,900 บาท/เดือน`
- เหมาะกับองค์กรกลาง
- ได้ admin API เต็ม, usage monitoring, และ priority support

### 2.5 Enterprise

- ราคา: quote
- dedicated instance
- on-prem / private VPC
- custom contract
- SLA
- custom feature

## 3. กฎขายที่ควรชัด

- Community = วิจัย / ทดลอง
- Hosted API = subscription
- Production embedding / on-prem = commercial contract
- Overage = คิดตาม units
- Custom integration = คิด project fee เพิ่ม

## 4. Landing page ควรมีอะไร

หน้าเว็บแรกควรมี 6 บล็อกหลัก

### 4.1 Hero

ประโยคเดียวให้เข้าใจทันที

ตัวอย่าง:

`Human-state simulation API for research, evaluation, and prototype intelligence systems.`

### 4.2 What it does

- จำลองสภาวะมนุษย์
- ควบคุมด้วย Phasa Tawan
- ใช้ใน research, NPC, customer simulation, therapy training

### 4.3 Use cases

- NPC / game AI
- customer behavior simulation
- therapy training sandbox
- agent grounding / stateful orchestration

### 4.4 Product options

- Community Edition
- Hosted API
- Enterprise / Dedicated

### 4.5 Proof

- API features
- SDK
- examples
- Docker deploy
- validation passed

### 4.6 CTA

- `Request API Access`
- `Book Research/Commercial Demo`
- `Discuss Enterprise Deployment`

## 5. Sales copy แบบสั้น

### Headline

`Simulate human state with a controllable API`

### Subheadline

`Nuengdeaw AI combines human-state simulation, Phasa Tawan scripting, and production-ready APIs for research, evaluation, and prototype systems.`

### CTA

`Request API Access`

## 6. Contact flow สำหรับรับลูกค้า

### 6.1 หน้า landing

- ปุ่ม `Request Access`

### 6.2 ฟอร์ม 1 หน้า

เก็บข้อมูลขั้นต่ำ:

- ชื่อองค์กร
- use case
- ต้องการ research หรือ commercial
- ต้องการ hosted API หรือ self-host
- ปริมาณใช้งานคร่าว ๆ
- email / LINE / phone

### 6.3 หลังส่งฟอร์ม

- auto reply
- แยก lead เป็น 3 กลุ่ม:
  - research
  - commercial eval
  - enterprise

### 6.4 ขั้นตอบกลับ

- Research:
  ส่ง docs / community path
- Commercial eval:
  นัดเดโม + ส่งราคา
- Enterprise:
  discovery call + quote

## 7. รายได้รอบแรกควรมาจากอะไร

ลำดับรายได้ที่เหมาะที่สุดช่วงต้น:

1. custom demo / integration
2. paid pilot
3. monthly hosted API
4. enterprise annual contract

## 8. ข้อสรุปเชิงกลยุทธ์

- รายได้ก้อนแรกมักไม่ได้มาจาก self-serve signup ทันที
- รายได้รอบแรกมักมาจากลูกค้าที่เห็น use case ชัดแล้วอยากทดลองหรือทำ pilot
- เมื่อมี `2-3` case study ที่น่าเชื่อถือแล้ว API subscription จะขายง่ายขึ้นมาก

## 9. สิ่งที่ควรทำต่อหลัง PR นี้

- ทำ landing page เวอร์ชันขายจริง
- ทำ contact form / lead capture flow
- ตั้ง production deployment checklist ของ `api.sanom.ai`
- ทำ v1.0.1 implementation checklist แยกจากเอกสารแผนนี้
