<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>1A2B 猜數字遊戲</title>

<style>
    body {
        font-family: Arial, sans-serif;
        max-width: 600px;
        margin: 40px auto;
        text-align: center;
        background: #f5f5f5;
    }

    h1 {
        color: #333;
    }

    .game-box {
        background: white;
        padding: 20px;
        border-radius: 12px;
        box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }

    input {
        font-size: 20px;
        padding: 8px;
        width: 150px;
        text-align: center;
    }

    button {
        font-size: 16px;
        padding: 8px 16px;
        margin: 5px;
        cursor: pointer;
    }

    #message {
        margin-top: 15px;
        font-weight: bold;
        color: #0066cc;
    }

    table {
        width: 100%;
        margin-top: 20px;
        border-collapse: collapse;
    }

    th, td {
        border: 1px solid #ddd;
        padding: 8px;
    }

    th {
        background: #4CAF50;
        color: white;
    }

    tr:nth-child(even) {
        background: #f2f2f2;
    }
</style>
</head>
<body>

<div class="game-box">
    <h1>🎯 1A2B 猜數字</h1>

    <p>請輸入 4 位不重複數字</p>

    <input type="text" id="guessInput" maxlength="4" placeholder="例如 1234">
    <button onclick="checkGuess()">猜看看</button>
    <button onclick="newGame()">重新開始</button>

    <div id="message"></div>

    <table>
        <thead>
            <tr>
                <th>次數</th>
                <th>猜測數字</th>
                <th>結果</th>
            </tr>
        </thead>
        <tbody id="history"></tbody>
    </table>
</div>

<script>
let answer = "";
let count = 0;

function generateAnswer() {
    let nums = ['0','1','2','3','4','5','6','7','8','9'];
    let result = "";

    while(result.length < 4){
        let index = Math.floor(Math.random() * nums.length);
        result += nums[index];
        nums.splice(index,1);
    }

    return result;
}

function newGame() {
    answer = generateAnswer();
    count = 0;

    document.getElementById("history").innerHTML = "";
    document.getElementById("message").innerHTML = "新遊戲開始！";

    console.log("答案：", answer); // 測試用，可刪除
}

function isValidInput(num) {
    if(!/^\d{4}$/.test(num)) return false;

    let set = new Set(num.split(""));
    return set.size === 4;
}

function checkGuess() {
    let guess = document.getElementById("guessInput").value.trim();

    if(!isValidInput(guess)) {
        document.getElementById("message").innerHTML =
            "請輸入 4 位且不重複的數字！";
        return;
    }

    count++;

    let A = 0;
    let B = 0;

    for(let i=0;i<4;i++) {
        if(guess[i] === answer[i]) {
            A++;
        } else if(answer.includes(guess[i])) {
            B++;
        }
    }

    let result = `${A}A${B}B`;

    let row = `
        <tr>
            <td>${count}</td>
            <td>${guess}</td>
            <td>${result}</td>
        </tr>
    `;

    document.getElementById("history").insertAdjacentHTML("beforeend", row);

    if(A === 4) {
        document.getElementById("message").innerHTML =
            `🎉 恭喜答對！共猜了 ${count} 次！`;
    } else {
        document.getElementById("message").innerHTML =
            `結果：${result}`;
    }

    document.getElementById("guessInput").value = "";
    document.getElementById("guessInput").focus();
}

newGame();
</script>

</body>
</html>
