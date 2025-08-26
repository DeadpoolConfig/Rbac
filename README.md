Role-Based Healthcare Appointment System

I'll enhance the existing code to implement role-based access control (RBAC) for doctors, admins, and patients. This will include different views and functionality based on user roles.

Updated Implementation

1. Enhanced User Model and Data

First, let's update our db.json to include more comprehensive role-based data:

```json
{
  "users": [
    {
      "id": 1,
      "email": "patient@example.com",
      "password": "password123",
      "name": "John Doe",
      "role": "patient",
      "phone": "555-123-4567",
      "dateOfBirth": "1985-05-15"
    },
    {
      "id": 2,
      "email": "doctor@example.com",
      "password": "doctor123",
      "name": "Dr. Sarah Smith",
      "role": "doctor",
      "specialization": "Cardiology",
      "phone": "555-987-6543",
      "yearsOfExperience": 12
    },
    {
      "id": 3,
      "email": "admin@example.com",
      "password": "admin123",
      "name": "Admin User",
      "role": "admin",
      "phone": "555-000-0000"
    }
  ],
  "appointments": [
    {
      "id": 1,
      "patientId": 1,
      "patientName": "John Doe",
      "doctorId": 2,
      "doctorName": "Dr. Sarah Smith",
      "date": "2023-10-15",
      "time": "10:00 AM",
      "reason": "Regular checkup",
      "status": "confirmed",
      "notes": "Patient needs to fast before appointment"
    },
    {
      "id": 2,
      "patientId": 1,
      "patientName": "John Doe",
      "doctorId": 2,
      "doctorName": "Dr. Sarah Smith",
      "date": "2023-10-20",
      "time": "2:30 PM",
      "reason": "Follow-up appointment",
      "status": "pending"
    },
    {
      "id": 3,
      "patientId": 4,
      "patientName": "Mary Johnson",
      "doctorId": 2,
      "doctorName": "Dr. Sarah Smith",
      "date": "2023-10-18",
      "time": "9:00 AM",
      "reason": "Heart condition consultation",
      "status": "confirmed"
    }
  ],
  "doctors": [
    {
      "id": 2,
      "name": "Dr. Sarah Smith",
      "specialization": "Cardiology",
      "availableSlots": ["9:00 AM", "10:00 AM", "2:30 PM", "4:00 PM"]
    },
    {
      "id": 5,
      "name": "Dr. Michael Johnson",
      "specialization": "Pediatrics",
      "availableSlots": ["8:00 AM", "11:00 AM", "3:00 PM", "5:00 PM"]
    }
  ]
}
```

2. Enhanced User Model

```typescript
// models/user.model.ts
export interface User {
  id: number;
  email: string;
  password: string;
  name: string;
  role: 'patient' | 'doctor' | 'admin';
  phone?: string;
  dateOfBirth?: string;
  specialization?: string;
  yearsOfExperience?: number;
}
```

3. Role-Based Navigation Component

```typescript
// components/navigation/navigation.component.ts
import { Component } from '@angular/core';
import { AuthService } from '../../services/auth.service';

@Component({
  selector: 'app-navigation',
  templateUrl: './navigation.component.html',
  styleUrls: ['./navigation.component.scss']
})
export class NavigationComponent {
  isMenuCollapsed = true;
  
  constructor(private authService: AuthService) { }
  
  logout(): void {
    this.authService.logout();
  }
  
  get currentUser() {
    return this.authService.currentUserValue;
  }
  
  // Check user role for conditional rendering
  isPatient(): boolean {
    return this.currentUser?.role === 'patient';
  }
  
  isDoctor(): boolean {
    return this.currentUser?.role === 'doctor';
  }
  
  isAdmin(): boolean {
    return this.currentUser?.role === 'admin';
  }
}
```

```html
<!-- components/navigation/navigation.component.html -->
<nav class="navbar navbar-expand-lg navbar-light bg-white shadow">
  <div class="container-fluid">
    <a class="navbar-brand" routerLink="/dashboard">
      <i class="fas fa-heartbeat text-primary me-2"></i>
      <span class="fw-bold">Healthcare Portal</span>
    </a>
    
    <button 
      class="navbar-toggler" 
      type="button" 
      (click)="isMenuCollapsed = !isMenuCollapsed"
      aria-expanded="false" 
      aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    
    <div class="collapse navbar-collapse" [class.show]="!isMenuCollapsed">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link" routerLink="/dashboard" routerLinkActive="active">Dashboard</a>
        </li>
        
        <!-- Patient-specific menu items -->
        <li class="nav-item" *ngIf="isPatient()">
          <a class="nav-link" routerLink="/appointments" routerLinkActive="active">My Appointments</a>
        </li>
        <li class="nav-item" *ngIf="isPatient()">
          <a class="nav-link" routerLink="/book-appointment" routerLinkActive="active">Book Appointment</a>
        </li>
        
        <!-- Doctor-specific menu items -->
        <li class="nav-item" *ngIf="isDoctor()">
          <a class="nav-link" routerLink="/my-schedule" routerLinkActive="active">My Schedule</a>
        </li>
        <li class="nav-item" *ngIf="isDoctor()">
          <a class="nav-link" routerLink="/patient-records" routerLinkActive="active">Patient Records</a>
        </li>
        
        <!-- Admin-specific menu items -->
        <li class="nav-item" *ngIf="isAdmin()">
          <a class="nav-link" routerLink="/manage-doctors" routerLinkActive="active">Manage Doctors</a>
        </li>
        <li class="nav-item" *ngIf="isAdmin()">
          <a class="nav-link" routerLink="/manage-appointments" routerLinkActive="active">All Appointments</a>
        </li>
        <li class="nav-item" *ngIf="isAdmin()">
          <a class="nav-link" routerLink="/reports" routerLinkActive="active">Reports</a>
        </li>
        
        <!-- Common menu items -->
        <li class="nav-item">
          <a class="nav-link" routerLink="/profile" routerLinkActive="active">Profile</a>
        </li>
      </ul>
      
      <div class="d-flex align-items-center">
        <span class="me-3 d-none d-lg-block text-gray-600 small">
          Welcome, {{ currentUser?.name }} ({{ currentUser?.role }})
        </span>
        <button class="btn btn-outline-secondary" (click)="logout()">
          <i class="fas fa-sign-out-alt fa-sm fa-fw me-1"></i> Logout
        </button>
      </div>
    </div>
  </div>
</nav>
```

4. Enhanced Dashboard with Role-Based Views

```typescript
// components/dashboard/dashboard.component.ts
import { Component, OnInit } from '@angular/core';
import { Appointment } from '../../models/appointment.model';
import { User } from '../../models/user.model';
import { AppointmentService } from '../../services/appointment.service';
import { AuthService } from '../../services/auth.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: ['./dashboard.component.scss']
})
export class DashboardComponent implements OnInit {
  appointments: Appointment[] = [];
  allAppointments: Appointment[] = [];
  currentUser: User | null = null;
  isLoading: boolean = true;
  stats: any = {};

  constructor(
    private appointmentService: AppointmentService,
    private authService: AuthService
  ) { }

  ngOnInit(): void {
    this.currentUser = this.authService.currentUserValue;
    
    if (this.currentUser) {
      this.loadAppointments();
      this.calculateStats();
    }
  }

  loadAppointments(): void {
    this.isLoading = true;
    
    this.appointmentService.getAppointments().subscribe({
      next: (appointments) => {
        this.allAppointments = appointments;
        
        // Filter appointments based on user role
        if (this.currentUser?.role === 'patient') {
          this.appointments = appointments.filter(apt => apt.patientId === this.currentUser?.id);
        } else if (this.currentUser?.role === 'doctor') {
          this.appointments = appointments.filter(apt => apt.doctorId === this.currentUser?.id);
        } else {
          // Admin sees all appointments
          this.appointments = appointments;
        }
        
        this.isLoading = false;
      },
      error: () => {
        this.isLoading = false;
      }
    });
  }

  calculateStats(): void {
    if (this.currentUser?.role === 'doctor') {
      const myAppointments = this.allAppointments.filter(apt => apt.doctorId === this.currentUser?.id);
      this.stats = {
        total: myAppointments.length,
        confirmed: myAppointments.filter(apt => apt.status === 'confirmed').length,
        pending: myAppointments.filter(apt => apt.status === 'pending').length,
        cancelled: myAppointments.filter(apt => apt.status === 'cancelled').length
      };
    } else if (this.currentUser?.role === 'admin') {
      this.stats = {
        total: this.allAppointments.length,
        confirmed: this.allAppointments.filter(apt => apt.status === 'confirmed').length,
        pending: this.allAppointments.filter(apt => apt.status === 'pending').length,
        cancelled: this.allAppointments.filter(apt => apt.status === 'cancelled').length
      };
    }
  }

  getStatusBadgeClass(status: string): string {
    switch (status) {
      case 'confirmed':
        return 'bg-success';
      case 'pending':
        return 'bg-warning';
      case 'cancelled':
        return 'bg-danger';
      default:
        return 'bg-secondary';
    }
  }

  // Check user role for conditional rendering
  isPatient(): boolean {
    return this.currentUser?.role === 'patient';
  }
  
  isDoctor(): boolean {
    return this.currentUser?.role === 'doctor';
  }
  
  isAdmin(): boolean {
    return this.currentUser?.role === 'admin';
  }
}
```

```html
<!-- components/dashboard/dashboard.component.html -->
<app-navigation></app-navigation>

<div class="container-fluid mt-4">
  <div class="row">
    <div class="col-12">
      <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 class="h3 mb-0 text-gray-800">Dashboard</h1>
        <button *ngIf="isPatient()" class="d-none d-sm-inline-block btn btn-sm btn-primary shadow-sm" routerLink="/book-appointment">
          <i class="fas fa-calendar-plus fa-sm text-white-50"></i> New Appointment
        </button>
      </div>
      
      <!-- Welcome message -->
      <div class="row">
        <div class="col-12">
          <div class="card border-left-primary shadow h-100 py-2">
            <div class="card-body">
              <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                  <div class="text-xs font-weight-bold text-primary text-uppercase mb-1">
                    Welcome back, {{ currentUser?.name }}
                  </div>
                  <div class="h5 mb-0 font-weight-bold text-gray-800">
                    <span *ngIf="isPatient()">You have {{ appointments.length }} upcoming appointment(s)</span>
                    <span *ngIf="isDoctor()">You have {{ stats.total }} appointment(s) scheduled</span>
                    <span *ngIf="isAdmin()">There are {{ stats.total }} appointment(s) in the system</span>
                  </div>
                </div>
                <div class="col-auto">
                  <i class="fas fa-user-md fa-2x text-gray-300"></i>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Statistics for Doctors and Admins -->
      <div class="row mt-4" *ngIf="isDoctor() || isAdmin()">
        <!-- Confirmed Appointments Card -->
        <div class="col-xl-3 col-md-6 mb-4">
          <div class="card border-left-success shadow h-100 py-2">
            <div class="card-body">
              <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                  <div class="text-xs font-weight-bold text-success text-uppercase mb-1">Confirmed</div>
                  <div class="h5 mb-0 font-weight-bold text-gray-800">{{ stats.confirmed }}</div>
                </div>
                <div class="col-auto">
                  <i class="fas fa-check-circle fa-2x text-gray-300"></i>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Pending Appointments Card -->
        <div class="col-xl-3 col-md-6 mb-4">
          <div class="card border-left-warning shadow h-100 py-2">
            <div class="card-body">
              <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                  <div class="text-xs font-weight-bold text-warning text-uppercase mb-1">Pending</div>
                  <div class="h5 mb-0 font-weight-bold text-gray-800">{{ stats.pending }}</div>
                </div>
                <div class="col-auto">
                  <i class="fas fa-clock fa-2x text-gray-300"></i>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Cancelled Appointments Card -->
        <div class="col-xl-3 col-md-6 mb-4">
          <div class="card border-left-danger shadow h-100 py-2">
            <div class="card-body">
              <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                  <div class="text-xs font-weight-bold text-danger text-uppercase mb-1">Cancelled</div>
                  <div class="h5 mb-0 font-weight-bold text-gray-800">{{ stats.cancelled }}</div>
                </div>
                <div class="col-auto">
                  <i class="fas fa-times-circle fa-2x text-gray-300"></i>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Total Appointments Card -->
        <div class="col-xl-3 col-md-6 mb-4">
          <div class="card border-left-info shadow h-100 py-2">
            <div class="card-body">
              <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                  <div class="text-xs font-weight-bold text-info text-uppercase mb-1">Total</div>
                  <div class="h5 mb-0 font-weight-bold text-gray-800">{{ stats.total }}</div>
                </div>
                <div class="col-auto">
                  <i class="fas fa-calendar fa-2x text-gray-300"></i>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Appointments list -->
      <div class="row mt-4">
        <div class="col-12">
          <div class="card shadow mb-4">
            <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
              <h6 class="m-0 font-weight-bold text-primary">
                <span *ngIf="isPatient()">Your Appointments</span>
                <span *ngIf="isDoctor()">Your Patient Appointments</span>
                <span *ngIf="isAdmin()">All Appointments</span>
              </h6>
            </div>
            <div class="card-body">
              <div *ngIf="isLoading" class="text-center">
                <div class="spinner-border text-primary" role="status">
                  <span class="visually-hidden">Loading...</span>
                </div>
              </div>
              
              <div *ngIf="!isLoading && appointments.length === 0" class="text-center py-4">
                <i class="fas fa-calendar-times fa-3x text-gray-300 mb-3"></i>
                <p class="text-gray-500">
                  <span *ngIf="isPatient()">You don't have any appointments scheduled yet.</span>
                  <span *ngIf="isDoctor()">You don't have any patient appointments scheduled.</span>
                  <span *ngIf="isAdmin()">There are no appointments in the system.</span>
                </p>
                <button *ngIf="isPatient()" class="btn btn-primary" routerLink="/book-appointment">
                  <i class="fas fa-plus me-1"></i> Schedule Your First Appointment
                </button>
              </div>
              
              <div *ngIf="!isLoading && appointments.length > 0" class="table-responsive">
                <table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
                  <thead>
                    <tr>
                      <th>Date</th>
                      <th>Time</th>
                      <th *ngIf="isDoctor() || isAdmin()">Patient</th>
                      <th *ngIf="isPatient() || isAdmin()">Doctor</th>
                      <th>Reason</th>
                      <th>Status</th>
                      <th>Actions</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr *ngFor="let appointment of appointments">
                      <td>{{ appointment.date | date:'fullDate' }}</td>
                      <td>{{ appointment.time }}</td>
                      <td *ngIf="isDoctor() || isAdmin()">{{ appointment.patientName }}</td>
                      <td *ngIf="isPatient() || isAdmin()">{{ appointment.doctorName }}</td>
                      <td>{{ appointment.reason }}</td>
                      <td>
                        <span class="badge" [ngClass]="getStatusBadgeClass(appointment.status)">
                          {{ appointment.status | titlecase }}
                        </span>
                      </td>
                      <td>
                        <button class="btn btn-sm btn-info me-1">
                          <i class="fas fa-eye"></i>
                        </button>
                        <button *ngIf="isPatient() || isAdmin()" class="btn btn-sm btn-warning me-1">
                          <i class="fas fa-edit"></i>
                        </button>
                        <button *ngIf="isPatient() || isAdmin()" class="btn btn-sm btn-danger">
                          <i class="fas fa-trash"></i>
                        </button>
                        <button *ngIf="isDoctor() && appointment.status === 'pending'" class="btn btn-sm btn-success me-1">
                          <i class="fas fa-check"></i> Confirm
                        </button>
                        <button *ngIf="isDoctor()" class="btn btn-sm btn-secondary me-1">
                          <i class="fas fa-file-medical"></i> Notes
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

5. Updated Routing with Role-Based Routes

```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { LoginComponent } from './components/login/login.component';
import { DashboardComponent } from './components/dashboard/dashboard.component';
import { AuthGuard } from './guards/auth.guard';
import { RoleGuard } from './guards/role.guard';

const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'login', component: LoginComponent },
  { 
    path: 'dashboard', 
    component: DashboardComponent, 
    canActivate: [AuthGuard] 
  },
  { 
    path: 'book-appointment', 
    loadChildren: () => import('./modules/booking/booking.module').then(m => m.BookingModule),
    canActivate: [AuthGuard, RoleGuard],
    data: { roles: ['patient'] }
  },
  { 
    path: 'my-schedule', 
    loadChildren: () => import('./modules/schedule/schedule.module').then(m => m.ScheduleModule),
    canActivate: [AuthGuard, RoleGuard],
    data: { roles: ['doctor'] }
  },
  { 
    path: 'manage-doctors', 
    loadChildren: () => import('./modules/management/management.module').then(m => m.ManagementModule),
    canActivate: [AuthGuard, RoleGuard],
    data: { roles: ['admin'] }
  },
  { 
    path: 'manage-appointments', 
    loadChildren: () => import('./modules/management/management.module').then(m => m.ManagementModule),
    canActivate: [AuthGuard, RoleGuard],
    data: { roles: ['admin'] }
  },
  { path: '**', redirectTo: '/dashboard' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})Role-Based Healthcare Appointment System

I'll enhance the existing code to implement role-based access control (RBAC) for doctors, admins, and patients. This will include different views and functionality based on user roles.

Updated Implementation

1. Enhanced User Model and Data

First, let's update our db.json to include more comprehensive role-based data:

```json
{
  "users": [
    {
      "id": 1,
      "email": "patient@example.com",
      "password": "password123",
      "name": "John Doe",
      "role": "patient",
      "phone": "555-123-4567",
      "dateOfBirth": "1985-05-15"
    },
    {
      "id": 2,
      "email": "doctor@example.com",
      "password": "doctor123",
      "name": "Dr. Sarah Smith",
      "role": "doctor",
      "specialization": "Cardiology",
      "phone": "555-987-6543",
      "yearsOfExperience": 12
    },
    {
      "id": 3,
      "email": "admin@example.com",
      "password": "admin123",
      "name": "Admin User",
      "role": "admin",
      "phone": "555-000-0000"
    }
  ],
  "appointments": [
    {
      "id": 1,
      "patientId": 1,
      "patientName": "John Doe",
      "doctorId": 2,
      "doctorName": "Dr. Sarah Smith",
      "date": "2023-10-15",
      "time": "10:00 AM",
      "reason": "Regular checkup",
      "status": "confirmed",
      "notes": "Patient needs to fast before appointment"
    },
    {
      "id": 2,
      "patientId": 1,
      "patientName": "John Doe",
      "doctorId": 2,
      "doctorName": "Dr. Sarah Smith",
      "date": "2023-10-20",
      "time": "2:30 PM",
      "reason": "Follow-up appointment",
      "status": "pending"
    },
    {
      "id": 3,
      "patientId": 4,
      "patientName": "Mary Johnson",
      "doctorId": 2,
      "doctorName": "Dr. Sarah Smith",
      "date": "2023-10-18",
      "time": "9:00 AM",
      "reason": "Heart condition consultation",
      "status": "confirmed"
    }
  ],
  "doctors": [
    {
      "id": 2,
      "name": "Dr. Sarah Smith",
      "specialization": "Cardiology",
      "availableSlots": ["9:00 AM", "10:00 AM", "2:30 PM", "4:00 PM"]
    },
    {
      "id": 5,
      "name": "Dr. Michael Johnson",
      "specialization": "Pediatrics",
      "availableSlots": ["8:00 AM", "11:00 AM", "3:00 PM", "5:00 PM"]
    }
  ]
}
```

2. Enhanced User Model

```typescript
// models/user.model.ts
export interface User {
  id: number;
  email: string;
  password: string;
  name: string;
  role: 'patient' | 'doctor' | 'admin';
  phone?: string;
  dateOfBirth?: string;
  specialization?: string;
  yearsOfExperience?: number;
}
```

3. Role-Based Navigation Component

```typescript
// components/navigation/navigation.component.ts
import { Component } from '@angular/core';
import { AuthService } from '../../services/auth.service';

@Component({
  selector: 'app-navigation',
  templateUrl: './navigation.component.html',
  styleUrls: ['./navigation.component.scss']
})
export class NavigationComponent {
  isMenuCollapsed = true;
  
  constructor(private authService: AuthService) { }
  
  logout(): void {
    this.authService.logout();
  }
  
  get currentUser() {
    return this.authService.currentUserValue;
  }
  
  // Check user role for conditional rendering
  isPatient(): boolean {
    return this.currentUser?.role === 'patient';
  }
  
  isDoctor(): boolean {
    return this.currentUser?.role === 'doctor';
  }
  
  isAdmin(): boolean {
    return this.currentUser?.role === 'admin';
  }
}
```

```html
<!-- components/navigation/navigation.component.html -->
<nav class="navbar navbar-expand-lg navbar-light bg-white shadow">
  <div class="container-fluid">
    <a class="navbar-brand" routerLink="/dashboard">
      <i class="fas fa-heartbeat text-primary me-2"></i>
      <span class="fw-bold">Healthcare Portal</span>
    </a>
    
    <button 
      class="navbar-toggler" 
      type="button" 
      (click)="isMenuCollapsed = !isMenuCollapsed"
      aria-expanded="false" 
      aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    
    <div class="collapse navbar-collapse" [class.show]="!isMenuCollapsed">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link" routerLink="/dashboard" routerLinkActive="active">Dashboard</a>
        </li>
        
        <!-- Patient-specific menu items -->
        <li class="nav-item" *ngIf="isPatient()">
          <a class="nav-link" routerLink="/appointments" routerLinkActive="active">My Appointments</a>
        </li>
        <li class="nav-item" *ngIf="isPatient()">
          <a class="nav-link" routerLink="/book-appointment" routerLinkActive="active">Book Appointment</a>
        </li>
        
        <!-- Doctor-specific menu items -->
        <li class="nav-item" *ngIf="isDoctor()">
          <a class="nav-link" routerLink="/my-schedule" routerLinkActive="active">My Schedule</a>
        </li>
        <li class="nav-item" *ngIf="isDoctor()">
          <a class="nav-link" routerLink="/patient-records" routerLinkActive="active">Patient Records</a>
        </li>
        
        <!-- Admin-specific menu items -->
        <li class="nav-item" *ngIf="isAdmin()">
          <a class="nav-link" routerLink="/manage-doctors" routerLinkActive="active">Manage Doctors</a>
        </li>
        <li class="nav-item" *ngIf="isAdmin()">
          <a class="nav-link" routerLink="/manage-appointments" routerLinkActive="active">All Appointments</a>
        </li>
        <li class="nav-item" *ngIf="isAdmin()">
          <a class="nav-link" routerLink="/reports" routerLinkActive="active">Reports</a>
        </li>
        
        <!-- Common menu items -->
        <li class="nav-item">
          <a class="nav-link" routerLink="/profile" routerLinkActive="active">Profile</a>
        </li>
      </ul>
      
      <div class="d-flex align-items-center">
        <span class="me-3 d-none d-lg-block text-gray-600 small">
          Welcome, {{ currentUser?.name }} ({{ currentUser?.role }})
        </span>
        <button class="btn btn-outline-secondary" (click)="logout()">
          <i class="fas fa-sign-out-alt fa-sm fa-fw me-1"></i> Logout
        </button>
      </div>
    </div>
  </div>
</nav>
```

4. Enhanced Dashboard with Role-Based Views

```typescript
// components/dashboard/dashboard.component.ts
import { Component, OnInit } from '@angular/core';
import { Appointment } from '../../models/appointment.model';
import { User } from '../../models/user.model';
import { AppointmentService } from '../../services/appointment.service';
import { AuthService } from '../../services/auth.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: ['./dashboard.component.scss']
})
export class DashboardComponent implements OnInit {
  appointments: Appointment[] = [];
  allAppointments: Appointment[] = [];
  currentUser: User | null = null;
  isLoading: boolean = true;
  stats: any = {};

  constructor(
    private appointmentService: AppointmentService,
    private authService: AuthService
  ) { }

  ngOnInit(): void {
    this.currentUser = this.authService.currentUserValue;
    
    if (this.currentUser) {
      this.loadAppointments();
      this.calculateStats();
    }
  }

  loadAppointments(): void {
    this.isLoading = true;
    
    this.appointmentService.getAppointments().subscribe({
      next: (appointments) => {
        this.allAppointments = appointments;
        
        // Filter appointments based on user role
        if (this.currentUser?.role === 'patient') {
          this.appointments = appointments.filter(apt => apt.patientId === this.currentUser?.id);
        } else if (this.currentUser?.role === 'doctor') {
          this.appointments = appointments.filter(apt => apt.doctorId === this.currentUser?.id);
        } else {
          // Admin sees all appointments
          this.appointments = appointments;
        }
        
        this.isLoading = false;
      },
      error: () => {
        this.isLoading = false;
      }
    });
  }

  calculateStats(): void {
    if (this.currentUser?.role === 'doctor') {
      const myAppointments = this.allAppointments.filter(apt => apt.doctorId === this.currentUser?.id);
      this.stats = {
        total: myAppointments.length,
        confirmed: myAppointments.filter(apt => apt.status === 'confirmed').length,
        pending: myAppointments.filter(apt => apt.status === 'pending').length,
        cancelled: myAppointments.filter(apt => apt.status === 'cancelled').length
      };
    } else if (this.currentUser?.role === 'admin') {
      this.stats = {
        total: this.allAppointments.length,
        confirmed: this.allAppointments.filter(apt => apt.status === 'confirmed').length,
        pending: this.allAppointments.filter(apt => apt.status === 'pending').length,
        cancelled: this.allAppointments.filter(apt => apt.status === 'cancelled').length
      };
    }
  }

  getStatusBadgeClass(status: string): string {
    switch (status) {
      case 'confirmed':
        return 'bg-success';
      case 'pending':
        return 'bg-warning';
      case 'cancelled':
        return 'bg-danger';
      default:
        return 'bg-secondary';
    }
  }

  // Check user role for conditional rendering
  isPatient(): boolean {
    return this.currentUser?.role === 'patient';
  }
  
  isDoctor(): boolean {
    return this.currentUser?.role === 'doctor';
  }
  
  isAdmin(): boolean {
    return this.currentUser?.role === 'admin';
  }
}
```

```html
<!-- components/dashboard/dashboard.component.html -->
<app-navigation></app-navigation>

<div class="container-fluid mt-4">
  <div class="row">
    <div class="col-12">
      <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 class="h3 mb-0 text-gray-800">Dashboard</h1>
        <button *ngIf="isPatient()" class="d-none d-sm-inline-block btn btn-sm btn-primary shadow-sm" routerLink="/book-appointment">
          <i class="fas fa-calendar-plus fa-sm text-white-50"></i> New Appointment
        </button>
      </div>
      
      <!-- Welcome message -->
      <div class="row">
        <div class="col-12">
          <div class="card border-left-primary shadow h-100 py-2">
            <div class="card-body">
              <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                  <div class="text-xs font-weight-bold text-primary text-uppercase mb-1">
                    Welcome back, {{ currentUser?.name }}
                  </div>
                  <div class="h5 mb-0 font-weight-bold text-gray-800">
                    <span *ngIf="isPatient()">You have {{ appointments.length }} upcoming appointment(s)</span>
                    <span *ngIf="isDoctor()">You have {{ stats.total }} appointment(s) scheduled</span>
                    <span *ngIf="isAdmin()">There are {{ stats.total }} appointment(s) in the system</span>
                  </div>
                </div>
                <div class="col-auto">
                  <i class="fas fa-user-md fa-2x text-gray-300"></i>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Statistics for Doctors and Admins -->
      <div class="row mt-4" *ngIf="isDoctor() || isAdmin()">
        <!-- Confirmed Appointments Card -->
        <div class="col-xl-3 col-md-6 mb-4">
          <div class="card border-left-success shadow h-100 py-2">
            <div class="card-body">
              <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                  <div class="text-xs font-weight-bold text-success text-uppercase mb-1">Confirmed</div>
                  <div class="h5 mb-0 font-weight-bold text-gray-800">{{ stats.confirmed }}</div>
                </div>
                <div class="col-auto">
                  <i class="fas fa-check-circle fa-2x text-gray-300"></i>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Pending Appointments Card -->
        <div class="col-xl-3 col-md-6 mb-4">
          <div class="card border-left-warning shadow h-100 py-2">
            <div class="card-body">
              <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                  <div class="text-xs font-weight-bold text-warning text-uppercase mb-1">Pending</div>
                  <div class="h5 mb-0 font-weight-bold text-gray-800">{{ stats.pending }}</div>
                </div>
                <div class="col-auto">
                  <i class="fas fa-clock fa-2x text-gray-300"></i>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Cancelled Appointments Card -->
        <div class="col-xl-3 col-md-6 mb-4">
          <div class="card border-left-danger shadow h-100 py-2">
            <div class="card-body">
              <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                  <div class="text-xs font-weight-bold text-danger text-uppercase mb-1">Cancelled</div>
                  <div class="h5 mb-0 font-weight-bold text-gray-800">{{ stats.cancelled }}</div>
                </div>
                <div class="col-auto">
                  <i class="fas fa-times-circle fa-2x text-gray-300"></i>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Total Appointments Card -->
        <div class="col-xl-3 col-md-6 mb-4">
          <div class="card border-left-info shadow h-100 py-2">
            <div class="card-body">
              <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                  <div class="text-xs font-weight-bold text-info text-uppercase mb-1">Total</div>
                  <div class="h5 mb-0 font-weight-bold text-gray-800">{{ stats.total }}</div>
                </div>
                <div class="col-auto">
                  <i class="fas fa-calendar fa-2x text-gray-300"></i>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Appointments list -->
      <div class="row mt-4">
        <div class="col-12">
          <div class="card shadow mb-4">
            <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
              <h6 class="m-0 font-weight-bold text-primary">
                <span *ngIf="isPatient()">Your Appointments</span>
                <span *ngIf="isDoctor()">Your Patient Appointments</span>
                <span *ngIf="isAdmin()">All Appointments</span>
              </h6>
            </div>
            <div class="card-body">
              <div *ngIf="isLoading" class="text-center">
                <div class="spinner-border text-primary" role="status">
                  <span class="visually-hidden">Loading...</span>
                </div>
              </div>
              
              <div *ngIf="!isLoading && appointments.length === 0" class="text-center py-4">
                <i class="fas fa-calendar-times fa-3x text-gray-300 mb-3"></i>
                <p class="text-gray-500">
                  <span *ngIf="isPatient()">You don't have any appointments scheduled yet.</span>
                  <span *ngIf="isDoctor()">You don't have any patient appointments scheduled.</span>
                  <span *ngIf="isAdmin()">There are no appointments in the system.</span>
                </p>
                <button *ngIf="isPatient()" class="btn btn-primary" routerLink="/book-appointment">
                  <i class="fas fa-plus me-1"></i> Schedule Your First Appointment
                </button>
              </div>
              
              <div *ngIf="!isLoading && appointments.length > 0" class="table-responsive">
                <table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
                  <thead>
                    <tr>
                      <th>Date</th>
                      <th>Time</th>
                      <th *ngIf="isDoctor() || isAdmin()">Patient</th>
                      <th *ngIf="isPatient() || isAdmin()">Doctor</th>
                      <th>Reason</th>
                      <th>Status</th>
                      <th>Actions</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr *ngFor="let appointment of appointments">
                      <td>{{ appointment.date | date:'fullDate' }}</td>
                      <td>{{ appointment.time }}</td>
                      <td *ngIf="isDoctor() || isAdmin()">{{ appointment.patientName }}</td>
                      <td *ngIf="isPatient() || isAdmin()">{{ appointment.doctorName }}</td>
                      <td>{{ appointment.reason }}</td>
                      <td>
                        <span class="badge" [ngClass]="getStatusBadgeClass(appointment.status)">
                          {{ appointment.status | titlecase }}
                        </span>
                      </td>
                      <td>
                        <button class="btn btn-sm btn-info me-1">
                          <i class="fas fa-eye"></i>
                        </button>
                        <button *ngIf="isPatient() || isAdmin()" class="btn btn-sm btn-warning me-1">
                          <i class="fas fa-edit"></i>
                        </button>
                        <button *ngIf="isPatient() || isAdmin()" class="btn btn-sm btn-danger">
                          <i class="fas fa-trash"></i>
                        </button>
                        <button *ngIf="isDoctor() && appointment.status === 'pending'" class="btn btn-sm btn-success me-1">
                          <i class="fas fa-check"></i> Confirm
                        </button>
                        <button *ngIf="isDoctor()" class="btn btn-sm btn-secondary me-1">
                          <i class="fas fa-file-medical"></i> Notes
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

5. Updated Routing with Role-Based Routes

```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { LoginComponent } from './components/login/login.component';
import { DashboardComponent } from './components/dashboard/dashboard.component';
import { AuthGuard } from './guards/auth.guard';
import { RoleGuard } from './guards/role.guard';

const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'login', component: LoginComponent },
  { 
    path: 'dashboard', 
    component: DashboardComponent, 
    canActivate: [AuthGuard] 
  },
  { 
    path: 'book-appointment', 
    loadChildren: () => import('./modules/booking/booking.module').then(m => m.BookingModule),
    canActivate: [AuthGuard, RoleGuard],
    data: { roles: ['patient'] }
  },
  { 
    path: 'my-schedule', 
    loadChildren: () => import('./modules/schedule/schedule.module').then(m => m.ScheduleModule),
    canActivate: [AuthGuard, RoleGuard],
    data: { roles: ['doctor'] }
  },
  { 
    path: 'manage-doctors', 
    loadChildren: () => import('./modules/management/management.module').then(m => m.ManagementModule),
    canActivate: [AuthGuard, RoleGuard],
    data: { roles: ['admin'] }
  },
  { 
    path: 'manage-appointments', 
    loadChildren: () => import('./modules/management/management.module').then(m => m.ManagementModule),
    canActivate: [AuthGuard, RoleGuard],
    data: { roles: ['admin'] }
  },
  { path: '**', redirectTo: '/dashboard' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
