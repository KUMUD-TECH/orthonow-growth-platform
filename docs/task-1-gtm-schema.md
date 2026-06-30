# Task 1 – GTM Event Schema
## Project Overview

The objective of this implementation is to establish a comprehensive Google Tag Manager (GTM) and Google Analytics 4 (GA4) tracking framework for OrthoNow. The tracking strategy captures the complete patient acquisition journey—from the first page visit through consultation booking—to enable accurate reporting, funnel analysis, audience creation, and Google Ads optimization.

## GTM Event Schema

| Event Name | Trigger Type | Trigger Condition | Key Parameters  | GA4 Report | GA4 Audience |
|------------|-------------------------|------------------------|----------------------------|-------------------|------------------|
| `page_view` | Page View | Every page load | `page_name`, `page_type`, `page_url` | Pages & Screens | All Website Visitors |
| `landing_page_view` | Page View | Consultation landing page is loaded | `page_name`, `campaign_source`, `device_type` | Landing Page Report | Google Ads Visitors |
| `clinic_page_view` | Page View | User visits any clinic location page | `clinic_location`, `city`, `page_url` | Pages & Screens | Users Interested in Specific Clinics |
| `blog_article_view` | Page View | User opens a blog article | `article_title`, `article_category`, `author` | Content Performance | Content Readers |
| `scroll_depth` | Scroll Trigger | User reaches 25%, 50%, 75%, or 100% scroll depth | `scroll_percentage`, `page_name`, `article_title` | Engagement Report | Highly Engaged Users |
| `cta_clicked` | Click Trigger | User clicks the primary CTA button | `button_text`, `button_location`, `page_name` | Events Report | CTA Engaged Users |
| `call_now_clicked` | Click Trigger | User clicks any "Call Now" button | `clinic_location`, `button_location`, `page_name` | Events Report | High Intent Users |
| `whatsapp_chat_started` | Click Trigger | User clicks the WhatsApp chat widget | `button_location`, `page_name`, `device_type` | Events Report | WhatsApp Engaged Users |
| `patient_guide_form_started` | Custom Event | User starts filling the Patient Guide form | `guide_name`, `page_name`, `device_type` | Funnel Exploration | Guide Interested Users |
| `patient_guide_form_submitted` | Form Submission | Patient Guide form submitted successfully | `guide_name`, `clinic_location`, `traffic_source` | Lead Generation Report | Lead Magnet Users |
| `patient_guide_downloaded` | Link Click | PDF download starts successfully | `guide_name`, `file_name`, `file_type` | File Download Report | Guide Downloaders |
| `booking_started` | Custom Event | User enters Step 1 of booking | `clinic_location`, `specialty`, `page_name` | Funnel Exploration | Booking Started Users |
| `booking_step_completed` | Custom Event | User successfully completes any booking step | `step_number`, `step_name`, `clinic_location` | Funnel Exploration | Booking Progress Users |
| `booking_completed` | Custom Event | User confirms booking successfully | `booking_id`, `clinic_location`, `specialty` | Conversions Report | Converted Patients |
| `booking_abandoned` | Custom Event | User leaves booking before completion | `last_completed_step`, `clinic_location`, `session_id` | Funnel Exploration | Booking Abandoners |
| `form_validation_error` | Custom Event | Validation fails during booking | `field_name`, `error_type`, `step_number` | Events Report | Form Error Users |
| `thank_you_page_view` | Virtual Page View | Booking success state is displayed | `booking_id`, `clinic_location`, `traffic_source` | Pages & Screens | Converted Patients |
| `session_engaged` | Timer / Engagement | Session exceeds GA4 engagement threshold | `engagement_time`, `page_type`, `device_type` | Engagement Report | Engaged Sessions |
| `outbound_link_click` | Click Trigger | User clicks an external link | `link_url`, `link_text`, `page_name` | Events Report | External Link Users |
| `consultation_form_submitted` | Custom Event | Consultation landing page form submitted successfully | `clinic_location`, `traffic_source`, `campaign_name` | Conversions Report | Qualified Leads |



## Booking Form Funnel Tracking

The booking form consists of three sequential steps. Since it is a multi-step form, Google Tag Manager cannot reliably detect step transitions automatically. The frontend application must push a custom event to the `window.dataLayer` whenever a user successfully completes a step. GTM listens for these events using Custom Event Triggers and forwards them to GA4.

| Booking Step | Frontend dataLayer Event | GTM Trigger | GTM Trigger Condition | GA4 Event | GA4 Funnel Condition | Purpose |
|--------------|--------------------------|-------------|-----------------------|------------|----------------------|---------|
| Step 1 – Select Clinic & Specialty | `booking_step_completed` | Custom Event | Event equals `booking_step_completed` and `step_number = 1` | `booking_step_completed` | `step_number = 1` | Measure users who successfully select a clinic location and specialty. |
| Step 2 – Enter Patient Details | `booking_step_completed` | Custom Event | Event equals `booking_step_completed` and `step_number = 2` | `booking_step_completed` | `step_number = 2` | Measure users who successfully complete the patient information step. |
| Step 3 – Confirm Booking | `booking_completed` | Custom Event | Event equals `booking_completed` | `booking_completed` | Final conversion step | Measure successfully confirmed consultation bookings. |

