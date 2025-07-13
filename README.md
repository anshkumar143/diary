<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Secure Shared Diary</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      margin: auto;
      background-color: #f0f8ff;
      max-width: 800px;
    }
    h1, h2 {
      text-align: center;
    }
    textarea {
      width: 100%;
      height: 120px;
      padding: 10px;
      font-size: 16px;
      box-sizing: border-box;
      margin-top: 10px;
    }
    button {
      width: 100%;
      padding: 12px;
      font-size: 16px;
      margin-top: 10px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 5px;
    }
    button:hover {
      background-color: #0056b3;
    }
    input[type="password"] {
      width: 100%;
      padding: 12px;
      font-size: 16px;
      margin-top: 10px;
      box-sizing: border-box;
    }
    .entry {
      border: 1px solid #ccc;
      padding: 10px;
      margin-top: 15px;
      background-color: #fff;
      border-radius: 5px;
    }
    .timestamp {
      font-size: 12px;
      color: #888;
      margin-bottom: 5px;
    }
    #diarySection {
      display: none;
    }
  </style>
</head>
<body>

  <h1>📓 My Secure Shared Diary</h1>

  <!-- Password prompt -->
  <div id="passwordSection">
    <p>Please enter the diary password:</p>
    <input type="password" id="passwordInput" placeholder="Enter password">
    <button onclick="checkPassword()">Unlock Diary</button>
    <p id="wrongPassword" style="color:red; display:none;">Incorrect password. Try again.</p>
  </div>

  <!-- Diary Section -->
  <div id="diarySection">
    <textarea id="entryText" placeholder="Write your thoughts here..."></textarea>
    <button onclick="saveEntry()">Save Entry</button>

    <h2>🗂️ Previous Entries</h2>
    <div id="entriesContainer"></div>
  </div>

  <script>
    // Your secure password
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
      const text = document.getElementById("entryText").value.trim();
      if (!text) {
        alert("Please write something first!");
        return;
      }

      const entry = {
        content: text,
        timestamp: new Date().toLocaleString()
      };

      let entries = JSON.parse(localStorage.getItem("diaryEntries")) || [];
      entries.unshift(entry);
      localStorage.setItem("diaryEntries", JSON.stringify(entries));

      document.getElementById("entryText").value = "";
      displayEntries();
    }

    function displayEntries() {
      const entriesContainer = document.getElementById("entriesContainer");
      entriesContainer.innerHTML = "";

      const entries = JSON.parse(localStorage.getItem("diaryEntries")) || [];

      entries.forEach(entry => {
        const div = document.createElement("div");
        div.className = "entry";
        div.innerHTML = `<div class="timestamp">${entry.timestamp}</div><p>${entry.content}</p>`;
        entriesContainer.appendChild(div);
      });
    }
  </script>

</body>
</html>
