Got it! Here’s a **reusable Angular (standalone) “Stat Card” component** styled with **Tailwind** + a tiny bit of SCSS for the soft circles.
You can pass **title, count, color, icon, footer text, link** via `@Input()` props.

---

# 1) Component

### `card-stat.component.ts`

```ts
import { Component, Input } from '@angular/core';
import { NgIf, NgClass } from '@angular/common';
import { RouterLink } from '@angular/router';

type Tone = 'emerald'|'blue'|'indigo'|'violet'|'fuchsia'|'rose'|'orange'|'amber'|'slate';

@Component({
  selector: 'app-card-stat',
  standalone: true,
  imports: [NgIf, NgClass, RouterLink],
  templateUrl: './card-stat.component.html',
  styleUrls: ['./card-stat.component.scss']
})
export class CardStatComponent {
  /** Big title at top-left */
  @Input() title = 'Title';
  /** Large number */
  @Input() count: number | string = 0;
  /** Tailwind color family used for gradient, ring & hover */
  @Input() color: Tone = 'emerald';
  /** Optional footer label like “View All” */
  @Input() footer?: string;
  /** Optional router link / URL */
  @Input() link?: string;
  /** Icon SVG (string). Provide any SVG path; see demo below */
  @Input() iconPath?: string;

  // Tailwind class helpers
  gradient(c: Tone) {
    return `bg-gradient-to-br from-${c}-900 to-${c}-800`;
  }
  ring(c: Tone) {
    return `ring-1 ring-inset ring-${c}-700/40 hover:ring-${c}-500/60`;
  }
  text(c: Tone) {
    return `text-${c}-300`;
  }
  button(c: Tone) {
    return `text-${c}-200 hover:text-white`;
  }
}
```

### `card-stat.component.html`

```html
<article
  class="stat-card relative overflow-hidden rounded-3xl p-6 md:p-7 shadow-xl transition
         text-white/90 backdrop-blur-sm"
  [ngClass]="[gradient(color), ring(color)]"
>
  <!-- top -->
  <div class="flex items-start justify-between gap-4">
    <h3 class="text-base md:text-lg font-medium leading-tight">{{ title }}</h3>

    <svg *ngIf="iconPath"
         class="size-8 md:size-10 opacity-80"
         fill="none" viewBox="0 0 24 24" stroke="currentColor"
         [ngClass]="text(color)">
      <path [attr.d]="iconPath" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
  </div>

  <!-- number -->
  <div class="mt-3 md:mt-4">
    <div class="text-5xl md:text-6xl font-semibold tracking-tight"
         style="font-variant-numeric: tabular-nums;">
      {{ count }}
    </div>
  </div>

  <!-- footer -->
  <div class="mt-6 flex items-center justify-between">
    <div class="text-xs md:text-sm text-white/70">
      <!-- You can project extra info here if needed -->
      <ng-content></ng-content>
    </div>

    <a *ngIf="footer && link" [routerLink]="link"
       class="text-sm font-medium underline-offset-4 hover:underline transition"
       [ngClass]="button(color)">
      {{ footer }}
    </a>
    <span *ngIf="footer && !link"
          class="text-sm font-medium" [ngClass]="button(color)">{{ footer }}</span>
  </div>

  <!-- soft circles decoration -->
  <div class="decoration pointer-events-none absolute inset-0"></div>
</article>
```

### `card-stat.component.scss`

```scss
/* Soft rings like your screenshot */
.stat-card .decoration::before,
.stat-card .decoration::after {
  content: "";
  position: absolute;
  border-radius: 9999px;
  border: 2px solid rgba(255,255,255,.12);
  width: 260px; height: 260px;
  right: -40px; bottom: -60px;
  transform: translate(0,0);
}
.stat-card .decoration::after {
  width: 180px; height: 180px;
  right: 80px; bottom: 30px;
  border-color: rgba(255,255,255,.18);
}
```

---

# 2) Example usage (parent)

### `dashboard.component.html`

```html
<div class="grid gap-5 md:grid-cols-3">
  <app-card-stat
    title="My Goals"
    [count]="3"
    color="emerald"
    footer="View All"
    link="/goals"
    [iconPath]="'M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0h6'"
  ></app-card-stat>

  <app-card-stat
    title="Enrolled Courses"
    [count]="8"
    color="indigo"
    footer="View All"
    link="/courses"
    [iconPath]="'M4 6h16M4 10h16M4 14h16M4 18h16'"
  ></app-card-stat>

  <app-card-stat
    title="Pending Tasks"
    [count]="12"
    color="rose"
    footer="Details"
    [iconPath]="'M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z'"
  >
    <!-- optional projected content -->
    Updated 2 min ago
  </app-card-stat>
</div>
```

---

# 3) Tailwind setup (quick)

* Ensure Tailwind is installed & configured for Angular.
* Add your component paths in `tailwind.config.js` `content` array (e.g. `./src/**/*.{html,ts}`).
* Because we generate color classes dynamically, **safelist** families you’ll use:

```js
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{html,ts}'],
  safelist: [
    { pattern: /(from|to|ring|text|hover:ring|hover:text)-(emerald|blue|indigo|violet|fuchsia|rose|orange|amber|slate)-(50|100|200|300|500|700|800|900)\/?(\d+)?/ }
  ],
  theme: { extend: {} },
  plugins: [],
};
```

---

## Notes

* You can add more color options by extending the `Tone` union and safelist.
* Replace `routerLink` with plain `<a [href]="link">` if you’re not using Angular Router.
* For a logo at the bottom instead of text, pass an SVG via `iconPath` or project custom markup in the footer `<ng-content>` area.

Want me to convert this into a small **Angular StackBlitz** snippet or add **loading/skeleton** states?
