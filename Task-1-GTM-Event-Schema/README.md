# Task 1 - GTM Event Schema

This document proposes a Google Tag Manager (GTM) event tracking strategy for the OrthoNow appointment booking journey and related user interactions.

## Overview

The objective of this tracking plan is to capture meaningful user interactions across the website so that the marketing and analytics teams can:

- Measure user engagement
- Identify booking funnel drop-offs
- Optimize conversion rates
- Build remarketing audiences
- Track campaign performance in GA4
- Import high-quality conversions into Google Ads

The event schema has been designed around the key user interactions mentioned in the assignment, with particular focus on the multi-step appointment booking journey.

## GTM Event Schema

| Event Name | Trigger Type | Key Parameters | GA4 Report / Audience |
|------------|-------------|----------------|------------------------|
| appointment_step_1_completed | Custom dataLayer Event | clinic_location, specialty, step_number | Funnel Exploration |
| appointment_step_2_completed | Custom dataLayer Event | phone_present, preferred_date, step_number | Funnel Exploration |
| appointment_step_3_completed | Custom dataLayer Event | booking_confirmation, appointment_type, step_number | Funnel Exploration |
| appointment_booking_success | Custom dataLayer Event | booking_id, clinic_location, specialty | Conversion Report, Google Ads |
| call_now_clicked | Click Trigger | page_name, clinic_location, button_position | Engagement Report |
| whatsapp_chat_opened | Click Trigger | page_name, device_type, source | Engagement Report |
| patient_guide_downloaded | Form Submit + File Download | guide_name, phone_present, download_type | Lead Generation Report |
| clinic_page_view | Page View Trigger | clinic_location, page_title, page_url | Pages & Screens Report |
| blog_scroll_50 | Scroll Trigger (50%) | article_title, scroll_percentage, page_url | Content Engagement |
| blog_scroll_90 | Scroll Trigger (90%) | article_title, scroll_percentage, page_url | Content Engagement |

## Booking Funnel Tracking

The appointment booking journey consists of three major steps followed by a successful booking confirmation.

The following events are pushed to the Data Layer whenever a user completes each stage of the booking flow.

### Step 1 – Clinic / Service Selection

```javascript
window.dataLayer.push({
    event: "appointment_step_1_completed",
    clinic_location: "Bengaluru",
    specialty: "Orthopaedics",
    step_number: 1
});
```

---

### Step 2 – Patient Information

```javascript
window.dataLayer.push({
    event: "appointment_step_2_completed",
    phone_present: true,
    preferred_date: "2026-07-05",
    step_number: 2
});
```

---

### Step 3 – Appointment Confirmation

```javascript
window.dataLayer.push({
    event: "appointment_step_3_completed",
    booking_confirmation: true,
    appointment_type: "Consultation",
    step_number: 3
});
```

---

### Final Booking Success

```javascript
window.dataLayer.push({
    event: "appointment_booking_success",
    booking_id: "BK123456",
    clinic_location: "Bengaluru",
    specialty: "Orthopaedics"
});
```

## GA4 Funnel Exploration

The booking events can be used to build a Funnel Exploration report in Google Analytics 4.

### Funnel Steps

1. appointment_step_1_completed
2. appointment_step_2_completed
3. appointment_step_3_completed
4. appointment_booking_success

Using this funnel, the marketing team can identify where users abandon the booking journey.

Example:

- High drop-off after Step 1 may indicate users are unable to find a suitable clinic or specialist.
- High drop-off after Step 2 may indicate the booking form is too long or difficult to complete.
- High drop-off after Step 3 may indicate scheduling or confirmation issues.

These insights help improve the booking experience and increase overall conversion rates.

## Google Ads Conversion Recommendation

### Recommended Conversion

**appointment_booking_success**

### Reason

This event represents a successfully completed appointment booking, making it the most valuable business conversion.

Importing this event into Google Ads allows the marketing team to:

- Measure campaign performance based on actual bookings.
- Optimize bidding strategies for high-quality leads.
- Improve Return on Ad Spend (ROAS).
- Build remarketing audiences using users who started but did not complete the booking process.

## Implementation Notes

- Google Tag Manager listens for custom `dataLayer.push()` events.
- GTM forwards these events to Google Analytics 4.
- Important conversions, such as successful appointment bookings, can then be imported into Google Ads.
- Personally identifiable information (PII), such as patient names or phone numbers, should not be sent to analytics platforms.
- Event names follow a consistent naming convention using lowercase letters and underscores for better maintainability.

## Conclusion

This GTM event schema provides a structured approach to tracking user interactions throughout the OrthoNow booking journey. The proposed events enable accurate conversion measurement, funnel analysis, audience creation, and campaign optimization while following analytics best practices such as consistent event naming and avoiding personally identifiable information (PII).