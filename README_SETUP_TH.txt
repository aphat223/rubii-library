RUBII LIBRARY ♡ — คู่มือติดตั้งผ่าน iPhone
============================================

ไฟล์ในชุดนี้
- index.html                 หน้าเว็บ (พัฒนาต่อจากต้นแบบเดิม)
- supabase-config.js         ช่องใส่ URL และ Publishable/anon key ของ Supabase
- supabase/schema.sql        โครงสร้างฐานข้อมูล + สิทธิ์ RLS + หมวดและโค้ดตัวอย่าง
- original-prototype.html    ไฟล์ต้นแบบเดิม เก็บไว้เป็นข้อมูลอ้างอิง

ระบบที่เตรียมไว้
- คลังกลางสาธารณะ ทุกคนอ่านและคัดลอกโค้ดได้
- สมาชิกสมัคร/เข้าสู่ระบบด้วยอีเมลและรหัสผ่าน
- ปุ่มเข้าสู่ระบบด้วย Google (ต้องตั้งค่า Google provider ใน Supabase ก่อน)
- สมาชิกเพิ่มโค้ดได้ เลือกเผยแพร่สาธารณะหรือเก็บส่วนตัว
- สมาชิกแก้ไข/ลบได้เฉพาะโค้ดของตัวเอง
- มี RLS ป้องกันไม่ให้สมาชิกอ่าน/แก้ไขข้อมูลส่วนตัวของคนอื่น
- โหมดสว่าง/มืดจำค่าบนเครื่อง

ขั้นตอนที่ 1 — สร้าง Supabase project
1. เปิด https://supabase.com/dashboard ใน Safari แล้วสมัคร/เข้าสู่ระบบ
2. กด New project และตั้งชื่อเช่น rubii-library
3. ตั้งรหัสผ่านฐานข้อมูล เก็บไว้ในที่ปลอดภัย แล้วรอสร้างโปรเจกต์เสร็จ
4. เข้า Project Settings > API (หรือ Connect/API Keys ตามหน้าตา dashboard)
5. คัดลอก Project URL และ Publishable key (หรือ legacy anon public key)
   ห้ามใช้ service_role key หรือ secret key ในเว็บไซต์เด็ดขาด

ขั้นตอนที่ 2 — สร้างฐานข้อมูล
1. ใน Supabase เปิด SQL Editor > New query
2. เปิดไฟล์ supabase/schema.sql จากชุดนี้ แล้วคัดลอกเนื้อหาทั้งหมด
3. วางใน SQL Editor แล้วกด Run
4. ไปที่ Table Editor ควรเห็นตาราง categories และ snippets

ขั้นตอนที่ 3 — เชื่อมหน้าเว็บกับ Supabase
1. เปิดไฟล์ supabase-config.js ใน editor ที่แก้ไขไฟล์ข้อความได้
2. แทน PASTE_SUPABASE_PROJECT_URL_HERE ด้วย Project URL จริง
3. แทน PASTE_SUPABASE_PUBLISHABLE_OR_ANON_KEY_HERE ด้วย Publishable/anon key
4. บันทึกไฟล์ (เก็บเฉพาะ Publishable/anon key เท่านั้น ไม่ใช่ Secret key)

ขั้นตอนที่ 4 — เปิดสมัครสมาชิกอีเมล
1. Supabase > Authentication > Providers > Email
2. เปิด Email provider และตั้งค่า Confirm email ตามต้องการ
3. Authentication > URL Configuration
4. หลังเผยแพร่เว็บแล้ว ใส่ URL เว็บจริงใน Site URL และ Redirect URLs

ขั้นตอนที่ 5 — เปิด Google Login
1. Supabase > Authentication > Providers > Google แล้วเปิดใช้งาน
2. สร้าง OAuth Client ใน Google Cloud Console (OAuth consent screen / Credentials)
3. ใส่ Client ID และ Client Secret ใน Google provider ของ Supabase
4. นำ Callback URL ที่ Supabase แสดง ไปเพิ่มเป็น Authorized redirect URI ใน Google Cloud
5. ใส่ URL เว็บไซต์จริงใน Supabase Redirect URLs ด้วย
   หมายเหตุ: Google OAuth ต้องตั้งค่า Client ID/Secret ใน dashboard ด้วยตนเอง

ขั้นตอนที่ 6 — ตั้งบัญชีเจ้าของเว็บเป็น Admin
1. สมัครสมาชิกผ่านหน้าเว็บด้วยอีเมลของคุณก่อน
2. ใน Supabase เปิด SQL Editor แล้วรันคำสั่งนี้ โดยเปลี่ยนอีเมลให้ตรงกับบัญชีคุณ:

update auth.users
set raw_app_meta_data = coalesce(raw_app_meta_data, '{}'::jsonb) || '{"role":"admin"}'::jsonb
where email = 'YOUR_EMAIL@example.com';

3. ออกจากระบบและเข้าสู่ระบบใหม่ เพื่อให้ session ได้รับ role ใหม่
4. Admin สามารถจัดการหมวดหมู่ผ่าน Supabase dashboard ได้ ส่วนสมาชิกเพิ่ม snippet สาธารณะ/ส่วนตัวผ่านหน้าเว็บ

ขั้นตอนที่ 7 — เผยแพร่เว็บ
แนวทางที่เหมาะกับการแก้ไขบน iPhone คือใช้ GitHub เชื่อม Netlify
1. สร้าง repository ใหม่บน https://github.com/new
2. อัปโหลดไฟล์และโฟลเดอร์ทั้งหมดจากชุดนี้ โดยคงโครงสร้างเดิมไว้
3. เปิด https://app.netlify.com/ แล้วเลือก Add new site > Import an existing project
4. เชื่อม GitHub แล้วเลือก repository นี้
5. สำหรับเว็บชุดนี้ไม่ต้องมี Build command และ Publish directory ให้ใช้ root (.)
6. กด Deploy site แล้วรอ URL ที่ลงท้ายด้วย netlify.app
7. นำ URL นี้ไปตั้งใน Supabase Authentication > URL Configuration และ Google OAuth

ข้อควรทราบสำหรับ iPhone:
- อัปโหลดหลายไฟล์เข้า GitHub ผ่าน Safari อาจต้องเลือก Request Desktop Website หรืออัปโหลดทีละไฟล์
- ถ้าหน้าเว็บอัปโหลดโฟลเดอร์ไม่สะดวก ให้ใช้ GitHub app หรือคอมพิวเตอร์ช่วยอัปโหลดครั้งแรกได้
- หลังเชื่อม GitHub กับ Netlify แล้ว การอัปเดตไฟล์ใน repository จะ deploy ใหม่อัตโนมัติ

ความปลอดภัย
- Publishable/anon key ใช้ใน browser ได้เมื่อเปิด RLS ตาม schema นี้
- ห้ามใส่ Supabase service_role key / secret key ลงใน index.html หรือ supabase-config.js
- RLS ป้องกันข้อมูล private ระดับฐานข้อมูล ไม่ใช่แค่ซ่อนปุ่มในหน้าเว็บ
- สมาชิกที่เผยแพร่ public จะทำให้เนื้อหานั้นอ่านได้โดยทุกคนบนอินเทอร์เน็ต
- ชุดนี้เป็น starter implementation ยังต้องทดสอบการสมัคร, อีเมลยืนยัน, Google OAuth และ RLS บน project ของคุณก่อนเปิดให้สมาชิกจริง

การนำข้อมูลเก่าจาก localStorage
ข้อมูลที่เคยบันทึกในต้นแบบเดิมอยู่ใน browser ของเครื่องเดิม ไม่ได้อยู่ในไฟล์ HTML
ถ้ายังมีข้อมูลที่เพิ่มเองในต้นแบบ ให้เปิดเว็บต้นแบบเดิมบนอุปกรณ์นั้น แล้วคัดลอกข้อมูลออกมาก่อน
จากนั้นเพิ่มรายการผ่านบัญชีสมาชิกบนเว็บใหม่นี้ หรือแจ้งให้ช่วยทำเครื่องมือนำเข้าข้อมูล JSON เพิ่มเติม
