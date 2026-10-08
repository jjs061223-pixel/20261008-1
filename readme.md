---
title: 選擇題測驗卷網站講義（學生版）.md

---

---
title: 選擇題測驗卷網站講義（學生版）

---

---
title: 選擇題測驗卷網站講義（學生版）
tags: [114程式設計與實習_上學期]

---

# 選擇題測驗卷網站講義（學生版）

學號：415730703　　姓名：徐睿妤

> **填寫方式**
> 1. 每個學習都要放：**執行截圖**、**三次問 AI 的提示詞**、**最後採用的程式碼**。
> 2. 問 AI 的提示詞請**逐字貼上**自己實際輸入的內容（不要寫摘要），第一次、第二次、第三次依序記錄。
> 3. 程式碼貼在「點開貼上」的收合區塊裡，貼上**你最後真正採用、而且能執行**的版本。

---

## 學習1：產生一個選擇題測驗卷網站

https://cfchen58.synology.me/115/week4/stage1/

**這個階段的目標：** 用 p5.js 做出一個一次顯示一題、四個選項、答完會顯示對錯與總分的測驗網站（題目先寫在程式裡）。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習1截圖](請貼上截圖)
![螢幕擷取畫面 2026-10-08 144517](https://hackmd.io/_uploads/SkiP22Vsfl.png)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
``使用p5.js撰寫一個選擇題網頁測驗系統，我已經產生一個p5.js專案，請把程式碼寫道sketch.js檔案內，每條指令都需要加上中文註解。測驗系統題目設定為五題，測驗題目內容為程式設計p5.js簡易指令練習測驗，系統採用全螢幕畫布，使用者答錯時，系統會正確答案選項上，加上EBF2FA背景顏色，該選項要上下跳動，答錯的選項採用D00000背景顏色，選項左右移動。選擇題總共有四個選項，當五題結束後，需要顯示答對的題數，每次顯示一個題目，需要有下一個題目的按鈕`

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習1的程式碼
```javascript=
//學習1程式碼所在
// 定義 5 題 p5.js 基礎測驗題目與選項
let questions = [
  {
    question: "1. 在 p5.js 中，用來設定畫布大小的指令是什麼？",
    options: ["A. setup()", "B. createCanvas()", "C. background()", "D. resizeCanvas()"],
    answer: 1 // 正確答案索引：1 (對應 B)
  },
  {
    question: "2. 下列哪一個函式會在 p5.js 中不斷重複執行？",
    options: ["A. setup()", "B. preload()", "C. draw()", "D. mouseClicked()"],
    answer: 2 // 正確答案索引：2 (對應 C)
  },
  {
    question: "3. 若要設定圖形內部的填滿顏色，應該使用哪一個指令？",
    options: ["A. fill()", "B. stroke()", "C. color()", "D. background()"],
    answer: 0 // 正確答案索引：0 (對應 A)
  },
  {
    question: "4. 下列哪一個指令可以在畫布上繪製一個圓形？",
    options: ["A. rect()", "B. line()", "C. circle()", "D. triangle()"],
    answer: 2 // 正確答案索引：2 (對應 C)
  },
  {
    question: "5. 若要設定畫布背景顏色，應該使用哪一個指令？",
    options: ["A. clear()", "B. fill()", "C. stroke()", "D. background()"],
    answer: 3 // 正確答案索引：3 (對應 D)
  }
];

// 系統全域變數設定
let currentQuestion = 0; // 目前進行到的題目編號（從 0 開始）
let score = 0; // 記錄答對的總題數
let gameState = "QUIZ"; // 遊戲狀態：QUIZ（測驗中）、END（測驗結束）
let selectedOption = -1; // 使用者點選的選項索引，預設 -1 表示未選擇
let hasAnswered = false; // 是否已經回答目前題目
let animTime = 0; // 動畫計數器，用於計算跳動與搖晃角度

// p5.js 初始化設定函式
function setup() {
  createCanvas(windowWidth, windowHeight); // 建立與全螢幕同寬同高的畫布
  textAlign(CENTER, CENTER); // 設定文字對齊方式為水平與垂直置中
}

// 視窗大小改變時自動觸發的函式
function windowResized() {
  resizeCanvas(windowWidth, windowHeight); // 動態調整畫布大小以適應視窗
}

// p5.js 主重複繪製函式
function draw() {
  background(245, 247, 250); // 清除背景並設定輕柔的淡灰色背景
  animTime += 0.1; // 每一幀更新動畫計數器時間

  // 根據當前遊戲狀態繪製不同畫面
  if (gameState === "QUIZ") {
    drawQuizScreen(); // 繪製測驗畫面
  } else if (gameState === "END") {
    drawEndScreen(); // 繪製結算畫面
  }
}

// 繪製測驗畫面函式
function drawQuizScreen() {
  let q = questions[currentQuestion]; // 取得當前題目的物件資料

  // 繪製頂部題號標題
  fill(100); // 設定文字顏色為灰色
  textSize(18); // 設定字體大小
  text(`題目 ${currentQuestion + 1} / ${questions.length}`, width / 2, height * 0.12); // 顯示當前題號進度

  // 繪製題目內文
  fill(30); // 設定深灰色文字
  textSize(22); // 設定較大字體
  text(q.question, width / 2, height * 0.2); // 顯示題目文字

  // 定義選項按鈕尺寸與垂直佈局參數
  let btnWidth = min(width * 0.8, 500); // 選項按鈕寬度，最大不超過 500px
  let btnHeight = 50; // 選項按鈕高度
  let startY = height * 0.32; // 第一個選項的初始 Y 座標
  let spacing = 18; // 選項之間的間距

  // 迴圈繪製 4 個選項按鈕
  for (let i = 0; i < q.options.length; i++) {
    let x = width / 2; // 按鈕中心 X 座標預設為畫面中央
    let y = startY + i * (btnHeight + spacing); // 計算每個選項按鈕的中心 Y 座標
    let bgColor = color(255); // 預設選項背景顏色為白色

    // 判斷是否已作答，並設定動態顏色與位移效果
    if (hasAnswered) {
      if (selectedOption === q.answer) {
        // --- 答對時的處理 ---
        if (i === q.answer) {
          bgColor = color('#d4edda'); // 答對的選項顯示淡綠色背景
        }
      } else {
        // --- 答錯時的處理 ---
        if (i === selectedOption) {
          bgColor = color('#cbeef3'); // 答錯點選的選項背景設為 #cbeef3
          x += sin(animTime * 10) * 10; // 讓答錯的選項左右搖晃位移
        } else if (i === q.answer) {
          bgColor = color('#f49cbb'); // 正確答案選項背景設為 #f49cbb
          y += sin(animTime * 10) * 8; // 讓正確答案選項上下跳動位移
        }
      }
    } else {
      // --- 未作答時的 Hover 懸停效果 ---
      if (isMouseOver(x, y, btnWidth, btnHeight)) {
        bgColor = color(230, 240, 255); // 滑鼠懸停時顯示淡藍色
      }
    }

    // 繪製選項外框與背景矩形
    push(); // 儲存當前繪圖設定
    translate(x, y); // 平移原點至選項中心點（方便處理動態位移）
    rectMode(CENTER); // 設定矩形繪製模式為中心點對齊
    stroke(200); // 設定矩形邊框顏色
    strokeWeight(1.5); // 設定邊框粗細
    fill(bgColor); // 套用計算後的背景顏色
    rect(0, 0, btnWidth, btnHeight, 10); // 繪製圓角矩形

    // 繪製選項文字
    noStroke(); // 禁用文字邊框
    fill(40); // 設定文字顏色為深灰色
    textSize(18); // 設定選項字體大小
    text(q.options[i], 0, 0); // 在選項框中央繪製選項文字
    pop(); // 還原之前的繪圖設定
  }

  // 若已回答該題，則顯示「下一題」按鈕
  if (hasAnswered) {
    drawNextButton(); // 呼叫繪製下一題按鈕函式
  }
}

// 繪製下一題按鈕函式
function drawNextButton() {
  let btnX = width / 2; // 按鈕 X 座標
  let btnY = height * 0.82; // 按鈕 Y 座標
  let btnWidth = 160; // 按鈕寬度
  let btnHeight = 45; // 按鈕高度

  push(); // 儲存當前繪圖設定
  rectMode(CENTER); // 設定矩形中心對齊

  // 判斷滑鼠是否懸停在下一題按鈕上
  if (isMouseOver(btnX, btnY, btnWidth, btnHeight)) {
    fill(52, 120, 246); // 滑鼠懸停時顯示較深藍色
  } else {
    fill(66, 133, 244); // 預設藍色背景
  }

  noStroke(); // 禁用邊框
  rect(btnX, btnY, btnWidth, btnHeight, 8); // 繪製圓角矩形按鈕

  fill(255); // 設定文字顏色為白色
  textSize(18); // 設定字體大小
  // 根據是否為最後一題改變按鈕文字內容
  let btnText = (currentQuestion < questions.length - 1) ? "下一題" : "看結果";
  text(btnText, btnX, btnY); // 繪製按鈕文字
  pop(); // 還原繪圖設定
}

// 繪製測驗結果結算畫面函式
function drawEndScreen() {
  fill(40); // 設定文字顏色
  textSize(32); // 設定標題字體大小
  text("測驗完成！", width / 2, height * 0.3); // 顯示完成文字

  textSize(24); // 設定得分字體大小
  text(`您的總得分：答對 ${score} / ${questions.length} 題`, width / 2, height * 0.42); // 顯示最終統計答對題數

  // 繪製「再測驗一次」按鈕
  let btnX = width / 2; // 按鈕中心 X 座標
  let btnY = height * 0.58; // 按鈕中心 Y 座標
  let btnW = 180; // 按鈕寬度
  let btnH = 50; // 按鈕高度

  push(); // 儲存當前繪圖設定
  rectMode(CENTER); // 設定矩形中心對齊
  if (isMouseOver(btnX, btnY, btnW, btnH)) {
    fill(40, 167, 69); // 滑鼠懸停時顯示深綠色
  } else {
    fill(76, 175, 80); // 預設綠色背景
  }
  noStroke(); // 禁用邊框
  rect(btnX, btnY, btnW, btnH, 8); // 繪製按鈕背景矩形

  fill(255); // 設定文字為白色
  textSize(18); // 設定字體大小
  text("再試一次", btnX, btnY); // 繪製按鈕文字
  pop(); // 還原繪圖設定
}

// 滑鼠點擊觸發事件函式
function mousePressed() {
  if (gameState === "QUIZ") {
    let q = questions[currentQuestion]; // 取得當前題目
    let btnWidth = min(width * 0.8, 500); // 選項寬度
    let btnHeight = 50; // 選項高度
    let startY = height * 0.32; // 第一個選項 Y 座標
    let spacing = 18; // 間距

    // 若尚未回答，檢查使用者是否點選了某個選項
    if (!hasAnswered) {
      for (let i = 0; i < q.options.length; i++) {
        let y = startY + i * (btnHeight + spacing); // 計算第 i 個選項的 Y 座標
        if (isMouseOver(width / 2, y, btnWidth, btnHeight)) { // 判斷點擊位置
          selectedOption = i; // 紀錄使用者選取的選項索引
          hasAnswered = true; // 設定為已回答狀態

          if (selectedOption === q.answer) {
            score++; // 如果選擇正確，答對題數加 1
          }
          break; // 結束選項判斷迴圈
        }
      }
    } else {
      // 若已回答，檢查是否點擊「下一題」按鈕
      let nextBtnX = width / 2; // 下一題按鈕 X 座標
      let nextBtnY = height * 0.82; // 下一題按鈕 Y 座標
      if (isMouseOver(nextBtnX, nextBtnY, 160, 45)) {
        currentQuestion++; // 進到下一題
        hasAnswered = false; // 重設回答狀態為未回答
        selectedOption = -1; // 重設選項索引

        // 判斷是否已完成所有題目
        if (currentQuestion >= questions.length) {
          gameState = "END"; // 切換為結算畫面狀態
        }
      }
    }
  } else if (gameState === "END") {
    // 結算畫面下，檢查是否點擊「再試一次」按鈕
    if (isMouseOver(width / 2, height * 0.58, 180, 50)) {
      resetQuiz(); // 呼叫重置測驗函式
    }
  }
}

// 判斷滑鼠座標是否落於指定矩形範圍內的輔助函式
function isMouseOver(cx, cy, w, h) {
  return mouseX > cx - w / 2 && mouseX < cx + w / 2 && mouseY > cy - h / 2 && mouseY < cy + h / 2;
}

// 重置測驗資料函式
function resetQuiz() {
  currentQuestion = 0; // 重設題目索引為第一題
  score = 0; // 答對題數歸零
  gameState = "QUIZ"; // 遊戲狀態改回測驗中
  selectedOption = -1; // 清空選取的選項
  hasAnswered = false; // 重設回答狀態
}
```
:::


---

## 學習2：網頁設定為響應式網頁

https://cfchen58.synology.me/115/week4/stage2/

**這個階段的目標：** 讓網站在電腦、平板、手機（直向與橫向）都能正常顯示，視窗大小改變時版面自動調整。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![20261008](https://hackmd.io/_uploads/SkhsCn4ofe.gif)
![學習2截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```
網頁設定為響應式網頁，如何 讓網站在電腦、平板、手機（直向與橫向）都能正常顯示，視窗大小改變時版面自動調整。
### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習2的程式碼
```javascript=
//學習2程式碼所在

```
:::


---

## 學習3：設定嵌入 Google 字型，網頁文字採用這些字型

https://cfchen58.synology.me/115/week4/stage3/

**這個階段的目標：** 從 Google Fonts 嵌入繁體中文字型，並讓畫布上的題目與選項文字使用這些字型。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習3截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習3的程式碼
```javascript=
//學習3程式碼所在

```
:::


---

## 學習4：設定題庫並抽題顯示題目網頁（CSV 檔案）

https://cfchen58.synology.me/115/week4/stage4/

**這個階段的目標：** 把題目移到 questions.csv，網站讀取題庫後每次隨機抽出 5 題。
**這個階段會修改的檔案：** index.html、sketch.js、questions.csv

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習4截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習4的程式碼
```javascript=
//學習4程式碼所在

```
:::


---

## 學習5：利用 Google Sheets 當題庫

https://cfchen58.synology.me/115/week4/stage5/

**這個階段的目標：** 把題庫放在 Google 試算表，網站直接讀取，老師改試算表，網站題目就跟著更新。
**這個階段會修改的檔案：** index.html、sketch.js（questions.csv 當備用題庫）

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習5截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習5的程式碼
```javascript=
//學習5程式碼所在

```
:::


---

## 我的心得

這五個學習中，哪一個最困難？你是怎麼解決的？（請寫出實際發生的事）

＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿
