<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My Diary</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      max-width: 700px;
      margin: auto;
      background-color: #f0f8ff;
    }
    textarea {
      width: 100%;
      height: 150px;
      padding: 10px;
      font-size: 16px;
    }
    button {
      margin-top: 10px;
      padding: 10px 20px;
      font-size: 16px;
    }
    .entry {
      border: 1px solid #ccc;
      padding: 10px;
      margin-top: 15px;
      background-color: #fff;
    }
    .timestamp {
      font-size: 12px;
      color: #888;
    }
  </style>
</head>
<body>

  <h1>My Diary</h1>
  <textarea id="entryText" placeholder="Write your thoughts here..."></textarea><br>
  <button onclick="saveEntry()">Save Entry</button>

  <h2>Previous Entries</h2>
  <div id="entriesContainer"></div>

  <script>
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

    // Load entries on page load
    window.onload = displayEntries;
  </script>

</body>
</html>
