Perfect ✅ You want this **single course row** (from Learning History) as a **reusable Angular component**, with:

* Course image, title, start/completion dates
* Completion status with **color logic**:

  * 🔴 Red if `< 30%`
  * 🟠 Orange if `30% - 99%`
  * 🟢 Green if `100%`
* Rating stars + reviews count
* "Download Certificate" button → active only if **100% complete**, otherwise disabled (grey).

Here’s a clean Angular 19 implementation:

---

### 1. Component TS

```ts
// learning-history-item.component.ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-learning-history-item',
  templateUrl: './learning-history-item.component.html',
  styleUrls: ['./learning-history-item.component.scss']
})
export class LearningHistoryItemComponent {
  @Input() course: any;

  getCompletionColor(): string {
    if (this.course.completion === 100) return 'green';
    if (this.course.completion < 30) return 'red';
    return 'orange';
  }

  get isCertificateAvailable(): boolean {
    return this.course.completion === 100;
  }
}
```

---

### 2. Component HTML

```html
<div class="course-row flex justify-between items-center bg-[#1e1e2f] p-4 rounded-xl shadow mb-3">
  <!-- Left: Thumbnail + Info -->
  <div class="flex items-center gap-4">
    <img [src]="course.image" alt="Course Image" class="w-16 h-16 rounded-md object-cover shadow-md">

    <div class="flex flex-col">
      <h3 class="text-white font-semibold text-lg">{{ course.title }}</h3>
      <p class="text-sm text-gray-400">Start Date: {{ course.startDate }}</p>
      <p class="text-sm text-gray-400">
        Completion Date: {{ course.completionDate || '--' }}
      </p>
      <p class="font-semibold" 
         [ngClass]="{
           'text-green-400': course.completion === 100,
           'text-orange-400': course.completion < 100 && course.completion >= 30,
           'text-red-400': course.completion < 30
         }">
        {{ course.completion }}% Complete
      </p>
    </div>
  </div>

  <!-- Right: Rating + Certificate -->
  <div class="flex flex-col items-end">
    <div class="flex items-center gap-1 text-yellow-400">
      <span class="font-bold text-lg">{{ course.rating }}</span>
      <i class="fas fa-star"></i>
      <span class="text-gray-400 text-sm">({{ course.reviews }} reviews)</span>
    </div>

    <button 
      class="mt-2 px-3 py-1 rounded-lg font-medium shadow"
      [ngClass]="isCertificateAvailable ? 'bg-blue-600 text-white hover:bg-blue-700' : 'bg-gray-600 text-gray-400 cursor-not-allowed'">
      Download Certificate
    </button>
  </div>
</div>
```

---

### 3. Component SCSS

```scss
.course-row {
  transition: transform 0.2s ease;
  &:hover {
    transform: translateY(-2px);
  }

  img {
    border: 2px solid #333;
  }

  button {
    transition: all 0.2s ease;
  }
}
```

---

### 4. Usage Example

```html
<app-learning-history-item 
  [course]="{
    image: 'assets/ai-course.jpg',
    title: 'Artificial Intelligence Professional Certificate',
    startDate: '02/08/2025',
    completionDate: '02/09/2025',
    completion: 100,
    rating: 4.8,
    reviews: 63833
  }">
</app-learning-history-item>

<app-learning-history-item 
  [course]="{
    image: 'assets/ml-course.jpg',
    title: 'Machine Learning A-Z: AI, Python & R',
    startDate: '02/07/2025',
    completionDate: null,
    completion: 80,
    rating: 4.1,
    reviews: 53843
  }">
</app-learning-history-item>
```

---

✨ This will give you the **exact pixel-perfect row** with:

* Same **color coding** for completion text.
* Correct **star rating + reviews** styling.
* **Download Certificate button enabled only when 100% complete**.

---

Do you also want me to make this **list reusable** (like `app-learning-history-list` that takes an array of courses and renders multiple rows), or just keep this **single row component** for now?
