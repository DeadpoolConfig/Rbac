Got it 👍
You already have a **Course interface** and a **Provider interface** like this:

```ts
export interface Provider {
  name: string;
  logoUrl: string;
}

export interface Course {
  id: number;
  title: string;
  subtitle: string;
  authorId: number;
  provider: Provider;      // provider has name + logo
  thumbnailUrl: string;
  rating: number;
  reviewCount: number;
  enrollmentCount: number;
  difficulty: 'Beginner' | 'Intermediate' | 'Advanced';
  durationText: string;
  skills: string[];
  whatYoullLearn: string[];
  requirements: string[];
  status: 'Published' | 'Draft' | 'Archived';
  publishedDate: string;
}
```

---

### 🔹 Update the **Card Component** to Accept Course Data

Instead of passing multiple inputs separately, let’s pass the **whole `Course` object**.

#### `course-card.component.ts`

```ts
import { Component, Input } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Course } from '../models/course.model'; // adjust path

@Component({
  selector: 'app-course-card',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './course-card.component.html',
  styleUrls: ['./course-card.component.scss']
})
export class CourseCardComponent {
  @Input() course!: Course;
  @Input() progress: string = ''; // keep progress optional
}
```

---

#### `course-card.component.html`

```html
<div class="course-card">
  <!-- Thumbnail + Progress -->
  <div class="image-wrapper">
    <img [src]="course.thumbnailUrl" alt="Course Thumbnail" class="thumbnail"/>
    <div *ngIf="progress" class="progress-badge">{{ progress }}</div>
  </div>

  <!-- Content -->
  <div class="content">
    <h3 class="title">{{ course.title }}</h3>

    <!-- Provider -->
    <div class="platform">
      <img [src]="course.provider.logoUrl" [alt]="course.provider.name" class="platform-logo"/>
    </div>

    <!-- Rating + Reviews -->
    <div class="rating-row">
      <i class="fa fa-star star-icon"></i>
      <span class="rating">{{ course.rating }}</span>
      <span class="details">({{ course.reviewCount }} reviews | {{ course.enrollmentCount }} Enrolled)</span>
    </div>

    <!-- Difficulty + Duration -->
    <div class="footer">
      <span class="level">{{ course.difficulty }}</span>
      <span class="dot">·</span>
      <span class="duration">{{ course.durationText }}</span>
    </div>
  </div>
</div>
```

---

#### `course-card.component.scss`

(unchanged, same as before — will keep design pixel perfect)

---

### 🔹 Example Usage

#### `dashboard.component.ts`

```ts
import { Component } from '@angular/core';
import { Course } from '../models/course.model';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
})
export class DashboardComponent {
  sampleCourse: Course = {
    id: 1,
    title: 'Google Data Analytics',
    subtitle: 'Learn data analytics with Google',
    authorId: 101,
    provider: {
      name: 'LinkedIn Learning',
      logoUrl: 'assets/linkedin-learning.png'
    },
    thumbnailUrl: 'assets/google-data.jpg',
    rating: 4.3,
    reviewCount: 133000,
    enrollmentCount: 635,
    difficulty: 'Beginner',
    durationText: '6 months',
    skills: ['SQL', 'Excel', 'Data Visualization'],
    whatYoullLearn: [],
    requirements: [],
    status: 'Published',
    publishedDate: '2025-01-01'
  };
}
```

---

#### `dashboard.component.html`

```html
<app-course-card 
  [course]="sampleCourse" 
  progress="21% Complete">
</app-course-card>
```

---

✅ Now your **card consumes the real `Course` object structure** and will remain reusable for any future courses.
✅ You only need to pass `[course]` and optional `[progress]`.

---

Do you also want me to make it **responsive grid-ready** (so you can pass a list of `Course[]` and display them as cards in a row/column layout)?
