# OrthoNow Growth Platform

## Task 02 – Landing Page Build

A high-converting landing page designed for the **"Get an Expert Orthopaedic Opinion"** campaign.

The landing page targets working professionals (28–50 years) in Bengaluru experiencing knee or back pain and focuses on maximizing consultation bookings through a clean conversion-focused design.

---

## Features

- Responsive landing page built with HTML, CSS and Vanilla JavaScript
- Conversion-focused hero section
- Trust indicators (Patient Rating, Clinics, Patients Treated)
- Consultation booking form
- Client-side validation
- GTM-compatible `dataLayer.push()` implementation
- Thank-you state without page reload
- Mobile responsive layout
- Optimized for Core Web Vitals

---

## Tech Stack

- HTML5
- CSS3
- Vanilla JavaScript
- Google Tag Manager Data Layer

---

## GTM Event

```javascript
window.dataLayer.push({
  event: "consultation_form_submitted",
  clinic_location: "...",
  specialty: "...",
  form_name: "consultation_form",
  lead_type: "consultation_request",
  phone_provided: true
});
```

---

## Screenshots

### Landing Page

![Landing Page](screenshots/landing-page.png)

### Mobile View

![Mobile View](screenshots/mobile-view.png)

### Thank You State

![Thank You](screenshots/thank-you-state.png)

### GTM Data Layer

![Console](screenshots/datalayer-console.png)

### Lighthouse Mobile

![Lighthouse](screenshots/lighthouse-mobile.png)

---

## How to Run

1. Clone the repository.
2. Open `index.html` in any modern browser.
3. Fill the consultation form.
4. Verify the thank-you state and `dataLayer` event in the browser console.