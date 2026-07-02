# Task 3 - Integration Design

## Overview

The landing page collects the user's name and phone number when they request a consultation. After the form is submitted, the data should be processed automatically so the marketing team, CRM, and WhatsApp notification system all stay in sync.

## Integration Flow

1. The user submits the consultation form.
2. The frontend sends the form data to a backend API.
3. The backend validates the request and sends the data to HubSpot using the HubSpot CRM API.
4. Before creating a new contact, the backend searches HubSpot using the submitted phone number. Since HubSpot does not deduplicate contacts by phone by default, this step prevents duplicate records.
5. If the contact already exists, it is updated. Otherwise, a new contact is created with:
   - Name
   - Phone Number
   - Clinic Preference
   - Source = Google Ads - Consultation Landing Page
   - Lead Status = New Enquiry
6. After the contact is successfully created or updated in HubSpot, the backend calls the Karix WhatsApp Business API to send a confirmation message.
7. At the same time, Google Tag Manager sends the `consultation_form_submitted` event to GA4, and the conversion is imported into Google Ads for campaign optimisation.

## Biggest Failure Point

The biggest risk is creating duplicate contacts in HubSpot because the form does not collect an email address. HubSpot normally uses email for deduplication, so relying only on the default behaviour could create multiple records for the same patient.

To avoid this, the backend should first search HubSpot using the submitted phone number. If a matching contact is found, update the existing record instead of creating a new one.

## WhatsApp SLA (Within 2 Minutes)

The WhatsApp message depends on both the backend and the Karix API being available. Network issues, API failures, or temporary service outages could delay message delivery.

To monitor this, the backend should log every WhatsApp request and response. Failed requests should be recorded, and an automatic retry mechanism should attempt delivery again after a short delay. Monitoring API response times and failed requests will help ensure the 2-minute SLA is consistently met.

## Why I Chose This Approach

I chose direct API integration instead of using automation tools because it provides better control over validation, error handling, and contact management. It also makes the integration easier to maintain as the application grows.