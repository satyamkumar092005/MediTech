# DocNow: Telemedicine Platform Documentation

## 1. Project Overview
DocNow is a full-featured telemedicine platform designed to connect patients with doctors for online consultations. It supports appointment booking, real-time AI-powered consultations, secure payments, and prescription management.

### Key Actors:
- **Patients**: Can search for doctors, book appointments, chat with AI, participate in live AI-assisted consultations, make payments, and view prescriptions.
- **Doctors**: Can manage their schedules, confirm/complete appointments, issue prescriptions, and view patient history and ratings.
- **Admins**: Can manage users, doctors, specializations, and moderate ratings.

---

## 2. Technology Stack

### Backend
- **Framework**: Laravel 13 (PHP 8.3)
- **Database**: PostgreSQL/MySQL (assumed standard)
- **Authentication**: Laravel Breeze (Inertia version) + Laravel Socialite (OAuth)
- **Permissions**: Spatie Laravel Permission (Role-based access control)
- **Real-time**: Laravel Reverb + Laravel Echo (WebSockets)
- **Payments**: Razorpay Integration
- **AI Integration**: Google Gemini (via HTTP API)
- **Speech/Audio**: Deepgram (Nova-3 for STT, Aura-2 for TTS)

### Frontend
- **Framework**: React 18 (with TypeScript)
- **State Management/Routing**: Inertia.js 2.0
- **Styling**: Tailwind CSS 4.0
- **UI Components**: Radix UI + Shadcn UI
- **Animations**: Framer Motion
- **Icons**: Lucide React

---

## 3. Core Features

### Patient Portal
- **Doctor Discovery**: Search doctors by name or specialization.
- **Appointment Booking**: Select date and time based on doctor availability.
- **Instant Appointments**: Quick booking for immediate needs.
- **AI Health Assistant**:
    - **Symptom Checker**: AI-powered initial assessment using Gemini.
    - **AI Chat**: Pre-consultation chat with a doctor-persona AI.
- **Live AI Consultation**: Real-time voice-based consultation powered by Deepgram and Gemini.
- **Payments**: Secure payment processing via Razorpay.
- **Prescription Tracking**: View and manage prescriptions issued by doctors.
- **Ratings**: Provide feedback and ratings for doctors after consultations.

### Doctor Portal
- **Schedule Management**: Set availability time slots for patients.
- **Appointment Workflow**: Confirm pending requests, start calls, and mark as completed.
- **Digital Prescriptions**: Create and issue prescriptions directly from the appointment details.
- **Patient History**: Access patient records and past appointments.

### Admin Dashboard
- **User Management**: Monitor all users and update roles.
- **Doctor Onboarding**: CRUD operations for doctor profiles and qualifications.
- **System Configuration**: Manage medical specializations.
- **Moderation**: Review and approve/delete patient ratings.

---

## 4. Database Architecture (Key Models)

| Model | Description |
|-------|-------------|
| **User** | Base identity model with standard profile fields (name, email, etc.). |
| **Doctor** | Extended profile for doctors (specialization, fee, bio). |
| **Patient** | Extended profile for patients (blood group, allergies). |
| **Appointment** | The core transaction model linking Patient, Doctor, and Schedule. Stores status, meeting links, and payment IDs. |
| **Specialization** | Categories of medical expertise (e.g., Cardiology, Pediatrics). |
| **Schedule** | Time slots created by doctors for booking. |
| **Prescription** | Records medical advice and items issued during an appointment. |
| **Rating** | Patient feedback and numerical score for doctor performance. |
| **Coupon** | Discount codes that can be applied to appointment fees. |

---

## 5. Important Files & Directories

### Backend (app/)
- **`app/Http/Controllers/`**:
    - `GeminiController.php`: Handles AI logic (Chat and Symptom Check) using Google's Gemini Pro.
    - `DeepgramController.php`: Manages tokens and configurations for real-time voice consultations.
    - `PaymentController.php`: Integrates Razorpay for order creation and payment verification.
    - `AppointmentController.php`: The "brain" of the app, handling the lifecycle of consultations.
- **`app/Services/`**:
    - `CalendarService.php`: Generates `.ics` files for calendar synchronization.
- **`app/Events/`**:
    - `AppointmentStatusUpdated.php`: Broadcasts real-time updates to patients and doctors via Reverb.

### Frontend (resources/js/)
- **`resources/js/Pages/`**: Organized by role (Admin, Doctor, Patient).
    - `Patient/Doctors/Show.tsx`: The booking interface.
    - `Patient/Appointments/Call.tsx`: The live AI consultation interface.
- **`resources/js/components/ui/`**: Reusable Shadcn UI components (Buttons, Inputs, Modals).

### Configuration & Routes
- **`routes/web.php`**: Defines all web routes, grouped by middleware/roles.
- **`config/services.php`**: Stores credentials for Gemini, Razorpay, and Deepgram.
- **`vite.config.js`**: Frontend build configuration using the Vite plugin for Laravel.

---

## 6. External Integrations

### Google Gemini
Used for natural language processing. The app uses the `gemini-3-flash-preview` model for fast, context-aware medical assistant responses.

### Deepgram
Used for the Voice AI feature.
- **STT (Speech-to-Text)**: `nova-3` model for high-accuracy transcription.
- **TTS (Text-to-Speech)**: `aura-2-thalia-en` for natural-sounding voice output.

### Razorpay
Handles all financial transactions. It uses a server-side order creation followed by client-side checkout and server-side verification.

---

## 7. Setup & Installation

1. **Clone & Dependencies**:
   ```bash
   composer install
   npm install
   ```
2. **Environment Configuration**:
   - Copy `.env.example` to `.env`.
   - Set database credentials.
   - Add API keys for: `GEMINI_API_KEY`, `RAZORPAY_KEY`, `RAZORPAY_SECRET`, `DEEPGRAM_API_KEY`.
3. **Database Setup**:
   ```bash
   php artisan key:generate
   php artisan migrate --seed
   ```
4. **Running the Application**:
   ```bash
   # Starts server, queue, logs, and vite in parallel
   npm run dev
   ```

---

## 8. Real-time Features
The project uses **Laravel Reverb** for low-latency communication.
- **Private Channels**: `user.{id}` for personalized notifications.
- **Broadcast Events**: Used for instant appointment status updates (Pending -> Confirmed -> Completed).

---

## 9. Email Notifications
The system sends automated emails for key events:
- **Appointment Lifecycle**: Booking confirmation, cancellations, and status updates.
- **Medical Records**: Sending prescriptions directly to patients' emails.
- **Security**: OTP (One-Time Password) for login/verification.

All mailables are located in `app/Mail/` and use Blade templates in `resources/views/emails/`.

---

## 10. Testing & Quality Assurance
The project includes a suite of Feature and Unit tests:
- **Feature Tests**: Validate end-to-end workflows like appointment booking, payment verification, and role-based access.
- **Unit Tests**: Focus on isolated logic like calendar ICS generation.

Run tests using:
```bash
php artisan test
```

---
*Documentation generated on May 22, 2026.*
