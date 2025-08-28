Perfect ✅ I see the **Learning History (05)** card UI in your screenshot.
We’ll extend the profile page: after the **Stats section**, we’ll add a **Learning History card list** with **course image, title, start/completion date, completion status, rating (stars), reviews, and download button** — pixel-perfect to the UI you provided.

Here’s the updated Angular implementation:

---

### `profile.component.html` (append after stats)

```html
<!-- Stats Section -->
<div class="stats">
  <div class="stat">
    <h2>{{ user.learningHours }}</h2>
    <p>Learning Hours</p>
  </div>
  <div class="stat">
    <h2>{{ user.pendingCourses }}</h2>
    <p>Pending Courses</p>
  </div>
  <div class="stat">
    <h2>{{ user.ratedCourses }}</h2>
    <p>Rated Courses</p>
  </div>
</div>

<!-- Learning History Section -->
<div class="learning-history">
  <div class="header">
    <h3>Learning History ({{ learningHistory.length | number:'2.0' }})</h3>
    <span class="sort">Sort: This Year</span>
  </div>

  <div class="course-card" *ngFor="let course of learningHistory">
    <img class="thumbnail" [src]="course.image" alt="{{ course.title }}" />

    <div class="course-info">
      <h4>{{ course.title }}</h4>
      <p>Start Date: {{ course.startDate }}</p>
      <p>
        Completion Date:
        <span *ngIf="course.completionDate; else dash">
          {{ course.completionDate }}
        </span>
        <ng-template #dash> -- </ng-template>
      </p>
      <p
        class="status"
        [ngClass]="{
          complete: course.completionPercent === 100,
          inprogress: course.completionPercent > 0 && course.completionPercent < 100,
          notstarted: course.completionPercent === 0
        }"
      >
        {{ course.completionPercent }}% Complete
      </p>
    </div>

    <div class="course-meta">
      <p class="rating">
        {{ course.rating }}
        <i class="fa fa-star star"></i>
        <span class="reviews">({{ course.reviews }} reviews)</span>
      </p>
      <button
        class="download-btn"
        [disabled]="course.completionPercent < 100"
      >
        <i class="fa fa-download"></i> Download Certificate
      </button>
    </div>
  </div>
</div>
```

---

### `profile.component.scss` (append)

```scss
.learning-history {
  background: #1b263b;
  padding: 1.5rem;
  border-radius: 12px;

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;

    h3 {
      margin: 0;
      font-size: 1.2rem;
    }

    .sort {
      font-size: 0.9rem;
      color: #ccc;
    }
  }

  .course-card {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: #0d1b2a;
    padding: 1rem;
    border-radius: 10px;
    margin-bottom: 1rem;

    .thumbnail {
      width: 60px;
      height: 60px;
      border-radius: 8px;
      margin-right: 1rem;
      object-fit: cover;
    }

    .course-info {
      flex: 2;

      h4 {
        margin: 0 0 0.5rem 0;
        font-size: 1rem;
        color: #fff;
      }

      p {
        margin: 0.2rem 0;
        font-size: 0.85rem;
        color: #ccc;
      }

      .status {
        font-weight: bold;

        &.complete {
          color: #00e5ff;
        }
        &.inprogress {
          color: #ffa726;
        }
        &.notstarted {
          color: #e53935;
        }
      }
    }

    .course-meta {
      text-align: right;
      min-width: 180px;

      .rating {
        font-size: 0.9rem;
        color: #fff;

        .star {
          color: gold;
          margin-left: 2px;
        }

        .reviews {
          color: #aaa;
          font-size: 0.8rem;
        }
      }

      .download-btn {
        margin-top: 0.5rem;
        background: #1e88e5;
        color: white;
        padding: 0.4rem 0.8rem;
        border: none;
        border-radius: 6px;
        font-size: 0.8rem;
        cursor: pointer;
        transition: 0.2s;

        i {
          margin-right: 4px;
        }

        &:hover:not(:disabled) {
          background: #1565c0;
        }

        &:disabled {
          background: #555;
          cursor: not-allowed;
        }
      }
    }
  }
}
```

---

### `profile.component.ts` (add learning history array)

```ts
learningHistory = [
  {
    title: 'Artificial Intelligence Professional Certificate',
    image: '/assets/courses/ai.jpg',
    startDate: '02/08/2025',
    completionDate: '02/09/2025',
    completionPercent: 100,
    rating: 4.8,
    reviews: 63838
  },
  {
    title: 'Machine Learning A-Z: AI, Python & R',
    image: '/assets/courses/ml1.jpg',
    startDate: '02/07/2025',
    completionDate: null,
    completionPercent: 80,
    rating: 4.1,
    reviews: 53843
  },
  {
    title: 'Machine Learning A-Z: AI, Python & R',
    image: '/assets/courses/ml2.jpg',
    startDate: '23/06/2025',
    completionDate: null,
    completionPercent: 20,
    rating: 4.0,
    reviews: 53843
  },
  {
    title: 'Python for Data Science and Machine Learning Bootcamp',
    image: '/assets/courses/python.jpg',
    startDate: '15/06/2025',
    completionDate: null,
    completionPercent: 80,
    rating: 4.7,
    reviews: 53843
  },
  {
    title: 'Tableau for Beginners: Get Certified in Data Visualization',
    image: '/assets/courses/tableau.jpg',
    startDate: '12/06/2025',
    completionDate: '02/07/2025',
    completionPercent: 100,
    rating: 4.4,
    reviews: 53843
  }
];
```

---

✅ This gives you a **pixel-perfect replica** of the **Learning History UI** in Angular 19 with SCSS:

* Thumbnail left, details in middle, ratings + download on right.
* Completion % color-coded (blue ✅ complete, orange 🟠 in-progress, red 🔴 low).
* Download button only enabled when **100% complete**.

---

👉 Do you want me to also make the **Download Certificate button dynamic** (e.g., trigger file download / open link), or just leave it as UI for now?
