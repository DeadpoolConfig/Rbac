Perfect ✅ You want the **About Me** section to have an **edit option with a Font Awesome pen icon**, so when clicked, it toggles between **view mode** and **edit mode (textarea + save)**.

I’ll give you the updated Angular code with this functionality:

---

### `profile.component.html`

```html
<div class="profile-container">
  <!-- Banner background -->
  <div class="banner"></div>

  <!-- Content -->
  <div class="profile-content">
    <!-- Profile Card -->
    <div class="profile-card">
      <img class="avatar" [src]="user.avatarUrl" alt="{{ user.fullName }}" />
      <h2 class="name">{{ user.fullName }}</h2>
      <p class="emp-id">Emp ID: {{ user.empId }}</p>
      <p class="designation">{{ user.track }}</p>
      <p class="email">{{ user.email }}</p>
    </div>

    <!-- About Section -->
    <div class="about-section">
      <div class="about-header">
        <h3>About Me</h3>
        <i
          class="fa fa-pen edit-icon"
          *ngIf="!isEditing"
          (click)="toggleEdit()"
        ></i>
        <i
          class="fa fa-save save-icon"
          *ngIf="isEditing"
          (click)="toggleEdit()"
        ></i>
      </div>

      <!-- View Mode -->
      <p *ngIf="!isEditing">{{ user.about }}</p>

      <!-- Edit Mode -->
      <textarea
        *ngIf="isEditing"
        [(ngModel)]="user.about"
        rows="4"
        class="edit-textarea"
      ></textarea>
    </div>

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
  </div>
</div>
```

---

### `profile.component.scss`

```scss
.profile-container {
  width: 100%;
  min-height: 100vh;
  background-color: #0d1b2a;
  color: #fff;
  font-family: 'Segoe UI', sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;

  .banner {
    width: 100%;
    height: 220px;
    background: url('/assets/banner-bg.png') no-repeat center center/cover;
  }

  .profile-content {
    width: 85%;
    margin-top: -80px;
    background: rgba(10, 25, 47, 0.95);
    border-radius: 12px;
    padding: 2rem;
    box-shadow: 0 6px 15px rgba(0, 0, 0, 0.3);
    display: flex;
    flex-direction: column;
    gap: 2rem;
  }

  .profile-card {
    background: #1b263b;
    border-radius: 12px;
    padding: 1.5rem;
    text-align: center;
    max-width: 280px;

    .avatar {
      width: 80px;
      height: 80px;
      border-radius: 50%;
      margin-bottom: 1rem;
    }

    .name {
      font-size: 1.5rem;
      font-weight: bold;
      margin: 0;
    }

    .emp-id,
    .designation,
    .email {
      font-size: 0.9rem;
      margin: 0.3rem 0;
      color: #ccc;
    }
  }

  .about-section {
    background: #1b263b;
    padding: 1.5rem;
    border-radius: 12px;
    position: relative;

    .about-header {
      display: flex;
      align-items: center;
      justify-content: space-between;

      h3 {
        font-size: 1.2rem;
        margin: 0;
      }

      .edit-icon,
      .save-icon {
        font-size: 1rem;
        cursor: pointer;
        color: #4fc3f7;
        transition: 0.2s;

        &:hover {
          color: #81d4fa;
        }
      }
    }

    p {
      margin-top: 1rem;
      line-height: 1.5;
      font-size: 0.95rem;
      color: #dcdcdc;
    }

    .edit-textarea {
      width: 100%;
      margin-top: 1rem;
      padding: 0.8rem;
      border-radius: 8px;
      border: none;
      outline: none;
      resize: none;
      font-size: 0.95rem;
      color: #000;
    }
  }

  .stats {
    display: flex;
    justify-content: space-around;
    background: #1b263b;
    padding: 1.5rem;
    border-radius: 12px;
    text-align: center;

    .stat {
      h2 {
        font-size: 2rem;
        font-weight: bold;
        color: #4fc3f7;
        margin: 0;
      }

      p {
        font-size: 0.9rem;
        color: #ccc;
        margin: 0.3rem 0 0;
      }
    }
  }
}
```

---

### `profile.component.ts`

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-profile',
  templateUrl: './profile.component.html',
  styleUrls: ['./profile.component.scss']
})
export class ProfileComponent {
  isEditing = false;

  user = {
    fullName: 'Harry Shimron',
    empId: '59694996',
    track: 'DC Software Engineer II',
    email: 'harshimron@deloitte.com',
    avatarUrl: 'https://i.pravatar.cc/150?u=hshimron',
    about:
      'This space can be used to add details about their verbal communication skills, presentation abilities, responsiveness to feedback, their advocacy for design principles etc.',
    learningHours: '4.5 hrs/week',
    pendingCourses: 2,
    ratedCourses: 6
  };

  toggleEdit() {
    this.isEditing = !this.isEditing;
  }
}
```

---

### 🔑 Notes

* Uses **Font Awesome** icons (`fa-pen`, `fa-save`).
* When you click the **pen icon**, it toggles to edit mode (textarea).
* When you click the **save icon**, it toggles back to view mode.
* Edit value is bound with `[(ngModel)]`. ✅

---

👉 Do you also want me to make the **edited “About” text persist** (e.g., save to backend / local storage), or is simple in-memory toggle enough for now?
