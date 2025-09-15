# Day 15 – Import On Interaction / Visibility (Lazy Loading)

## 📄 Summary
Lazy loading with Import On Interaction or Import On Visibility delays loading JavaScript modules until the user either **interacts** with a feature (e.g., clicking a button) or the feature becomes **visible** in the viewport. This reduces the amount of code loaded upfront, improving initial load performance.

## 💡 Problem It Solves
Large bundles increase load time, parse time, and block the main thread. Many features are not used immediately—or at all—by every user. By deferring the loading of these modules, we make apps faster and lighter for everyone.

## 🌎 Real-world Mapping
- **YouTube embeds** often load only when a user scrolls them into view.  
- **Modals or chat widgets** typically load their heavy code only when a user clicks the button to open them.  

This saves bandwidth and improves *time-to-interactive* for users who never engage with those features.

## 🛠 My Mini Implementation
```javascript
// Import On Interaction Example
const button = document.getElementById('open-modal');

button.addEventListener('click', async () => {
  const modalModule = await import('./modal.js');
  modalModule.openModal();
});

// Import On Visibility Example
const carousel = document.getElementById('image-carousel');

const observer = new IntersectionObserver(async (entries) => {
  if (entries[0].isIntersecting) {
    const carouselModule = await import('./carousel.js');
    carouselModule.initCarousel();
    observer.disconnect();
  }
});

observer.observe(carousel);
```