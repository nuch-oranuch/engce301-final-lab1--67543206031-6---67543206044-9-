# engce301-final-lab1--67543206031-6---67543206044-9-
อรนุช ลุงหลิ่ง 67543206031-6
ชนาธิป ระวิมี 67543206044-9

---

## Architecture Diagram

```text
Browser / Postman
       │
       │ HTTPS :443  (HTTP :80 redirect → HTTPS)
       ▼
┌─────────────────────────────────────────────────────────────┐
│  🛡️ Nginx (API Gateway + TLS Termination + Rate Limiter)    │
│                                                             │
│  /api/auth/* → auth-service:3001    (ไม่ต้องมี JWT)          │
│  /api/tasks/* → task-service:3002    [JWT required]         │
│  /api/logs/* → log-service:3003     [JWT required]         │
│  /              → frontend:80          (Static HTML)          │
└───────┬────────────────┬──────────────────┬─────────────────┘
        │                │                  │
        ▼                ▼                  ▼
┌──────────────┐ ┌───────────────┐ ┌──────────────────┐
│ 🔑 Auth Svc  │ │ 📋 Task Svc   │ │ 📝 Log Service   │
│   :3001      │ │   :3002       │ │   :3003          │
│              │ │               │ │                  │
│ • Login      │ │ • CRUD Tasks  │ │ • POST /api/logs │
│ • /verify    │ │ • JWT Guard   │ │ • GET  /api/logs │
│ • /me        │ │ • Log events  │ │ • เก็บลง DB       │
└──────┬───────┘ └───────┬───────┘ └──────────────────┘
       │                 │
       └────────┬────────┘
                ▼
     ┌─────────────────────┐
     │  🗄️ PostgreSQL       │
     │  (1 shared DB)      │
     │  • users  table     │
     │  • tasks  table     │
     │  • logs   table     │
     └─────────────────────┘