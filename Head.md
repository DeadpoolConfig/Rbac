Got it ✅
We’ll build the **header component in Angular 19** with:

* **Product Name** (Braindom)
* **Central Search Bar** (with auto-complete + reset `X` button)
* **Notification Icon** (with count badge)
* **User Avatar** (profile picture)
* **SCSS + Bootstrap styling**

I’ll also use **Angular Material Autocomplete** for the dropdown suggestions (best for typeahead functionality).

---

### Step 1: Create a header component

```bash
ng g c components/header
```

---

### Step 2: Install Angular Material (if not already)

```bash
ng add @angular/material
```

---

### Step 3: `header.component.html`

```html
<header class="header-container d-flex align-items-center justify-content-between px-4 py-2">
  <!-- Product Name -->
  <div class="product-name">
    <h4>Braindom</h4>
  </div>

  <!-- Search Bar -->
  <div class="search-bar flex-grow-1 px-4">
    <mat-form-field appearance="outline" class="search-field w-100">
      <mat-icon matPrefix>search</mat-icon>
      <input 
        type="text" 
        placeholder="Search Courses"
        matInput 
        [formControl]="searchControl"
        [matAutocomplete]="auto">

      <!-- Reset Button -->
      <button *ngIf="searchControl.value" matSuffix mat-icon-button (click)="resetSearch()">
        <mat-icon>close</mat-icon>
      </button>
    </mat-form-field>

    <!-- Autocomplete -->
    <mat-autocomplete #auto="matAutocomplete">
      <mat-option *ngFor="let course of filteredCourses | async" [value]="course">
        {{ course }}
      </mat-option>
    </mat-autocomplete>
  </div>

  <!-- Notification + Profile -->
  <div class="d-flex align-items-center gap-3">
    <!-- Notification Icon -->
    <div class="position-relative">
      <mat-icon class="notification-icon">notifications</mat-icon>
      <span class="badge">{{ notificationCount }}</span>
    </div>

    <!-- User Avatar -->
    <img class="avatar" src="assets/avatar.png" alt="User Avatar" />
  </div>
</header>
```

---

### Step 4: `header.component.ts`

```ts
import { Component } from '@angular/core';
import { FormControl } from '@angular/forms';
import { Observable, map, startWith } from 'rxjs';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.scss']
})
export class HeaderComponent {
  searchControl = new FormControl('');
  courses: string[] = ['Angular Basics', 'React Fundamentals', 'Node.js Intro', 'Spring Boot Mastery', 'Python for Data Science'];
  filteredCourses!: Observable<string[]>;

  notificationCount = 3;

  ngOnInit() {
    this.filteredCourses = this.searchControl.valueChanges.pipe(
      startWith(''),
      map(value => this._filter(value || ''))
    );
  }

  private _filter(value: string): string[] {
    const filterValue = value.toLowerCase();
    return this.courses.filter(course => course.toLowerCase().includes(filterValue));
  }

  resetSearch() {
    this.searchControl.setValue('');
  }
}
```

---

### Step 5: `header.component.scss`

```scss
.header-container {
  background: #1e1e2f;
  color: #fff;
  display: flex;
  align-items: center;
}

.product-name h4 {
  margin: 0;
  font-weight: 600;
  color: #fff;
}

.search-bar {
  max-width: 600px;
}

.search-field {
  background: #2d2d3a;
  border-radius: 30px;
  padding-left: 10px;

  input {
    color: #fff;
  }

  mat-icon {
    color: #bbb;
  }
}

.notification-icon {
  color: #fff;
  font-size: 28px;
  cursor: pointer;
}

.badge {
  position: absolute;
  top: -4px;
  right: -6px;
  background: red;
  color: white;
  font-size: 12px;
  padding: 2px 6px;
  border-radius: 50%;
}

.avatar {
  width: 38px;
  height: 38px;
  border-radius: 50%;
  border: 2px solid #fff;
  cursor: pointer;
}
```

---

👉 This gives you:
✔ **Braindom text** on left
✔ **Search bar with autocomplete & reset button** in center
✔ **Notification bell with badge** + **User avatar** on right

---

Do you want me to make the **auto-complete dropdown styled like your screenshot (dark background with white text)** instead of default Angular Material theme?
