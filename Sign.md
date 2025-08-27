Got it ✅. You want me to replicate the **Sign Up UI** shown in the image using **Angular 18 + Angular Material**, where:

* Left side (**50% width**) → Image/illustration.
* Right side (**50% width**) → Form with:

  * Product Name
  * Sign up title
  * Input fields → Username, Email, Password
  * Checkbox (agree to terms)
  * Register button (Material style)
  * Link for existing account login

Here’s the **step-by-step Angular 18 code setup** for this replica:

---

### 1. Generate Angular project

```bash
ng new signup-ui --routing --style=scss
cd signup-ui
```

### 2. Install Angular Material

```bash
ng add @angular/material
```

Choose a theme (e.g., **Indigo/Pink**), enable **global typography** and **animations**.

---

### 3. Create a SignUp Component

```bash
ng g c pages/signup
```

---

### 4. Signup Component HTML (`signup.component.html`)

```html
<div class="signup-container">
  <!-- Left Side - Image -->
  <div class="signup-image">
    <img src="assets/signup-illustration.png" alt="Signup Illustration" />
  </div>

  <!-- Right Side - Form -->
  <div class="signup-form">
    <h2 class="product-name">Product Name</h2>
    <h3 class="form-title">Sign up</h3>
    <p class="subtitle">Enter details mentioned below</p>

    <form>
      <mat-form-field appearance="outline" class="full-width">
        <mat-label>User Name</mat-label>
        <input matInput type="text" />
      </mat-form-field>

      <mat-form-field appearance="outline" class="full-width">
        <mat-label>Email</mat-label>
        <input matInput type="email" />
      </mat-form-field>

      <mat-form-field appearance="outline" class="full-width">
        <mat-label>Password</mat-label>
        <input matInput type="password" />
      </mat-form-field>

      <mat-checkbox>I agree to the terms and conditions by signing up</mat-checkbox>

      <button mat-raised-button color="primary" class="full-width register-btn">
        Register
      </button>

      <p class="login-text">
        Already have an Account? <a href="#">Login</a>
      </p>
    </form>
  </div>
</div>
```

---

### 5. SCSS Styles (`signup.component.scss`)

```scss
.signup-container {
  display: flex;
  height: 100vh;
  width: 100%;

  .signup-image {
    width: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #e0f7fa;

    img {
      width: 80%;
      height: auto;
      border-radius: 10px;
    }
  }

  .signup-form {
    width: 50%;
    padding: 3rem;
    display: flex;
    flex-direction: column;
    justify-content: center;

    .product-name {
      font-size: 1.8rem;
      font-weight: bold;
      margin-bottom: 0.5rem;
    }

    .form-title {
      font-size: 1.4rem;
      margin-bottom: 0.5rem;
    }

    .subtitle {
      margin-bottom: 1.5rem;
      color: #666;
    }

    .full-width {
      width: 100%;
      margin-bottom: 1rem;
    }

    .register-btn {
      margin-top: 1rem;
    }

    .login-text {
      margin-top: 1rem;
      font-size: 0.9rem;
      text-align: center;

      a {
        color: #1976d2;
        text-decoration: none;
        font-weight: 500;
      }
    }
  }
}
```

---

### 6. Import Angular Material Modules (`app.module.ts`)

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';
import { MatFormFieldModule } from '@angular/material/form-field';
import { MatInputModule } from '@angular/material/input';
import { MatButtonModule } from '@angular/material/button';
import { MatCheckboxModule } from '@angular/material/checkbox';

import { AppComponent } from './app.component';
import { SignupComponent } from './pages/signup/signup.component';

@NgModule({
  declarations: [AppComponent, SignupComponent],
  imports: [
    BrowserModule,
    BrowserAnimationsModule,
    FormsModule,
    ReactiveFormsModule,
    MatFormFieldModule,
    MatInputModule,
    MatButtonModule,
    MatCheckboxModule,
  ],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

---

### 7. Route Setup (`app-routing.module.ts`)

```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { SignupComponent } from './pages/signup/signup.component';

const routes: Routes = [
  { path: '', component: SignupComponent },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
})
export class AppRoutingModule {}
```

---

✅ Now, when you run:

```bash
ng serve
```

You’ll get a **50-50 split layout** → Left image & Right Material UI SignUp form exactly like in your screenshot.

---

Do you want me to also make this **responsive** (for tablet/mobile → image on top and form below)?
