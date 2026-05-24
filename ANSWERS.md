# ANSWERS.md

## 1. How to run

1. Download or clone the project folder.
2. Open the folder in Visual Studio Code.
3. Install the “Live Server” extension in VS Code.
4. Right click on `index.html`.
5. Select “Open with Live Server”.

The application will open in the browser automatically.

No additional installation or dependencies are required.

---

## 2. Stack & Design Choices

I chose Vanilla HTML, CSS, and JavaScript because the application requirements were focused on interaction design, persistence, and responsiveness rather than framework-specific features. Using plain JavaScript kept the project lightweight, easy to run, and easier to control without unnecessary complexity.

### Design Decision 1 — Weekly Grid Layout

I used a table-based weekly grid because habit tracking is easier to understand visually when users can scan habits horizontally across days. This layout makes streak patterns immediately noticeable and improves readability compared to a vertical list structure.

### Design Decision 2 — Highlighting Today’s Column

I visually highlighted the current day column using a different background color so users can instantly identify where they are in the current week. Since habit trackers are used daily, reducing the effort needed to locate “today” improves usability significantly.

### Week Start Choice

I chose Monday as the start of the week because it aligns better with productivity-focused planning and common work or study schedules.

### Streak Logic Choice

The streak counter counts consecutive completed days up to today. If today is not completed, the streak stops immediately. I chose this behavior because it gives users a more accurate representation of their active consistency.

---

## 3. Responsive & Accessibility

### Responsive Behavior

On smaller screens such as 360px mobile devices:
- The table becomes horizontally scrollable.
- Cell sizes shrink slightly for better fit.
- Spacing is reduced to preserve readability.

On larger screens such as 1440px laptops:
- The full weekly grid is visible without scrolling.
- Additional spacing improves scanability.
- The layout uses the available width more effectively.

### Accessibility Consideration Implemented

I ensured all interactive elements use real HTML buttons so they remain keyboard accessible by default. I also maintained visible contrast between checked and unchecked states.

### Accessibility Consideration Skipped

I did not implement full screen-reader labels for every checkbox cell due to time limitations. With additional time, I would add descriptive `aria-label` attributes for each habit/day interaction.

---

## 4. AI Usage

I used ChatGPT for:
- Brainstorming the application structure
- Generating the initial streak calculation logic
- Reviewing responsive layout ideas
- Improving wording for the documentation files

One important change I made to the AI-generated output was modifying the grid responsiveness. The initial suggestion used fixed-width cells, which caused layout issues on smaller screens. I changed the layout to use horizontal scrolling and smaller mobile cell sizing so the interface remained usable on narrow devices.

I also simplified parts of the JavaScript logic manually to make the code easier to read and maintain.

---

## 5. Honest Gap

One area that still needs improvement is the visual polish and animations. While the application is fully functional, interactions such as checking habits and navigating between weeks could feel smoother.

With another day of work, I would:
- Add smoother animations and transitions
- Improve the empty state design with illustrations or onboarding guidance
- Add habit reordering through drag-and-drop
- Improve accessibility support with screen-reader-friendly labels
- Refine typography and spacing for a more polished interface