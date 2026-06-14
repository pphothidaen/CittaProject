# Citta - Zero-Waste Agentic AI Platform (Docker Compose Distribution)

ยินดีต้อนรับสู่ Citta Docker Compose Distribution นี่คือ Repository ที่ออกแบบมาเพื่อให้คุณสามารถนำระบบ Citta Agentic AI ไปติดตั้งและรันบนเครื่อง Local หรือ Server ได้อย่างรวดเร็วโดยใช้ Docker images ที่ถูก build ไว้แล้ว

สำหรับ Source code ตัวเต็ม กรุณาดูที่ [Citta-Core repository](https://github.com/pphothidaen/citta-core)

## ความต้องการของระบบ (System Requirements)

- **Docker Engine** 24.0+ และ **Docker Compose** V2
- **RAM**: ขั้นต่ำ 8GB (แนะนำ 16GB สำหรับรันโมเดล Local)
- **Disk**: พื้นที่ว่าง 20GB+ (สำหรับ Docker images, ไฟล์โมเดล, และ Vector DB)

## โครงสร้างสถาปัตยกรรม

Distribution นี้จะจำลองการรันแบบ Multi-node ไว้ในเครื่องเดียว:
- **NATS JetStream**: ระบบ Event bus ที่เชื่อมต่อทุก Services เข้าด้วยกัน
- **ChromaDB**: เป็น Memory fabric สำหรับ Semantic cache และ Artifacts
- **OpenVINO OVMS**: Local inference provider สำหรับรันโมเดล Qwen-1.5B 
- **Cognitive Kernel**: เป็น API Gateway หลัก (`:8002`)
- **Inference Mesh**: จัดการเรื่องการรันคำสั่งกับโมเดล
- **Governance**: ทำหน้าที่ตรวจสอบคุณภาพ (Quality gate) และ Evaluate (`:8004`)
- **Learning Factory**: ทำงานเบื้องหลังเพื่อนำข้อมูลกลับมาเรียนรู้

## เริ่มต้นใช้งาน (Quick Start - Local Mode)

โดยค่าเริ่มต้น ระบบจะถูกตั้งค่าให้เป็น "Local-Only Mode" โดยใช้ OpenVINO สำหรับรันโมเดลภายในเครื่อง ซึ่งไม่จำเป็นต้องใช้ API keys ใดๆ

1. **โคลน Repository นี้**
   ```bash
   git clone https://github.com/pphothidaen/CittaProject.git
   cd CittaProject
   ```

2. **คัดลอกไฟล์ตั้งค่า (Environment Variables)**
   ```bash
   cp .env.example .env
   ```
   *(ทางเลือก) หากต้องการใช้โมเดลภายนอก (Google, OpenAI ฯลฯ) สามารถเข้าไปแก้ไฟล์ `.env` ได้*

3. **รันระบบ**
   ```bash
   docker compose up -d
   ```
   *หมายเหตุ: ในการรันครั้งแรก ระบบจะทำการโหลด Docker images ขนาดหลายกิกะไบต์ รวมถึงโหลดโมเดลสำหรับ OpenVINO กรุณารอสักครู่*

4. **ตรวจสอบระบบ**
   รอประมาณ 30 วินาทีให้ระบบเริ่มทำงานทั้งหมด จากนั้นทดสอบเรียก API ของ Kernel:
   ```bash
   curl http://localhost:8002/health
   ```
   สามารถเข้าดูหน้าต่าง API documentation ได้ที่ `http://localhost:8002/docs`

## การปิดระบบ

เพื่อปิดระบบโดยยังเก็บข้อมูลไว้ (Cache, Database):
```bash
docker compose down
```

หากต้องการล้างข้อมูลทั้งหมด:
```bash
docker compose down -v
```
