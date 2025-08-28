Got it 👍
You want me to create a **reusable Angular 19 component** (HTML + SCSS) for the **course card UI** shown in your screenshot (Google Data Analytics course card).

Here’s a clean **Angular reusable component** implementation:

---

### 📌 course-card.component.ts

```ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-course-card',
  templateUrl: './course-card.component.html',
  styleUrls: ['./course-card.component.scss']
})
export class CourseCardComponent {
  @Input() imageUrl!: string;
  @Input() title!: string;
  @Input() description!: string;
  @Input() author!: string;
  @Input() authorRole!: string;
  @Input() rating!: number;
  @Input() reviews!: string;
  @Input() authorImage!: string;
}
```

---

### 📌 course-card.component.html

```html
<div class="course-card">
  <!-- Top Banner Image -->
  <div class="banner">
    <img [src]="imageUrl" alt="Course banner" />
  </div>

  <!-- Content Section -->
  <div class="content">
    <h3 class="title">{{ title }}</h3>
    <p class="description">{{ description }}</p>

    <!-- Author Section -->
    <div class="author-section">
      <img class="author-img" [src]="authorImage" alt="Author" />
      <div class="author-info">
        <p class="author-name">{{ author }}</p>
        <span class="author-role">{{ authorRole }}</span>
      </div>
      <div class="rating">
        <span class="star">★</span>
        <span>{{ rating }}</span>
        <span class="reviews">({{ reviews }} reviews)</span>
      </div>
    </div>
  </div>
</div>
```

---

### 📌 course-card.component.scss

```scss
.course-card {
  width: 350px;
  background: #1e2235;
  border-radius: 12px;
  overflow: hidden;
  color: #fff;
  box-shadow: 0 4px 15px rgba(0,0,0,0.3);
  display: flex;
  flex-direction: column;

  .banner {
    img {
      width: 100%;
      height: 180px;
      object-fit: cover;
    }
  }

  .content {
    padding: 16px;

    .title {
      font-size: 1.2rem;
      font-weight: 600;
      margin-bottom: 6px;
    }

    .description {
      font-size: 0.9rem;
      color: #c7c7c7;
      margin-bottom: 12px;
    }

    .author-section {
      display: flex;
      align-items: center;
      justify-content: space-between;

      .author-img {
        width: 40px;
        height: 40px;
        border-radius: 50%;
        margin-right: 10px;
      }

      .author-info {
        flex: 1;
        .author-name {
          font-weight: 600;
        }
        .author-role {
          font-size: 0.75rem;
          color: #aaa;
        }
      }

      .rating {
        font-size: 0.9rem;
        display: flex;
        align-items: center;

        .star {
          color: #00d26a;
          margin-right: 4px;
        }

        .reviews {
          font-size: 0.75rem;
          color: #bbb;
          margin-left: 4px;
        }
      }
    }
  }
}
```

---

### 📌 Usage Example (in any parent component)

```html
<app-course-card
  imageUrl="assets/google-data-analytics.jpg"
  title="Google Data Analytics"
  description="Spreadsheet, Data Analysis, SQL, Data Visualization, Data Cleansing"
  author="ANU BHATT"
  authorRole="ENGINEERING MASTER"
  rating="4.3"
  reviews="133K"
  authorImage="assets/anu-bhatt.jpg"
></app-course-card>
```

---

✅ This component is **fully reusable** – you just pass inputs for different courses and it will render the same styled UI.

Do you want me to also **add Tailwind support** for even cleaner utility-based styling (like we did in your earlier tasks), or keep it **pure SCSS**?
