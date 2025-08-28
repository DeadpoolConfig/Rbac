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
<div class="profile-root max-w-6xl mx-auto p-6">
  <!-- Top banner with illustration and purple border -->
  <div class="banner relative rounded-md overflow-hidden border-4 border-[#9b59ff]">
    <img src="/assets/banner-illustration.jpg" alt="banner" class="w-full h-48 object-cover">

    <!-- info card that overlaps the banner -->
    <div class="info-card absolute left-6 bottom-[-34px] flex items-center bg-black bg-opacity-70 text-white rounded-md px-4 py-3 shadow-lg">
      <img [src]="user.avatarUrl" alt="avatar" class="avatar w-16 h-16 rounded-full border-2 border-white object-cover">
      <div class="ml-4">
        <div class="text-xl font-semibold">{{ user.fullName }}</div>
        <div class="text-xs opacity-80">Emp ID: 59694996</div>
        <div class="text-sm opacity-90 mt-1">{{ user.track }}</div>
      </div>
    </div>
  </div>

  <!-- Spacer to compensate for overlap -->
  <div class="h-12"></div>

  <!-- About me section -->
  <section class="about bg-[#1f2937] rounded-md p-6 mt-4 text-gray-100">
    <h3 class="text-lg font-medium mb-4">About Me</h3>
    <p class="text-sm leading-6">{{ user.bio }}</p>
  </section>

  <!-- Stats row -->
  <section class="stats grid grid-cols-3 gap-6 mt-8 text-center">
    <div class="stat-card bg-transparent">
      <div class="stat-value text-4xl font-semibold">{{ user.learningHoursPerWeek }}</div>
      <div class="stat-label text-sm opacity-80">hrs/week</div>
      <div class="mt-2 text-sm opacity-80">Learning Hours</div>
    </div>

    <div class="stat-card bg-transparent">
      <div class="stat-value text-4xl font-semibold">{{ user.pendingCourses }}</div>
      <div class="stat-label text-sm opacity-80">&nbsp;</div>
      <div class="mt-2 text-sm opacity-80">Pending Courses</div>
    </div>

    <div class="stat-card bg-transparent">
      <div class="stat-value text-4xl font-semibold">{{ user.ratedCourses }}</div>
      <div class="stat-label text-sm opacity-80">&nbsp;</div>
      <div class="mt-2 text-sm opacity-80">Rated Courses</div>
    </div>
  </section>
</div>


/* Filename: profile.component.scss */
:host {
  display: block;
}

.profile-root {
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
}

.banner {
  height: 200px;
  background: linear-gradient(180deg, #183048 0%, #12303c 100%);
}

.banner img {
  filter: saturate(0.95) contrast(0.95);
}

.info-card {
  transform: translateY(0%);
  min-width: 360px;
}

.avatar {
  box-shadow: 0 6px 18px rgba(2,6,23,0.6);
}

.about {
  background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
  border-radius: 8px;
}

.stats .stat-card {
  color: #e6e7ea;
}

.stat-value {
  text-shadow: 0 2px 0 rgba(0,0,0,0.25);
}

/* Responsive tweaks */
@media (max-width: 768px) {
  .info-card { left: 12px; right: 12px; width: calc(100% - 24px); }
  .banner { height: 160px; }
  .avatar { width: 56px; height: 56px; }
  .stats { grid-template-columns: 1fr; }
}

/* Tailwind usage note: This template assumes Tailwind is enabled in your Angular build. */
/* To use this component:
   1. Add app-profile-card selector in your page template: <app-profile-card [user]="profile"></app-profile-card>
   2. Replace /assets/banner-illustration.jpg with the actual illustration from your assets (or keep the purple bordered banner using CSS border color).
   3. Ensure Tailwind classes are available (tailwindcss configured in angular.json + postcss).
*/
