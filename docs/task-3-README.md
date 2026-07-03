# Task 03 – Integration Design

## End-to-End Integration Architecture

When a patient submits the consultation form, the landing page sends an HTTPS POST request to a Node.js (Express) backend API. I would use a backend instead of directly calling HubSpot because it provides better security, validation, logging, retry mechanisms, and keeps API keys protected.

The backend first validates the submitted data (Name, Phone, Clinic Preference, and Specialty). It then uses the HubSpot CRM Search API to search for an existing contact using the phone number. This is important because HubSpot's default deduplication works on email, while this landing page only collects a phone number. If a matching phone number exists, the backend updates the existing contact; otherwise, it creates a new contact with the following properties:

- Name
- Phone
- Clinic Preference
- Source = Google Ads – Consultation Landing Page
- Lead Status = New Enquiry

After a successful HubSpot response, the backend records the consultation_form_submitted conversion using the existing Google Ads/GTM measurement setup. Finally, instead of calling Karix synchronously, the backend places a WhatsApp notification job into a background queue. A worker service processes the queue and sends the confirmation message through the Karix WhatsApp Business API, allowing the user to receive an immediate success response while the message is delivered asynchronously.

The biggest failure point in this workflow is the dependency on external services such as HubSpot or Karix. If either API becomes unavailable, the lead should never be lost. To prevent this, failed requests are stored in a message queue and automatically retried using exponential backoff. Every retry and failure is logged, and alerts are generated if repeated failures occur, ensuring that no enquiry is missed.

To guarantee the WhatsApp confirmation is delivered within the required 2-minute SLA, I would monitor queue length, message processing time, Karix API response times, and failed delivery attempts. Automated alerts would trigger whenever queue delays or API latency approach the two-minute threshold, allowing the operations team to investigate before the SLA is breached. This architecture is scalable, fault-tolerant, and ensures reliable CRM synchronization, timely patient communication, and accurate Google Ads conversion tracking.