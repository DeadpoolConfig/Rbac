Perfect 🙌 You want a **reusable Angular component for testimonials** (like the screenshot).
We’ll make it **dynamic** so that you can pass testimonial data and it will render automatically.
We’ll use **Angular + Tailwind + SCSS + HTML**.

---

## 1. Create Component

```bash
ng g c components/testimonial-card
```

---

## 2. testimonial-card.component.ts

```ts
import { Component, Input } from '@angular/core';

interface Testimonial {
  rating: number;   // e.g., 4.8
  review: string;   // testimonial text
  userName: string; // reviewer name
  userTitle: string; // e.g., "Learning for U.S"
  avatarUrl: string; // profile image url
}

@Component({
  selector: 'app-testimonial-card',
  templateUrl: './testimonial-card.component.html',
  styleUrls: ['./testimonial-card.component.scss']
})
export class TestimonialCardComponent {
  @Input() testimonial!: Testimonial;

  getStarsArray(): number[] {
    return Array(Math.round(this.testimonial.rating)).fill(0);
  }
}
```

---

## 3. testimonial-card.component.html

```html
<div class="bg-gray-900 text-white rounded-2xl shadow-lg p-6 max-w-3xl mx-auto">
  <!-- Rating -->
  <div class="flex items-center gap-2 mb-4">
    <span class="text-lg font-semibold">{{ testimonial.rating }}</span>
    <div class="flex text-yellow-400">
      <ng-container *ngFor="let _ of getStarsArray()">
        <i class="fas fa-star"></i>
      </ng-container>
    </div>
  </div>

  <!-- Review Text -->
  <p class="text-gray-300 leading-relaxed mb-6">
    {{ testimonial.review }}
  </p>

  <!-- User Info -->
  <div class="flex items-center gap-4 bg-teal-700 rounded-xl p-4">
    <img [src]="testimonial.avatarUrl" alt="User Avatar" class="w-12 h-12 rounded-full border-2 border-white shadow-md">
    <div>
      <p class="font-semibold text-white">{{ testimonial.userName }}</p>
      <p class="text-gray-200 text-sm">{{ testimonial.userTitle }}</p>
    </div>
  </div>
</div>
```

---

## 4. testimonial-card.component.scss

```scss
:host {
  display: block;

  i {
    margin-right: 2px;
  }
}
```

---

## 5. Example Usage in `app.component.ts`

```ts
testimonialData = {
  rating: 4.8,
  review: `The Google Data Analytics course was a pivotal moment in my career.
  I went from having a basic understanding of spreadsheets to confidently performing complex queries in SQL 
  and building interactive dashboards in Tableau. The real-world projects helped me build a portfolio that stood out 
  to employers. I landed a job as a Junior Data Analyst just two months after completing the certificate.`,
  userName: "Wade Warren",
  userTitle: "Learning for U.S",
  avatarUrl: "https://i.pravatar.cc/150?img=5"
};
```

---

## 6. Example Usage in `app.component.html`

```html
<app-testimonial-card [testimonial]="testimonialData"></app-testimonial-card>
```

---

✅ Fully reusable testimonial component
✅ Supports **dynamic data** (rating, review text, user info, avatar)
✅ Uses **Tailwind + SCSS** for styling
✅ Auto-generates stars ⭐ from rating

---

Do you want me to also extend it into a **carousel/slider** so you can show multiple testimonials (like 4–5) with navigation?
