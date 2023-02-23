# lnwTrivia - Quiz App with Vue.js

### Preview: [lnwtrivia-kbpjj.vercel.app](https://lnwtrivia-kbpjj.vercel.app/)

A simple and interactive quiz app built with Vue.js. The app allows users to take quizzes on various topics and get instant results with scores.

[![Tech stack](https://skillicons.dev/icons?i=vue,tailwind,vercel)](https://skillicons.dev)

![image](https://user-images.githubusercontent.com/88102079/220551177-30811674-b99a-412e-8c7b-fb4fdb2045a9.png)


## Features

- A user-friendly interface with a responsive design.
- Instant scoring and result display after each quiz.
- Randomization of questions for each user to ensure a unique quiz experience.
- Various question categories and difficulties.
- Theme song and sound effects for quiz completion and correct/incorrect answers.
- Dark mode feature for improved user experience in low-light environments.

## Getting started

### 1. Select category and difficulty

Choose the category and difficulty you want to play.

<img  width="70%" alt="select-category-htp" src="https://user-images.githubusercontent.com/88102079/220555203-913a1ec5-a163-451a-b359-85b8f3da81d0.png" />

<img  width="70%" alt="select-difficulty-htp" src="https://user-images.githubusercontent.com/88102079/220555221-46e1d3d3-f8df-4ce3-bf46-9033de0e3ef4.png" />

### 2. Click Let's play button to start playing

If both category and difficulty are not selected, the button will not be displayed.

<img  width="70%" alt="letsplay-btn-htp" src="https://user-images.githubusercontent.com/88102079/220555327-bde5755e-3b1a-4895-aec8-7f6455db50fc.png" />

### 3. In the game, you will see

- **Back**: Return to home page

- **Score**: Shows scores scored during quiz in real time.
 
- **Song button**: Music button to switch background music on or off.

- **Dark mode button**: Switch to dark mode or light mode.

<img width="70%" alt="controll-bar-htp" src="https://user-images.githubusercontent.com/88102079/220555566-380ab8ad-45e9-426b-9b62-3b0e2b0b4011.png" />

- The question box shows the current verse number and the question.

- The answer button has a total of 4 options. When pressed, button with the correct answer will turn green. Then the next question will replace.

- If the player answers the question correctly, the score will increase by 1 point each time.

- **Restart button**: Starting over in the same category and difficulty, and recount the score.

<img  width="70%" alt="quiz-body-htp" src="https://user-images.githubusercontent.com/88102079/220555429-e57b80b6-6197-4de4-b7b1-4223f3f7ea66.png" />

### 4. After finish the quiz, you will see

- Result window will displays your category and difficulty selected by the user and scores obtained from all 10 questions

- **Try other one**: Return to the category and difficulty selection page again.

- **Play again**: Start playing again in the same category and difficulty. 

<img  width="70%" alt="result-htp" src="https://user-images.githubusercontent.com/88102079/220555687-c1058593-7eb2-48c9-9b44-27f208717688.png" />

## API Reference

The API question used in this project are provided by [The Trivia API](https://the-trivia-api.com/). Special thanks to the API team for providing the data for this project.

<img width="70%" alt="The Trivia API website homepage" src="https://user-images.githubusercontent.com/88102079/220561456-9ff5390b-4d2d-442d-ad46-4b77539a26ee.png" />

## Members

- [@sealbb](https://www.github.com/sealbb) 64130500013 Chonticha Li *(20%)*
> รับผิดชอบในส่วนของการทำ UI และ function ที่อยู่ในหน้าแรกของ Web application ทั้งหมด <br/>และรับผิดชอบในส่วนของ function หลักที่เกี่ยวกับการแสดงผลของคำตอบ <br/>และจัดทำเนื้อหาและวางโครงใน how to play ร่วมกับ *@darKKOREO*
- [@lostjerome](https://www.github.com/lostjerome) 64130500017 Naronkrach Tanajarusawas *(20%)*
> เป็นที่ปรึกษาหลักของเพื่อนในกลุ่ม และคอยรวม code จากเพื่อนคนอื่นและปรับปรุงแก้ไข
- [@bamxxls](https://www.github.com/bamxxls) 64130500026 Thaksaporn Lachaisong *(20%)*
> รับผิดชอบในส่วนของการทำ UI และ function ที่อยู่ในหน้าของการเลือก category และ ระดับความยากของคำถาม <br/>รวมไปถึง function ของการเล่นเพลงและเอฟเฟคเสียงต่าง ๆ
- [@CRYINGV](https://www.github.com/CRYINGV) 64130500030 Thanaphat Julmeewong *(20%)*
> รับผิดชอบในส่วนของ UI ที่อยู่ในหน้า quiz หรือหน้าหลังจากกดเริ่มเกมร่วมกับ *@darKKOREO* <br/>และรับผิดชอบในส่วนของ Dark mode ทั้งหมด รับผิดชอบ function ที่เกี่ยวกับ navigation bar และ ในส่วนการแสดงผลของคำตอบ
- [@darKKOREO](https://www.github.com/darKKOREO) 64130500031 Tanapat Daoloy *(20%)*
> รับผิดชอบในส่วนของ UI ที่อยู่ในหน้า quiz หรือหน้าหลังจากกดเริ่มเกมร่วมกับ *@CRYINGV* <br/>และรับผิดชอบในส่วนการแสดงข้อคำถามและคำตอบ และทำ popup reult <br/>และจัดทำเนื้อหาและวางโครงใน how to play ร่วมกับ *@sealbb*



