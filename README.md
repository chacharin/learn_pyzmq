# ZeroMQ Pub-Sub Example (with Forwarder)

โปรเจกต์นี้สาธิตการใช้ ZeroMQ ด้วยรูปแบบ Publisher-Subscriber โดยมีการแทรก node ตรงกลาง (Forwarder) ที่รับและส่งต่อข้อความ พร้อมกับเปลี่ยน `topic` เพื่อให้เข้าใจการสื่อสารแบบ pub-sub อย่างเป็นระบบ

## 🔧 โครงสร้างโปรแกรม

มีทั้งหมด 3 ไฟล์หลัก:

| ไฟล์ | บทบาท | เชื่อมต่อ | Topic ที่เกี่ยวข้อง |
|------|--------|-----------|------------------------------|
| `pub.py` | Publisher (ต้นทาง) | bind: `tcp://*:5555` | ส่ง `msg1` |
| `forwarder.py` | Node กลาง (Sub + Pub) | connect: 5555 → bind: 5556 | รับ `msg1` → ส่ง `msg2` |
| `sub.py` | Subscriber (ปลายทาง) | connect: `tcp://localhost:5556` | รับ `msg2` |

## 🚀 วิธีใช้งาน

1. ติดตั้ง ZeroMQ ก่อน:
    ```bash
    pip install pyzmq
    ```

2. เปิดเทอร์มินัล 3 หน้าต่าง แล้วรันแต่ละไฟล์ตามลำดับ:

    **เทอร์มินัลที่ 1:**
    ```bash
    python pub.py
    ```

    **เทอร์มินัลที่ 2:**
    ```bash
    python forwarder.py
    ```

    **เทอร์มินัลที่ 3:**
    ```bash
    python sub.py
    ```

คุณจะเห็นว่า `sub.py` ได้รับข้อความจาก `pub.py` โดยผ่าน `forwarder.py` และหัวข้อ (`topic`) ถูกแปลงจาก `msg1` → `msg2`

## 🧠 ความเข้าใจหลัก

- ZeroMQ แยก topic  และข้อความด้วย `send_string()` โดยใช้รูปแบบ `"topic message"`
- Subscriber ต้องระบุ topic ที่ต้องการติดตามผ่าน `.setsockopt_string(zmq.SUBSCRIBE, "topic")`
- การใช้ node ตรงกลางช่วยให้สามารถแปลงหรือกรองข้อความก่อนส่งต่อได้

## 📘 โค้ดเสริมแนะนำ

สามารถเพิ่มโปรแกรม `dual_pubsub.py` เพื่อฝึกใช้ pub + sub ในตัวเดียวกัน

## 📎 แหล่งเรียนรู้

- https://zguide.zeromq.org/
- https://zeromq.org/get-started/
