<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pomodoro Timer</title>
  <style>
    body {
      font-family: 'Courier New', Courier, monospace;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      height: 100vh;
      background-color: #ffe4e1; /* Soft pink background */
      color: #b56576; /* Soft plum color for text */
    }
    #timer {
      font-size: 60px;
      font-weight: bold;
      color: #c08585; /* Soft pink-red */
      border: 3px solid #f4cccc; /* Light pink border */
      padding: 20px;
      border-radius: 15px;
      background-color: #fff0f0; /* Pale pink background for timer */
    }
    #status {
      font-size: 28px;
      margin-top: 10px;
      font-style: italic;
      color: #b56576;
    }
    #buttons {
      margin-top: 20px;
    }
    button {
      padding: 10px 20px;
      font-size: 18px;
      margin: 5px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      background-color: #f8d7da; /* Light blush pink */
      color: #b56576; /* Soft plum color for text */
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #f1c6c7; /* Darker blush pink on hover */
    }
    button:disabled {
      background-color: #fde5e6; /* Light pink when disabled */
      cursor: not-allowed;
    }
  </style>
</head>
<body>

  <div>
    <div id="timer">25:00</div>
    <div id="status">Work Time</div>
    <div id="buttons">
      <button id="startBtn">Start</button>
      <button id="pauseBtn" disabled>Pause</button>
    </div>
  </div>

  <script>
    let workTime = 25 * 60; // 25 minutes in seconds
    let breakTime = 5 * 60;  // 5 minutes in seconds
    let isWork = true;       // Start with work session
    let time = workTime;     // Start with work time
    let countdown;
    let timerElement = document.getElementById('timer');
    let statusElement = document.getElementById('status');
    let startBtn = document.getElementById('startBtn');
    let pauseBtn = document.getElementById('pauseBtn');
    let isPaused = false;    // Track if the timer is paused

    function updateTimerDisplay() {
      let minutes = Math.floor(time / 60);
      let seconds = time % 60;

      seconds = seconds < 10 ? '0' + seconds : seconds;
      minutes = minutes < 10 ? '0' + minutes : minutes;

      timerElement.textContent = minutes + ":" + seconds;
    }

    function startPomodoroTimer() {
      countdown = setInterval(function() {
        if (!isPaused && time > 0) {
          time--;
          updateTimerDisplay();
        } else if (time === 0) {
          // Switch between work and break sessions
          isWork = !isWork;
          if (isWork) {
            statusElement.textContent = "Work Time";
            time = workTime;  // Reset to work time
          } else {
            statusElement.textContent = "Break Time";
            time = breakTime; // Reset to break time
          }
          updateTimerDisplay();
        }
      }, 1000);
    }

    // Start button functionality
    startBtn.addEventListener('click', function() {
      if (!countdown) {
        startPomodoroTimer();
        startBtn.disabled = true;  // Disable start button once the timer starts
        pauseBtn.disabled = false; // Enable pause button
      }
    });

    // Pause button functionality
    pauseBtn.addEventListener('click', function() {
      isPaused = !isPaused; // Toggle pause state
      pauseBtn.textContent = isPaused ? "Resume" : "Pause";
    });

    // Update the initial timer display
    updateTimerDisplay();
  </script>

</body>
</html>
