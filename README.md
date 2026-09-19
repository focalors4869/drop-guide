# drop — วิธีใช้

**[ภาษาไทย](#drop--วิธีใช้) · [English](#drop--how-to-use)**

drop คือเว็บส่งไฟล์ชั่วคราว ไม่ต้องสมัคร ไม่ต้องล็อกอิน ไม่มีรหัสผ่าน
คนส่งอัปโหลดเสร็จจะได้รหัสมาหนึ่งชุด แล้วส่งรหัสนั้นให้คนรับ
ไฟล์จะอยู่ได้ 1 ชั่วโมงนับจากตอนอัปโหลด หลังจากนั้นโหลดไม่ได้อีก และถูกลบออกจากเซิร์ฟเวอร์

เว็บอยู่ที่ **https://drop-4869.vercel.app**

---

## ฝั่งคนรับ

### วิธีที่เร็วที่สุด ไม่ต้องติดตั้งอะไรเลย

เปิด **https://drop-4869.vercel.app/get** กรอกรหัส กด Find files แล้วกด Download ทีละไฟล์
ถ้าได้มาไม่กี่ไฟล์ จบแค่นี้ ไม่ต้องอ่านต่อ

### วิธีที่ใช้ CLI (แนะนำถ้าได้หลายไฟล์)

**1. ถ้าเครื่องยังไม่มี npm**

แปลว่ายังไม่มี Node.js ให้โหลดตัวติดตั้ง LTS จาก https://nodejs.org ติดตั้งตามปกติ
เสร็จแล้ว **ปิด terminal แล้วเปิดใหม่** ขั้นนี้สำคัญ ถ้าไม่ปิดจะยังใช้คำสั่งไม่เจอ
เช็คด้วยสองคำสั่งนี้ ถ้าขึ้นเลขเวอร์ชันแปลว่าใช้ได้

```bash
node -v
npm -v
```

**2. ติดตั้ง CLI ครั้งเดียวจบ**

```bash
npm install -g drop-transfer-cli
```

คำสั่งที่ได้ชื่อ `drop`

**3. บอกที่อยู่เซิร์ฟเวอร์ ครั้งเดียวจบ**

ถ้าข้ามข้อนี้ ทุกครั้งต้องเติม `-u https://drop-4869.vercel.app` ต่อท้ายคำสั่งเอง

Windows (PowerShell หรือ CMD):

```powershell
setx DROP_URL "https://drop-4869.vercel.app"
```

macOS / Linux:

```bash
export DROP_URL="https://drop-4869.vercel.app"
```

เสร็จแล้ว **ปิด terminal แล้วเปิดใหม่** อีกครั้ง ถึงจะเริ่มใช้ค่าได้

**4. โหลดไฟล์**

```bash
drop get -d ABCDE-23456
```

ไฟล์จะไปอยู่ในโฟลเดอร์ที่คุณยืนอยู่ตอนพิมพ์คำสั่ง
ถ้าอยากลงโฟลเดอร์อื่น ใส่ต่อท้ายได้เลย มันสร้างโฟลเดอร์ให้ถ้ายังไม่มี

```bash
drop get -d ABCDE-23456 ./downloads
```

ได้ผลลัพธ์แบบนี้:

```
Drop found
Files: 2

Downloading image.png (3.2 MB)...
Downloading รายงาน.pdf (820.5 KB)...

Saved 2 files to /home/you/downloads
  image.png (3.2 MB)
  รายงาน.pdf (820.5 KB)
Saved successfully.
```

### พิมพ์รหัสยังไงก็ได้

ตัวพิมพ์เล็กพิมพ์ใหญ่ไม่สำคัญ และไม่ต้องใส่ dash ก็ได้ `abcde23456` ใช้ได้เหมือน `ABCDE-23456`
แต่ในรหัสจะไม่มีตัวอักษร **I, O** และเลข **0, 1** เลย ถ้าคิดว่าเห็นเป็นตัวนั้นให้เพ่งดูใหม่

---

## ปัญหาที่เจอบ่อย

**`'drop' is not recognized as an internal or external command` (Windows)**
**`drop: command not found` (macOS, Linux)**

ยังไม่ได้ติดตั้ง หรือ terminal ยังไม่รู้จักคำสั่งใหม่
รัน `npm install -g drop-transfer-cli` แล้วปิด terminal เปิดใหม่
ถ้ายังไม่หาย เช็คว่าติดตั้งอยู่จริงไหมด้วย `npm ls -g drop-transfer-cli`

**`Network error: could not reach http://localhost:3000`**

อันนี้เจอบ่อยสุด แปลว่ายังไม่ได้บอกที่อยู่เซิร์ฟเวอร์ CLI จึงเดาว่าใช้เครื่องตัวเอง
กลับไปทำข้อ 3 หรือสั่งแบบนี้แทน

```bash
drop get -d ABCDE-23456 -u https://drop-4869.vercel.app
```

**`Invalid code: ...`**

รหัสผิดรูปแบบ ต้องเป็นตัวอักษรหรือเลข 5 ตัว ขีดกลาง แล้วต่อด้วย 5 ตัว เช่น `ABCDE-23456`

**`Drop not found`**

ไม่มีรหัสนี้อยู่ในระบบ พิมพ์ผิด หรือคนส่งให้รหัสผิดมา ขอเขาเช็คอีกที

**`Drop expired`**

เกิน 1 ชั่วโมงแล้ว ขอให้คนส่งอัปโหลดใหม่

**`Download failed: ... is incomplete`**

เน็ตหลุดกลางทาง ไฟล์ที่โหลดค้างถูกลบทิ้งให้แล้วอัตโนมัติ สั่งคำสั่งเดิมซ้ำได้เลย

**`Rate limited by the server`**

ยิงถี่เกินไป รอสักครู่แล้วลองใหม่

**ติดตั้งแล้วขึ้น `EACCES` หรือ `permission denied` (macOS, Linux)**

Node ที่ลงจากตัวติดตั้งของระบบต้องใช้สิทธิ์ root ถึงจะเขียนโฟลเดอร์กลางได้
อย่าใช้ `sudo` ให้ติดตั้ง Node ผ่าน nvm แทน แล้วคำสั่งจะลงในโฟลเดอร์ของผู้ใช้เอง

**ได้ไฟล์ชื่อซ้ำกับอันเก่า**

ถ้าในดรอปเดียวกันมีไฟล์ชื่อซ้ำกัน มันจะเติมเลขให้เป็น `image (1).png`
แต่ถ้าโฟลเดอร์ปลายทางมีไฟล์ชื่อนั้นค้างอยู่จากรอบก่อน มันจะเขียนทับของเดิม
เลี่ยงได้โดยโหลดลงโฟลเดอร์ใหม่ทุกครั้ง

**หาไฟล์ไม่เจอ**

ไฟล์ไปอยู่ในโฟลเดอร์ที่คุณยืนอยู่ตอนพิมพ์คำสั่ง ดูที่ prompt ก็รู้
เช่น `PS D:\>` คือไฟล์ไปอยู่ `D:\` ถ้าไม่ชอบ ให้ระบุโฟลเดอร์เองในคำสั่ง

---

## ข้อควรรู้

- ไฟล์อัปโหลดขนาดสูงสุด 100 MB ต่อไฟล์ สูงสุด 20 ไฟล์ต่อดรอป
- หมดอายุแล้วโหลดไม่ได้ทันที แต่ไฟล์จริงจะถูกลบออกจากที่เก็บภายในประมาณหนึ่งวัน
- สั่ง `drop get -h` เพื่อดูตัวเลือกทั้งหมด

---

# drop — how to use

**[ภาษาไทย](#drop--วิธีใช้) · [English](#drop--how-to-use)**

drop is a temporary file transfer service. No sign-up, no login, no password. The
sender uploads and gets a code, then passes that code on. Files live for one hour
and are then unreachable.

Server: **https://drop-4869.vercel.app**

## Receiving

### No installs

Open **https://drop-4869.vercel.app/get**, type the code, press *Find files*, and
download each file. If a drop has one or two files, stop here.

### With the CLI

**1. No npm yet?** That means no Node.js. Install the LTS build from
https://nodejs.org, then **close and reopen your terminal** — otherwise the new
commands are not on your `PATH` yet.

```bash
node -v
npm -v
```

Both should print a version.

**2. Install the CLI once.**

```bash
npm install -g drop-transfer-cli
```

This gives you the `drop` command.

**3. Tell it where the server is, once.** Skip this and you must add
`-u https://drop-4869.vercel.app` to every command.

Windows:

```powershell
setx DROP_URL "https://drop-4869.vercel.app"
```

macOS / Linux:

```bash
export DROP_URL="https://drop-4869.vercel.app"
```

Close and reopen the terminal for `setx` to take effect.

**4. Download.**

```bash
drop get -d ABCDE-23456
```

Files are written to the directory you are standing in. To put them somewhere
else, add the path — the directory is created if it does not exist:

```bash
drop get -d ABCDE-23456 ./downloads
```

Codes ignore case and the dash: `abcde23456` works. The code alphabet has no
`I`, `O`, `0` or `1`, so if you think you see one, look again.

## Common problems

**`drop` is not recognized / `command not found`** — not installed, or the
terminal does not know about it yet. Install it, reopen the terminal, and check
with `npm ls -g drop-transfer-cli`.

**`could not reach http://localhost:3000`** — the CLI has no server URL, so it
assumed your own machine. Do step 3, or pass it inline:

```bash
drop get -d ABCDE-23456 -u https://drop-4869.vercel.app
```

**`Invalid code`** — wrong shape. Codes look like `ABCDE-23456`: five
characters, a dash, five more.

**`Drop not found`** — no such code. Typo, or the sender misread it.

**`Drop expired`** — past the one hour window. Ask for a fresh upload.

**`Download failed: ... is incomplete`** — the connection dropped. The partial
file was removed for you; just run the command again.

**`Rate limited by the server`** — too many requests. Wait a moment.

**`EACCES` during install (macOS, Linux)** — the system Node needs root to write
to its global folder. Do not reach for `sudo`; install Node with nvm instead and
the global folder moves into your home directory.

**Name clashes** — two files with the same name inside one drop become
`image (1).png`. A file already sitting in the target directory is overwritten,
so a fresh directory per download is the safe habit.

**Where did the files go?** — the directory you ran the command from. Your shell
prompt shows it: `PS D:\>` means `D:\`.

## Notes

- Uploads 100 MB per file, 20 files per drop.
- Downloads stop at the one hour mark; the stored objects are removed within
  about a day.
- `drop get -h` lists every option.
