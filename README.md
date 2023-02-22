# lnwTrivia - Quiz App with Vue.js

### Preview: [lnwtrivia-kbpjj.vercel.app](https://lnwtrivia-kbpjj.vercel.app/)

A simple and interactive quiz app built with Vue.js. The app allows users to take quizzes on various topics and get instant results with scores.

[![Tech stack](https://skillicons.dev/icons?i=vue,tailwind)](https://skillicons.dev)

![image](https://user-images.githubusercontent.com/88102079/220551177-30811674-b99a-412e-8c7b-fb4fdb2045a9.png)


## Features

- A user-friendly interface with a smooth and modern design.
- Instant scoring and result display after each quiz.
- Randomization of questions for each user to ensure a unique quiz experience.
- Multiple categories and difficulties of question.
- Theme song and sound effects for quiz completion and correct/incorrect answers.
- Dark mode feature for improved user experience in low-light environments.

## Getting started

### 1. Select category and difficulty

เลือก category และ difficulty ที่ต้องการ


<img  width="70%" alt="select-category-htp" src="https://user-images.githubusercontent.com/88102079/220555203-913a1ec5-a163-451a-b359-85b8f3da81d0.png" />

<img  width="70%" alt="select-difficulty-htp" src="https://user-images.githubusercontent.com/88102079/220555221-46e1d3d3-f8df-4ce3-bf46-9033de0e3ef4.png" />

### 2. Click Let's play button to start playing

let’s play เพื่อเริ่มเล่น หากยังไม่เลือกทั้ง category และ difficulty ปุ่มจะยังไม่แสดง

<img  width="70%" alt="letsplay-btn-htp" src="https://user-images.githubusercontent.com/88102079/220555327-bde5755e-3b1a-4895-aec8-7f6455db50fc.png" />

### 3. In the game, you will see

- ปุ่ม back เมื่อกดจะกลับไปสู้หน้า home

- Score แสดงคะแนนที่ทำได้ระหว่าง quiz แบบ real time
 
-  ปุ่มเพลงสำหรับเปิดหรือปิดเสียงเพลงประกอบ

- ปุ่มเปลี่ยนเป็น dark mode หรือ light mode

<img width="70%" alt="controll-bar-htp" src="https://user-images.githubusercontent.com/88102079/220555566-380ab8ad-45e9-426b-9b62-3b0e2b0b4011.png" />


- กล่อง question แสดงเลขข้อที่กำลังเล่นอยู่ และคำถาม

- ปุ่มคำตอบ มีทั้งหมด 4 ตัวเลือก เมื่อกดตอบคำถาม ปุ่มที่เป็นคำตอบที่ถูกต้อง จะเปลี่ยนเป็นสีเขียว จากนั้นจะเปลี่ยนคำถามต่อไป

- ถ้าผู้เล่น ตอบคำถามถูกต้อง score จะเพิ่มขึ้นครั้งละ 1 คะแนน

- ปุ่ม restart สำหรับเริ่มใหม่ในหมวดหมู่เดิม เมื่อกด จะทำการสุ่มคำถามใหม่ และเริ่มนับ score ใหม่

<img  width="70%" alt="quiz-body-htp" src="https://user-images.githubusercontent.com/88102079/220555429-e57b80b6-6197-4de4-b7b1-4223f3f7ea66.png" />

### 4. After finish the quiz, you will see

- หน้าต่างผลลัพธ์ จะแสดงหมวดหมู่ และความยากที่ผู้ใช้เลือก และคะแนนที่ทำได้จากทั้งหมด 10 ข้อ

- ปุ่ม Try other one เมื่อกดจะทำการกลับไปยังหน้าเลือก category และ level อีกครั้ง

- Play again จะเริ่มเล่นใหม่ใน category และ difficulty เดิม

<img  width="70%" alt="result-htp" src="https://user-images.githubusercontent.com/88102079/220555687-c1058593-7eb2-48c9-9b44-27f208717688.png" />

## API Reference

The API question used in this project are provided by [The Trivia API](https://the-trivia-api.com/) . Special thanks to the API team for providing the data for this project.

<img width="70%" alt="The Trivia API website homepage" src="https://user-images.githubusercontent.com/88102079/220561456-9ff5390b-4d2d-442d-ad46-4b77539a26ee.png" />

## Members

- [@sealbb](https://www.github.com/sealbb) 64130500013 ชลธิชา หลี่
- [@lostjerome](https://www.github.com/lostjerome) 64130500017 ณรงค์รัชช์ ธนจารุสวัสดิ์
- [@bamxxls](https://www.github.com/bamxxls) 64130500026 ทักษพร เลไชยสงค์
- [@CRYINGV](https://www.github.com/CRYINGV) 64130500030 ธนภัทร จุลมีวงษ์
- [@darKKOREO](https://www.github.com/darKKOREO) 64130500031 ธนภัทร์ ดาวลอย



