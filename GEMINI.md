# Project Context: Esmeralda Portfolio

## Project Overview
This project is a personal portfolio website for an artist named Esmeralda (Esmé Belle). It is a static website built with HTML, CSS, and Vanilla JavaScript, showcasing various forms of art including drawings, paintings, and 3D art.

**Key Technologies:**
*   **HTML5:** Semantic structure for `index.html` (portfolio feed) and `about.html`.
*   **CSS3:** Custom styling with CSS Variables, Flexbox/Grid, and media queries for responsiveness. Features scroll-triggered animations and a custom cursor.
*   **JavaScript (Vanilla):** Handles dynamic rendering of artworks, filtering logic, carousel functionality, video controls, and scroll observation for updating UI elements (background color, metadata).

## Architecture & Core Features

### Directory Structure
*   **`index.html`**: The main entry point. Contains the structure for the sidebar/mobile navigation and the container for the dynamic art feed.
*   **`about.html`**: A separate page containing the artist's biography and statement.
*   **`assets/`**: Stores all static media.
    *   `images/`: High-res artwork images and decoration elements (cutouts).
    *   `elements/`: UI elements like icons or specific decor graphics.
*   **`package.json`**: Tracks dependencies (currently `colorthief`, though usage should be verified).

### Key Features
1.  **Dynamic Content Rendering:** The `index.html` file contains a JavaScript array `artworks` which holds the data for all portfolio items. The `renderArtworks()` function dynamically builds the DOM based on this data.
2.  **Filtering:** Users can filter artworks by category (Paintings, Drawings, 3D-Art) via the sidebar navigation.
3.  **Responsive Design:**
    *   **Desktop:** Fixed sidebar navigation on the left, scrolling content on the right.
    *   **Mobile:** Hamburger menu with a full-screen overlay navigation. Artworks stack vertically.
4.  **Interactive Elements:**
    *   **Custom Cursor:** A circle following the mouse pointer, expanding on hoverable elements.
    *   **Carousel:** A custom JS class `Carousel` handles swipe/click navigation for artworks with multiple images.
    *   **Scroll Observer:** `IntersectionObserver` detects which artwork is in view to update the floating metadata footer and the background color of the page.
5.  **Decorations:** Parallax-like or fixed "cutout" images (e.g., flowers, mugs) decorate the periphery of the screen.

## Development & Usage

### Running the Project
Since this is a static site, it does not require a complex build process.
1.  **Local Server:** You can serve the project using any static file server.
    *   Python: `python3 -m http.server`
    *   Node (if `serve` is installed): `npx serve .`
    *   VS Code: "Live Server" extension.
2.  **Dependencies:** Run `npm install` to ensure `node_modules` are up to date (though primarily for `colorthief` if used).

### Conventions
*   **Styling:** CSS is embedded directly in `<style>` blocks within the HTML files. When making changes, ensure consistency with the existing CSS variables (e.g., `--bg-pinstripe-1`, `--font-body`).
*   **JavaScript:** Logic is contained within `<script>` tags at the bottom of the HTML files. Keep logic modular (e.g., the `Carousel` class).
*   **Assets:** New artwork images should be placed in `assets/images/` and added to the `artworks` array in `index.html`.
*   **Formatting:** Maintain the existing indentation (4 spaces) and coding style.

## Future Todos / Notes
*   **External CSS/JS:** Consider moving the large `<style>` and `<script>` blocks into separate `.css` and `.js` files (e.g., `style.css`, `app.js`) for better maintainability.
*   **Optimization:** Ensure images are optimized for web to improve load times, as there are many high-res assets.
