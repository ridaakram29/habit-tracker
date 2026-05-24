const habitInput = document.getElementById("habitName");
const addHabitBtn = document.getElementById("addHabitBtn");

const habitTable = document.getElementById("habitTable");

const emptyState = document.getElementById("emptyState");

const prevWeekBtn = document.getElementById("prevWeek");
const nextWeekBtn = document.getElementById("nextWeek");
const currentWeekBtn = document.getElementById("currentWeek");

let habits =
  JSON.parse(localStorage.getItem("habits")) || [];

let logs =
  JSON.parse(localStorage.getItem("logs")) || {};

let weekOffset = 0;

function saveData() {

  localStorage.setItem(
    "habits",
    JSON.stringify(habits)
  );

  localStorage.setItem(
    "logs",
    JSON.stringify(logs)
  );

}

function getWeekDates(offset = 0) {

  const today = new Date();

  const currentDay = today.getDay();

  const monday = new Date(today);

  monday.setDate(
    today.getDate() - currentDay + 1 + offset * 7
  );

  const week = [];

  for (let i = 0; i < 7; i++) {

    const date = new Date(monday);

    date.setDate(monday.getDate() + i);

    week.push(date);

  }

  return week;
}

function formatDate(date) {

  return date.toISOString().split("T")[0];

}

function calculateStreak(habitId) {

  let streak = 0;

  let date = new Date();

  while (true) {

    const key = formatDate(date);

    if (logs[key] && logs[key][habitId]) {

      streak++;

      date.setDate(date.getDate() - 1);

    } else {

      break;

    }

  }

  return streak;
}

function toggleHabit(habitId, dateKey) {

  if (!logs[dateKey]) {

    logs[dateKey] = {};

  }

  logs[dateKey][habitId] =
    !logs[dateKey][habitId];

  saveData();

  render();
}

function addHabit() {

  const name = habitInput.value.trim();

  if (!name) return;

  habits.push({
    id: Date.now(),
    name
  });

  habitInput.value = "";

  saveData();

  render();
}

function deleteHabit(id) {

  habits =
    habits.filter(habit => habit.id !== id);

  saveData();

  render();
}

function renameHabit(id) {

  const updated =
    prompt("Rename habit");

  if (!updated) return;

  habits = habits.map(habit => {

    if (habit.id === id) {

      return {
        ...habit,
        name: updated
      };

    }

    return habit;

  });

  saveData();

  render();
}

function render() {

  if (habits.length === 0) {

    emptyState.style.display = "block";

  } else {

    emptyState.style.display = "none";

  }

  const weekDates =
    getWeekDates(weekOffset);

  const todayKey =
    formatDate(new Date());

  habitTable.innerHTML = "";

  const headerRow =
    document.createElement("tr");

  headerRow.innerHTML =
    `<th>Habit</th>`;

  weekDates.forEach(date => {

    const th =
      document.createElement("th");

    const key =
      formatDate(date);

    th.textContent =
      date.toLocaleDateString(
        "en-US",
        { weekday: "short" }
      );

    if (key === todayKey) {

      th.classList.add("today");

    }

    headerRow.appendChild(th);

  });

  habitTable.appendChild(headerRow);

  habits.forEach(habit => {

    const row =
      document.createElement("tr");

    const streak =
      calculateStreak(habit.id);

    const habitCell =
      document.createElement("td");

    habitCell.innerHTML = `
      <div class="habit-name">

        <div>
          ${habit.name}<br>
          🔥 ${streak}
        </div>

        <div class="actions">

          <button onclick="renameHabit(${habit.id})">
            ✏️
          </button>

          <button onclick="deleteHabit(${habit.id})">
            🗑️
          </button>

        </div>

      </div>
    `;

    row.appendChild(habitCell);

    weekDates.forEach(date => {

      const key =
        formatDate(date);

      const td =
        document.createElement("td");

      if (key === todayKey) {

        td.classList.add("today");

      }

      const checked =
        logs[key] &&
        logs[key][habit.id];

      td.innerHTML = `
        <button
          class="check-btn ${checked ? "checked" : ""}"
          onclick="toggleHabit(${habit.id}, '${key}')"
        >
          ${checked ? "✓" : ""}
        </button>
      `;

      row.appendChild(td);

    });

    habitTable.appendChild(row);

  });

}

addHabitBtn.addEventListener(
  "click",
  addHabit
);

prevWeekBtn.addEventListener(
  "click",
  () => {

    weekOffset--;

    render();

  }
);

nextWeekBtn.addEventListener(
  "click",
  () => {

    weekOffset++;

    render();

  }
);

currentWeekBtn.addEventListener(
  "click",
  () => {

    weekOffset = 0;

    render();

  }
);

render();