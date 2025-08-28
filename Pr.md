<!-- Filename: profile.component.ts -->
import { Component, Input } from '@angular/core';

export interface UserProfile {
  id: number;
  username: string;
  email: string;
  fullName: string;
  track: string;
  avatarUrl: string;
  joinDate: string;
  role: string;
  bio: string;
  location?: string;
  learningHoursPerWeek?: number;
  pendingCourses?: number;
  ratedCourses?: number;
}

@Component({
  selector: 'app-profile-card',
  templateUrl: './profile.component.html',
  styleUrls: ['./profile.component.scss']
})
export class ProfileComponent {
  @Input() user: UserProfile = {
    id: 1,
    username: 'hshimron',
    email: 'harry.shimron@example.com',
    fullName: 'Harry Shimron',
    track: 'DC Software Engineer II',
    avatarUrl: 'https://i.pravatar.cc/150?u=hshimron',
    joinDate: '2021-08-14T00:00:00.000Z',
    role: 'Learner',
    bio: 'Passionate writer and avid traveler, Harry shares insights from his adventures around the globe. With a background in journalism, he combines storytelling with a love for photography, capturing the essence of each destination.',
    location: 'Bangalore, India',
    learningHoursPerWeek: 4.5,
    pendingCourses: 2,
    ratedCourses: 6
  };
}


<!-- Filename: profile.component.html -->
<div class="profile-root mx-auto p-6 max-w-7xl">
  <!-- Banner (uses uploaded photo if available) -->
  <div class="banner-wrapper">
    <div class="banner-border">
      <img src="/mnt/data/IMG_20250828_181549.jpg" alt="banner" class="banner-image" />
    </div>

    <!-- Floating info card -->
    <div class="info-card" role="region" aria-label="user summary">
      <img [src]="user.avatarUrl" alt="{{user.fullName}} avatar" class="avatar" />
      <div class="info-text">
        <div class="name">{{ user.fullName }}</div>
        <div class="meta">Emp ID: <span class="emp">59694996</span></div>
        <div class="track">{{ user.track }}</div>
      </div>
    </div>
  </div>

  <!-- Spacer to compensate for overlap -->
  <div class="overlap-spacer"></div>

  <!-- About section -->
  <section class="about bg-dark-panel p-6 rounded-md">
    <h3 class="section-title">About Me</h3>
    <p class="about-text">{{ user.bio }}</p>
  </section>

  <!-- Stats row -->
  <section class="stats grid grid-cols-3 gap-6 mt-8 text-center">
    <div class="stat">
      <div class="stat-number">{{ user.learningHoursPerWeek }}</div>
      <div class="stat-sub">hrs/week</div>
      <div class="stat-label">Learning Hours</div>
    </div>

    <div class="stat">
      <div class="stat-number">{{ user.pendingCourses }}</div>
      <div class="stat-sub">&nbsp;</div>
      <div class="stat-label">Pending Courses</div>
    </div>

    <div class="stat">
      <div class="stat-number">{{ user.ratedCourses }}</div>
      <div class="stat-sub">&nbsp;</div>
      <div class="stat-label">Rated Courses</div>
    </div>
  </section>
</div>


/* Filename: profile.component.scss */
:host { display: block; }

/* Design tokens - tweak to match pixels precisely */
$purple: #a24bff;
$panel: #111318;
$panel-2: #1f2730;
$text: #e9eef6;
$muted: rgba(233,238,246,0.72);

.profile-root {
  font-family: Inter, 'Segoe UI', Roboto, Arial, sans-serif;
}

.banner-wrapper {
  position: relative;
}

.banner-border {
  border: 5px solid $purple;
  border-radius: 6px;
  overflow: hidden;
  box-shadow: 0 6px 18px rgba(0,0,0,0.45);
}

.banner-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  display: block;
  transform: translateZ(0);
  filter: saturate(0.95) contrast(0.95);
}

/* Floating info card */
.info-card {
  position: absolute;
  left: 28px;
  bottom: -36px; /* overlaps banner */
  background: rgba(8,10,12,0.85);
  padding: 12px 16px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  min-width: 380px;
  gap: 12px;
  box-shadow: 0 10px 30px rgba(2,6,23,0.65);
}

.avatar {
  width: 68px;
  height: 68px;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid rgba(255,255,255,0.12);
}

.info-text .name {
  color: $text;
  font-size: 18px;
  font-weight: 600;
}

.info-text .meta {
  color: $muted;
  font-size: 12px;
  margin-top: 4px;
}

.info-text .track {
  color: rgba(233,238,246,0.94);
  font-size: 13px;
  margin-top: 6px;
}

.overlap-spacer { height: 44px; }

.about {
  background: linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.00));
  border-radius: 8px;
}

.section-title { color: $text; font-size: 16px; margin-bottom: 8px; }
.about-text { color: $muted; line-height: 1.6; font-size: 13px; }

.stats .stat { color: $text; }
.stat-number { font-size: 48px; font-weight: 600; line-height: 1; }
.stat-sub { font-size: 16px; color: $muted; margin-top: -6px; }
.stat-label { font-size: 12px; color: $muted; margin-top: 8px; }

/* Responsive */
@media (max-width: 900px) {
  .info-card { left: 16px; right: 16px; min-width: auto; width: calc(100% - 32px); }
  .avatar { width: 56px; height: 56px; }
  .banner-image { height: 160px; }
  .stat-number { font-size: 36px; }
  .stats { grid-template-columns: 1fr; }
}

/* Tailwind utility note: The template includes Tailwind classes where helpful (grid, spacing). If your Angular project uses Tailwind, the classes will apply automatically. */

/* Usage instructions:
1. Place the three files in an Angular component folder: profile.component.ts, profile.component.html, profile.component.scss
2. Ensure the banner image path (/mnt/data/IMG_20250828_181549.jpg) is available in your assets or replace with /assets/banner.jpg.
3. Use the selector <app-profile-card [user]="profile"></app-profile-card> in a parent template and pass real user data.
4. Tailwind is optional; SCSS handles the core pixel layout to match the provided mock.
*/
