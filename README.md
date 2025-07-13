<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Our Romantic Chat Diary</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #fff0f5;
      max-width: 700px;
      margin: auto;
      padding: 20px;
    }

    h1, h2 {
      text-align: center;
      color: #d63384;
    }

    #countdown {
      text-align: center;
      font-size: 20px;
      margin-bottom: 20px;
      color: #6f42c1;
    }

    .entry-box {
      margin-top: 10px;
    }

    select, textarea, input[type="password"] {
      width: 100%;
      padding: 10px;
      font-size: 16px;
      margin-top: 10px;
      box-sizing: border-box;
      border-radius: 6px;
      border: 1px solid #ccc;
    }

    button {
      width: 100%;
      padding: 12px;
      font-size: 16px;
      background-color: #ff4da6;
      color: white;
      border: none;
      border-radius: 6px;
      margin-top: 10px;
    }

    button:hover {
      background-color: #e60073;
    }

    .entry {
      border-radius: 8px;
      padding: 10px;
      margin-top: 10px;
      background-color: #ffffff;
      border: 1px solid #ccc;
    }

    .entry.me {
      background-color: #ffe6f0;
    }

    .entry.partner {
      background-color: #e0f7ff;
    }

    .timestamp {
      font-size: 12px;
      color: #666;
      text-align: right;
    }

    #diarySection {
      display: none;
    }

    #wrongPassword {
      color: red;
      display: none;
    }
  </style>
</head>
<body>

  <h1>💞 Our Romantic Chat Diary 💞</h1>

  <div id="countdown">Loading countdown...</div>

  <!-- Password Section -->
  <div id="passwordSection">
    <p>Enter your secret password to unlock the diary:</p>
    <input type="password" id="passwordInput" placeholder="Enter password">
    <button onclick="checkPassword()">Unlock Diary</button>
    <p id="wrongPassword">Incorrect password. Try again.</p>
  </div>

  <!-- Diary Section -->
  <div id="diarySection">
    <div class="entry-box">
      <label>Who is writing?</label>
      <select id="writer">
        <option value="Me">Me</option>
        <option value="Partner">Partner</option>
      </select>
    </div>
    <textarea id="entryText" placeholder="Write your heart out..."></textarea>
    <button onclick="saveEntry()">Save Entry</button>

    <h2>📝 Our Shared Memories</h2>
    <div id="entriesContainer"></div>
  </div>

  <script>
    const DIARY_PASSWORD = "nidhiansh";

    function checkPassword() {
      const enteredPassword = document.getElementById("passwordInput").value;
      if (enteredPassword === DIARY_PASSWORD) {
        document.getElementById("passwordSection").style.display = "none";
        document.getElementById("diarySection").style.display = "block";
        displayEntries();
      } else {
        document.getElementById("wrongPassword").style.display = "block";
      }
    }

    function saveEntry() {
      const writer = document.getElementById("writer").value;
      const text = document.getElementById("entryText").value.trim();
      if (!text) {
        alert("Write something lovely first!");
        return;
      }

      const entry = {
        who: writer,
        content: text,
        timestamp: new Date().toLocaleString()
      };

      let entries = JSON.parse(localStorage.getItem("sharedRomanticDiary")) || [];
      entries.unshift(entry);
      localStorage.setItem("sharedRomanticDiary", JSON.stringify(entries));

      document.getElementById("entryText").value = "";
      displayEntries();
    }

    function displayEntries() {
      const container = document.getElementById("entriesContainer");
      container.innerHTML = "";

      const entries = JSON.parse(localStorage.getItem("sharedRomanticDiary")) || [];

      entries.forEach(entry => {
        const div = document.createElement("div");
        div.className = "entry " + (entry.who === "Me" ? "me" : "partner");
        div.innerHTML = `
          <strong>${entry.who}:</strong>
          <p>${entry.content}</p>
          <div class="timestamp">${entry.timestamp}</div>
        `;
        container.appendChild(div);
      });
    }

    // Countdown Timer
    const targetDate = new Date("2025-05-12T00:00:00").getTime();

    function updateCountdown() {
      const now = new Date().getTime();
      const distance = targetDate - now;

      if (distance < 0) {
        document.getElementById("countdown").innerHTML = "💘 The special day has arrived! 💘";
        return;
      }

      const days = Math.floor(distance / (1000 * 60 * 60 * 24));
      const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
      const seconds = Math.floor((distance % (1000 * 60)) / 1000);

      document.getElementById("countdown").innerHTML =
        `💫 Countdown to 12 May 2025: ${days}d ${hours}h ${minutes}m ${seconds}s 💫`;
    }

    setInterval(updateCountdown, 1000);
  </script>

</body>
</html>
