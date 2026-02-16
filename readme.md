# LAB09-TODO

## ส่วนที่ 1 : การทำความเข้าใจโค้ด

**1.1 เหตุใดจึงต้องแยก duration และ timeLeftออกจากกัน อธิบายความแตกต่าง**

**ตอบ**

- duration คือ เวลาทั้งหมดที่เราตั้งไว้ตั้งแต่แรก
- timeLeft คือ เวลาที่เหลืออยู่จริงขณะนับถอยหลัง
- การแยกกันทำให้รู้เวลาเริ่มต้นของ task และสามารถ reset หรือคำนวณความคืบหน้าได้ง่าย

**1.2 formatTime() ฟังก์ชันนี้ทำความสำคัญอะไร? ถ้าไม่มีฟังก์ชันนี้ app จะเป็นยังไง**

**ตอบ**

- formatTime() ใช้แปลงวินาทีให้เป็นรูปแบบ นาที:วินาที เพื่อให้ผู้ใช้ดูเวลาได้ง่ายถ้าไม่มี ฟังก์ชันนี้ แอปจะแสดงเวลาเป็นตัวเลขวินาทีซึ่งดูยาก

**1.3 อธิบายข้อแตกต่างระหว่าง setInterval() และ setTimeout()**

**ตอบ**

- **setInterval()** จะทำงานซ้ำๆทุกช่วงเวลา ตามเวลาที่กำหนด เช่น ทุก 1 วินาที เหมาะกับการทำ timer หรือนับถอยหลัง
- **setTimeout()** จะทำงานครั้งเดียวหลังจากครบเวลาที่กำหนด ไม่วนซ้ำ เช่น รอ 3 วินาทีแล้วค่อยแสดงข้อความ

## ส่วนที่ 2 : ฟังก์ชันการทำงาน

**2.1เมื่อผู้ใช้คลิกปุ่ม "▶️ เริ่ม" ครั้งแรก เกิดอะไรขึ้นในลำดับใด (ระบุขั้นตอนอย่างน้อย 4 ขั้น)**

**ตอบ**

- 1. เรียกฟังก์ชัน toggleTimer(id)
- 2. เปลี่ยนค่า task.isRunning เป็น true
- 3. สร้าง setInterval() ให้นับเวลาทุก 1 วินาที
- 4. ลดค่า timeLeft ลงทีละ 1
- 5. เรียก renderTasks() เพื่ออัปเดตหน้าจอ

**2.2 ในฟังก์ชัน toggleTimer() ทำไมจึงต้องมี clearInterval() ก่อนแล้วจึงตั้ง task.isRunning = false**

**ตอบ**

- เพื่อหยุดการนับเวลาอย่างสมบูรณ์ถ้าไม่หยุด intervalก่อน timer จะยังทำงานอยู่แม้สถานะจะเป็นหยุดแล้ว

**2.3ถ้า timer หมดเวลา จะเกิดความผิดพลาดใดหากไม่มี clearInterval()**

**ตอบ**

- timer จะยังทำงานต่อ
- timeLeft() อาจจะติดลบ
- เกิด bug และสิ้นเปลืองหน่วยความจำ

## ส่วนที่ 3 : DOM และ UI

**3.1 ฟังก์ชัน renderTasks() สามารถแยกออกเป็นฟังก์ชันย่อยได้กี่ฟังก์ชัน จงเสนอการแยกที่เหมาะสม**

**ตอบ** 3 ฟังก์ชันย่อย ได้แก่

- renderEmptyState()
- renderTaskItem(task)
- renderTaskList(tasks)

**3.2ใช้วิธีใดในการอัปเดต UI ทุกครั้งที่ timer เปลี่ยนแปลง มีวิธีการอื่นนอกเหนือจำนวนปัจจุบันหรือไม่**

**ตอบ**

- ใช้การเรียก renderTasks() ทุกครั้งที่ข้อมูลเปลี่ยน
- วิธีอื่น : อัปเดตเฉพาะ DOM ที่เปลี่ยน

**3.3 "keypress" event listener ทำความสำคัญอะไร หากลบออกไป app จะเป็นยังไง**

**ตอบ**

- ทำให้กด Enter เพื่อเพิ่ม task ระบบจะเรียก addTask() ทันที
  ช่วยให้เพิ่ม task ได้สะดวก โดยไม่ต้องใช้เมาส์
- ถ้าลบออกผู้ใช้จะ กด Enter แล้วไม่เกิดอะไรขึ้น ต้องกดปุ่มเพิ่มเท่านั้น

## ส่วนที่ 4 : Web Audio API

**4.1 อธิบายหน้าที่ของการตั้งค่า:**

- oscillator.frequency.value = 800
- oscillator.type = "sine"
- gainNode.gain.setValueAtTime(0.3, audioContext.currentTime)

**ตอบ**

- frequency = 800 คือ ความแหลมของเสียง
- type = "sine" คือ เสียงนุ่ม ไม่แหลมเกิน
- gain = 0.3 คือ ระดับความดังของเสียง

**4.2ทำไมเสียง alarm จึงใช้เวลา 0.5 วินาที แล้วหยุด ถ้าเพิ่มเป็น 2 วินาทีจะส่งผลต่อ UX อย่างไร**

**ตอบ**

- ไม่รบกวนผู้ใช้ และ แจ้งเตือนพอดี
- ถ้า 2 วินาที อาจดังนานเกิน และ น่ารำคาญ

## ส่วนที่ 5 : Array Methods

**5.1ในฟังก์ชัน deleteTask(id) ใช้ filter() เพื่ออะไร เขียนวิธีอื่นแทน filter() ได้หรือไม่**

**ตอบ**

- เราใช้ filter() เพื่อ ลบ task ที่มี id ตรงกันออกจาก array
- ใช้วิะีอื่นได้ จะใช้ findIndex() + splice()

**5.2ในฟังก์ชัน renderTasks() ใช้ map() เพื่อทำอะไร ถ้าใช้ forEach() แทนจะดีหรือไม่**

**ตอบ**

- map() ใช้เพื่อแปลง atsk แต่ละตัวเป็น HTML
- forEach() ไม่เหมาะ เพราะ ไม่ return ค่า

## ส่วนที่ 6 : ข้อบกพร่องและการแก้ไข

**6.1หากผู้ใช้เพิ่ม task 2 อัน แล้วคลิก "เริ่ม" บน task ทั้ง 2 อันพร้อมๆ กัน จะเกิดอะไรขึ้น**

**ตอบ**

- timer ทั้งสองจะนับพร้อมกัน (ไม่มีข้อผิดพลาด แต่ UX อาจไม่ดี)

**6.2ถ้า timer กำลังเดินอยู่ และผู้ใช้รีเฟรชหน้า (F5) task จะหายไป ทำไม วิธีแก้ปัญหาคือ**

**ตอบ**

- เพราะข้อมูลอยู่ใน memory วิธีแก้คือ **ใช้ LocalStorage**

**6.3ในการทดสอบ app ที่ timer 30 นาที จะใช้เวลานานไปหรือไม่ เสนอวิธีทดสอบที่เร็วขึ้น**

**ตอบ**

- ลดเวลาเป็น 10 - 30 วินาที
- ใช้ค่าทดสอบแทนค่าจริง

## ส่วนที่ 7 : Enhancement

**7.1หากต้องการเพิ่มฟังก์ชัน "รีเซ็ต timer" คืนไปเป็น 30 นาที โค้ดจะเป็นยังไง**

**ตอบ**

- ตั้งค่า task.timeLeft = task.duration แล้วเรียก renderTasks()

**7.2วิธีใดในการเก็บ tasks ไว้ใน LocalStorage เพื่อไม่ให้หายเมื่อรีเฟรชหน้า**

**ตอบ**

- บันทึก tasks ด้วย JSON.stringify()
- โหลดกลับด้วย JSON.parse() ตอนเริ่มแอป

**7.3ถ้าต้องการให้ผู้ใช้กำหนดเวลา timer เอง จะต้องแก้ไขฟังก์ชันไหนบ้าง**

**ตอบ**

- แก้ตรง addTask()
- UI input เวลา
- ค่า duration และ timeLeft

## ส่วนที่ 8 : Concepts เชิงลึก

**8.1 ทำไม renderTasks() จึงต้องเรียกหลายครั้ง มีวิธี DRY ที่ดีกว่าหรือไม่**

**ตอบ**

- เพื่อให้ UI ตรงกับข้อมูลล่าสุด
  แนวคิด DRY : อัปเดตเฉพาะส่วนที่เปลี่ยน

**8.2ถ้าเปลี่ยนจาก setInterval เป็น requestAnimationFrame จะส่งผลดีหรือไม่**

**ตอบ**

- ไม่เหมาะ เพราะ timer ไม่ต้องการเฟรมเรตสูง
  setInterval() เหมาะกว่า

**8.3ในโค้ดที่ใช้ setInterval() มี Memory Leak ที่อาจเกิดขึ้นได้หรือไม่**

**ตอบ**

- มี ถ้าไม่ clearInterval() ตอนลบ task
  interval จะค้างอยู่ในหน่วยความจำ

# LAB09-Pomodoro Timer App

## ส่วนที่ 1 : ความเข้าใจเทคนิค Pomodoro

**1.1เทคนิค Pomodoro คืออะไร เหตุใดจึงเลือกใช้เวลา 25 นาที สำหรับการทำงาน**

**ตอบ**

- Pomodoro คือเทคนิคแบ่งเวลาทำงานเป็นช่วงสั้น ๆ เพื่อเพิ่มสมาธิ25 นาทีเป็นเวลาที่สมองโฟกัสได้ดีโดยไม่ล้าเกินไป

**1.2หากต้องการปรับเวลา Work/Break ให้แตกต่างจาก default (25/5) จะต้องแก้ไขโค้ดส่วนไหน**

**ตอบ**

- workDuration

- breakDuration

- input workTimeInput และ breakTimeInput

**1.3ทำไมหลังจาก 4 รอบ (pomodoros) จึงมี Long Break ขนาด 15 นาที ประโยชน์คืออะไร**

**ตอบ**

- เพื่อให้สมองพักจริง ลดอาการล้าช่วยให้โฟกัสระยะยาวได้ดีขึ้น

## ส่วนที่ 2 : การจัดโครงสร้าง State

**2.1ปัจจุบัน app เก็บ state อะไรบ้าง เช่น: เวลาที่เหลือ ,Phase(work/break),Rounds counter ,...อื่นๆ**

**ตอบ**

- timeLeft → เวลาที่เหลือ
- isWorkPhase → Work / Break
- currentSession → รอบปัจจุบัน
- completedSessions → รอบที่ทำเสร็จ
- isRunning → timer ทำงานหรือไม่
- intervalId → อ้างอิง setInterval

**2.2ทำไมจึงต้องมี rounds state หากแค่เก็บ phase ก็พอ**

**ตอบ**

- เพราะ phase อย่างเดียวไม่รู้ว่าทำไปกี่รอบจำเป็นสำหรับ logic Long Break

**2.3ถ้าต้องการให้ Pomodoro Count อัปเดต เมื่อจบแต่ละ Work phase จะแก้ไขฟังก์ชัน completePhase() ยังไง**

**ตอบ**

## ส่วนที่ 3 : Timer Logic

**3.1อธิบายการทำงานของ setInterval() ในส่วน timer - จะเกิดอะไรขึ้นทุก 1 วินาที**

**ตอบ**

- ทุก 1 วินาที จะลด timeLeft , เช็กว่าเวลาหมดรึยัง , อัปเดต UI

**3.2เหตุใดจึงต้องมี timerInterval เป็น global variable ถ้าไม่มี app จะเป็นยังไง**

**ตอบ**

- เพื่อให้หยุด timer ได้จากหลายฟังก์ชันถ้าไม่เป็น global จะ clearInterval ไม่ได้

**3.3ถ้าผู้ใช้คลิก "เริ่ม" 2 ครั้งติดต่อกัน จะเกิดอะไรขึ้น มีวิธีแก้หรือไม่**

**ตอบ**

- โค้ดเช็ก isRunning จึงไม่สร้าง interval ซ้ำเป็นการป้องกัน bug แล้ว

## ส่วนที่ 4 : DOM & UI

**4.1ฟังก์ชัน updateDisplay() ทำอะไร ต้องเรียกจากที่ไหนบ้าง**

**ตอบ**

- อัปเดต:
  เวลา
  phase
  ปุ่ม
  progress bar
  stats
- ต้องเรียกทุกครั้งที่ state เปลี่ยน

**4.2formatTime() ใช้ padding ด้วย padStart(2, "0") เพื่ออะไร ถ้าไม่มี timer จะแสดง 5:3 แทน 05:03 ได้หรือไม่**

**ตอบ**

- เพื่อให้แสดงเวลาเป็น 05:03 ถ้าไม่ใช้ จะได้ 5:3 ซึ่งดูไม่สวย

**4.3Progress bar คำนวณเปอร์เซ็นต์ยังไง เขียนสูตรออกมา**

**ตอบ**

- progress = (เวลาที่ผ่านไป / เวลาทั้งหมด) × 100

## ส่วนที่ 5 : Events & Controls

**5.1ปุ่มควบคุมมีอะไรบ้าง (Start, Pause, Reset, Settings) แต่ละปุ่มเรียกฟังก์ชันไหน**

**ตอบ**

- start / Pause -> startTimer()
- Reset -> resetTimer()
- Setting -> input change event

**5.2ทำไม Pause ต้องหยุด timer ด้วย clearInterval() หากไม่ทำจะไปไป**

**ตอบ**

- ถ้าไม่ clear timer จะยังเดินอยู่เบื้องหลัง

**5.3Reset button ควรตั้งค่ากลับเป็นอะไร:**

**ตอบ**

- timeLeft
- isWorkPhase
- currentSession
- intervalId

## ส่วนที่ 6 : Notification & Soung

**6.1ฟังก์ชัน playSound() ใช้เทคนิคอะไรในการสร้างเสียง (Web Audio API? HTML Audio?)**

**ตอบ**

- ใช้ Web Audio API (Oscillator)

**6.2ทำไม notification ควร work/break ต่างกันอย่างไร เสียง alarm ที่ดีควรเป็นยังไง**

**ตอบ**

- ช่วยให้ผู้ใช้รู้สถานะโดยไม่ต้องมองจอเสียงที่ดีควรสั้น ไม่รบกวน

**6.3ถ้าต้องการเพิ่ม Desktop Notification ด้วย Browser Notification API จะแก้ไขฟังก์ชัน completePhase() ยังไง**

**ตอบ**

- เรียก new Notification() ตอนจบ phase

## ส่วนที่ 7 : Progress Tracking & Stats

**7.1Stats ต้องติดตามข้อมูลอะไร:**

**ตอบ**

- Pomodoros ที่เสร็จแล้ว
- เวลารวมที่ทำงาน
- Long breaks ที่เกิดขึ้น

**7.2ตำแหน่งที่ดีที่สุดในการอัปเดต stats คือที่ไหน**

**ตอบ**

- ตอนจบ Work phase เท่านั้น

**7.3ธีการบันทึก stats ไว้ใน LocalStorage ได้อย่างไร**

**ตอบ**

- ใช้ JSON.stringify(state) ตอนเปลี่ยนค่าโหลดด้วย JSON.parse() ตอนเริ่มแอป

## ส่วนที่ 8 : Bug Detection

**8.1ถ้า timer กำลังเดินอยู่ แล้วผู้ใช้เปิด DevTools ขนาดใหญ่ timer จะยังคงเดินต่อหรือไม่**

**ตอบ**

- เดินช้าลงได้ เพราะ browser throttle timer

**8.2ถ้า completePhase() เรียก ในขณะที่ timer ยังวิ่งอยู่ จะเกิด memory leak ได้หรือไม่**

**ตอบ**

- เสี่ยง memory leak ถ้าไม่ clearInterval

**8.3น mobile ถ้าผู้ใช้ล็อก screen ขณะ timer เดิน timer จะหยุดหรือยังคงเดิน วิธีแก้คืออะไร**

**ตอบ**

- timer อาจหยุดแก้ด้วย Background API หรือ Service Worker

## ส่วนที่ 9 : Architecture & Code Quality

**9.1ถ้าต้องการแยก Timer logic ออกจาก UI ควรสร้าง class อย่างไร**

**ตอบ**

- สร้าง class PomodoroTimerแยก state กับ UI ออกจากกัน

**9.2Functions ที่มีมากควรจัดไว้ใน object เพื่อจัดการได้ดีขึ้นหรือไม่**

**ตอบ**

- ควร เพื่อให้โค้ดเป็นระบบและดูแลง่าย

**9.3Code reusability - ส่วนไหนของโค้ดสามารถทำให้เป็น utility functions ได้**

**ตอบ**

- formatTime()
- notification
- sound

## ส่วนที่ 10 : Enhancemants

**10.1วิธีเพิ่ม Dark Mode ควรแก้ไข CSS/JS ส่วนไหนบ้าง**

**ตอบ**

- CSS class
- toggle ด้วย JS

**10.2วิธีบันทึกประวัติการใช้งาน (session history) คืออะไร**

**ตอบ**

- เก็บ array ของ session ลง LocalStorage

**10.3วิธีเพิ่ม Keyboard Shortcuts (Space = Start/Pause, R = Reset) ควรใช้ event ไหน**

**ตอบ**

- ใช้ keydown event
  เช่น Space = Start/Pause, R = Reset

# LAB09-weather app

## ส่วนที่ 1 : Fetch API & Requests

**1.1 Fetch API คืออะไร ความแตกต่างระหว่าง Fetch API และ XMLHttpRequest คืออะไร**

**ตอบ**

- Fetch API คือวิธีดึงข้อมูลจาก server แบบสมัยใหม่
- เขียนง่ายกว่า
- ใช้ Promise / async-await XMLHttpRequest โค้ดยาว อ่านยาก และเก่ากว่า

**1.2โค้ดนี้ทำอะไร อธิบายการทำงานแต่ละขั้นตอน**

`fetch(url)
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.error(error));
`
**ตอบ**

- 1. ส่ง request ไปที่ url
- 2. แปลง response เป็น JSON
- 3. ได้รับข้อมูลแล้วนำไปใช้งาน
- 4. ถ้า error ให้จับที่ catch

**1.3ถ้า API response ล้มเหลว (network error) จะถูก handle ที่ไหน .catch() จะจับได้หรือไม่**

**ตอบ**

- .catch() จับได้ ถ้า network error หรือ fetch ล้มเหลว
- แต่ ไม่จับ error จาก HTTP status (404, 500)
  จึงต้องเช็ก response.ok เพิ่ม (ซึ่งโค้ดนี้ทำแล้ว)

## ส่วนที่ 2 : Geocoding API

**2.1Geocoding API ทำหน้าที่อะไร ทำไมจึงต้องใช้ก่อนเรียก Weather API**
[text](https://geocoding-api.open-meteo.com/v1/search?name=Bangkok)

**ตอบ**

- แปลงชื่อเมือง → latitude / longitude
  จำเป็นเพราะ Weather API ใช้พิกัด ไม่ใช้ชื่อเมือง

**2.2When user searches "Bangkok" ควรส่ง request ไปที่ endpoint ไหนก่อน ได้ข้อมูลอะไรจาก response**

**ตอบ**

- 1. ส่ง request ไปที่ Geocoding API ก่อน
- 2. ได้ข้อมูล latitude / longtitude / country
- 3. เอาพิกัดไปเรียก Weather API

**2.3ถ้า user ค้นหาเมือง "Nonthaburi" ซึ่งชื่อเดียวกับเมืองอื่น app จะตัดสินใจอย่างไร**

**ตอบ**

- API จะส่งหลายผลลัพธ์โค้ดนี้เลือก ผลลัพธ์แรก (results[0]) เป็นค่าเริ่มต้น

## ส่วนที่ 3 : Weather Data & Parameters

**3.1Weather API ต้องการ parameters อะไรบ้าง (latitude, longitude, ...อื่นๆ)**

**ตอบ**

- latitude
- longtitude
- current (อุณหภูมิ ,ความชื้น , อื่นๆ)
- hourly (พยาการณ์รายชั่วโมง)

**3.2Parameters นี้ request ข้อมูลอะไร ทำไมต้องใช้ weather_code**
`const params =
  "?latitude=13.7&longitude=100.5&current=temperature_2m,humidity,weather_code";
`
**ตอบ**

- ใช้บอก สภาพอากาศเป็นประเภท
  เช่น แดด ฝน หมอก → ใช้แสดง icon/emoji

**3.3ความแตกต่างระหว่าง current weather และ hourly forecast คืออะไร**

**ตอบ**

- current -> สภาพอากาศ "ตอนนี้"
- hourly -> พยากรณ์ล่วงหน้าเป็นช่วงเวลา

## ส่วนที่ 4 : Async / Await Pattern

**4.1ทำไมจึงใช้ async/await แทน .then().catch() มีข้อดีอะไรบ้าง**

`async function getWeather(city) {
  try {
    const response = await fetch(url);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error(error);
  }
}`

**ตอบ**

- อ่านง่าย
- เขียนเหมือน synchronous
- debug ง่ายกว่า .then()

**4.2await ทำอะไร ถ้าไม่ใช้ code จะเป็นยังไง**

**ตอบ**

- รอให้ Promise เสร็จก่อนค่อยทำบรรทัดถัดไป
  ถ้าไม่ใช้ → ได้ Promise แทนข้อมูลจริง

**4.3try-catch block ใช้เพื่ออะไร ตกลง error ที่ catch ได้อะไร**

**ตอบ**

- จับ error จาก fetch, json parsing, network
  ป้องกัน app crash

## ส่วนที่ 5 : DOM Manipulation

**5.1เมื่อได้ข้อมูล weather มา app ต้องแสดงผล element ใดบ้าง**

**ตอบ**

- ชื่อเมือง
- อุณหภูมิ
- รายละเอียด
- hourly forecast
- recent cities

**5.2Loading spinner ปรากฏในไหน เมื่อไหร่ควรแสดง/ซ่อน**

**ตอบ**

- แสดงตอนเริ่ม fetch
- ซ่อนเมื่อ fetch เสร็จหรือ error

**5.3ถ้าต้องอัปเดต temperature display ทุก 10 นาที ควรใช้ setInterval() หรือ setTimeout()**

**ตอบ**

- ใช้ setInterval()
  เพราะต้องทำซ้ำเป็นช่วงเวลา

## ส่วนที่ 6 : Data Storage (LocalStorage)

**6.1LocalStorage ใช้เพื่ออะไร บันทึก recent cities ยังไง**
`localStorage.setItem("recentCities", JSON.stringify(cities));`

**ตอบ**

- เก็บ recent cities เพื่อไม่ให้หายเมื่อ refresh

**6.2ทำไมต้องใช้ JSON.stringify() ถ้าเก็บ object โดยตรงจะเป็นอย่างไร**

**ตอบ**

- LocalStorage เก็บได้เฉพาะ string
  object ต้องแปลงเป็น JSON ก่อน

**6.3เมื่อ refresh หน้า app ควร restore recent cities จากไหน โค้ดเป็นยังไง**

**ตอบ**

- ดึงจาก `JSON.parse(localStorage.getItem("recentCities"))
`

## ส่วนที่ 7 : Error Handling & Edge Cases

**7.1ถ้า user ค้นหาเมืองที่ไม่มี (เช่น "XYZ") app จะแสดง error อย่างไร**

**ตอบ**

- แสดง error message ซ่อน weather container

**7.2ถ้า network ไม่มี (offline) fetch จะ throw error หรือ return success**

**ตอบ**

- fetch จะ throw error → เข้า catch

**7.3เมื่อ hover หรือ click ที่ recent city button โปรแกรมจะทำอะไร**

**ตอบ**

- เรียก fetchWeather() ด้วยพิกัดที่เก็บไว้

## ส่วนที่ 8 : Weather Display

**8.1 : Weather code 0 = Clear sky, 1 = Partly cloudy, 80 = Drizzle... เลือก icon/emoji ยังไง**

**ตอบ**

- ใช้ weather_code map กับ emoji

**8.2ความแตกต่างระหว่าง อุณหภูมิ (°C), ความชื้น (%), ความเร็วลม (m/s) คืออะไร**

**ตอบ**

- °C → อุณหภูมิ
- % → ความชื้น
- m/s → ความเร็วลม

**8.3Hourly forecast แสดง 24 ชั่วโมง ต้องเลือก element ไหนจาก response**

**ตอบ**

- ใช้ hourly.time และ hourly.temperature_2m

## ส่วนที่ 9 : Responsive Design & Mobile

**9.1ใน mobile screen weather card ควร มี layout อย่างไร (vertical/horizontal)**

**ตอบ**

- ควรเป็นแนวตั้ง (vertical)

**9.2 เมื่อผู้ใช้ rotate device grid/flex ควรตัดสินใจอย่างไร**

**ตอบ**

- ใช้ flex/grid ปรับ layout อัตโนมัติ

**9.3 Input field และ button บน mobile ต้องมี font size เท่าไร (ควรมากพอให้แตะง่าย)**

**ตอบ**

- อย่างน้อย 16px เพื่อแตะง่าย

## ส่วนที่ 10 : Code Architecture & Enhancement

**10.1ถ้าต้องสร้าง Weather class เพื่อจัดการ logic อย่างไร**

`class WeatherApp {
  async searchCity(cityName) {}
  async fetchWeather(lat, lon) {}
  displayWeather(data) {}
}
`
**ตอบ**

- แยก logic ออกจาก UIทำให้ดูแลง่ายและ reusable

**10.2วิธีเพิ่ม Favorite Cities feature ยังไง ต้องแก้ไข HTML/CSS/JS ส่วนไหนบ้าง**

**ตอบ**

- เพิ่มปุ่ม
- เก็บ city ใน LocalStorage
- แสดงใน UI แยกต่างหาก

**10.3วิธีเพิ่ม Dark Mode ด้วย CSS variables ได้อย่างไร**

**ตอบ**

- ใช้ CSS variables toggle class ด้วย Javascript
