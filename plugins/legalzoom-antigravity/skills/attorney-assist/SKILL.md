---
name: attorney-assist
description: Help request a phone consultation with an independent attorney through LegalZoom. Use when the user asks to speak with an attorney or accepts an attorney consultation recommendation after contract review. Requires connected LegalZoom MCP tools and eligible plan access.
---

# Request an attorney consultation

Use the connected LegalZoom MCP server's live tools. In this plugin the server is namespaced `legalzoom_legalzoom`; discover its current tool schemas before calling them. Tool names below identify operations, not a fixed host-specific invocation syntax. Follow the server's current instructions and returned guidance.

Consultations are provided by independent attorneys. Refer to “an attorney” or “the attorney,” rather than describing attorneys as LegalZoom employees. Do not claim an attorney-client relationship, privilege, full transcript delivery, or document delivery solely because this skill ran.

## Check access and gather the matter

1. Call `check_attorney_consultation_entitlements` using its discovered schema. The current tool takes no arguments and resolves the customer from authentication. Do not invent a customer or workspace ID.
2. If access cannot be verified, stop the service workflow. If the user lacks an eligible plan, explain the returned result without hardcoded prices or benefit promises. Offer the option to explore a plan or continue with AI-only assistance. Only call `add_bap_to_cart` if the user wants that action; use returned offer details and a checkout link rather than claiming a purchase was completed. Note that checkout requires signing in at legalzoom.com.
3. For an entitled user, get the workspace from `aiGuidance.workspaceIdForEscalation`. Ask what specific question the attorney should address and which US state or DC the legal matter concerns. Do not substitute the user's residence for the matter's jurisdiction.
4. Call `get_consultation_topics` with the returned account/workspace ID, and `get_valid_locations` to validate the matter's state. Use a topic verbatim from `consultationTopics`. If multiple topics fit, let the user choose; do not invent topics or claim unsupported specializations.

## Handle documents before scheduling

Ask whether the attorney needs to review documents. A contract read by Antigravity has not been delivered to the attorney.

If `upload_document` is available and the user wants to share a document, use it to obtain a secure browser upload link. Let the user upload the file and confirm completion; creating an upload session alone does not prove that the file was uploaded. Preserve each returned `lzfile:` reference for `request_attorney_review.lzFiles`. Do not transmit raw contract text in place of secure upload, fabricate file references, or upload unrelated local files.

If document upload is unavailable, explain the limitation and stop document-dependent scheduling until a supported sharing route is available. Do not invent a manual account URL or promise that the attorney will have the file.

## Present real availability

Call `get_attorney_availability` with the returned account ID, exact consultation topic, validated state, and the user's IANA timezone when known. For document review, upload first and pass `hasDocuments: true` so the service applies its review buffer. Explain the returned earliest date and document review lead time. If a slot was selected before documents were added, fetch availability again with the document flag.

Present returned attorneys by name, labeled “Licensed Attorney,” with their available dates, times, and timezone. Offer choices when the service returns them. Do not show internal topic codes or invent attorney names or time slots.

Let the user choose the attorney and time. Collect their callback phone number for this consultation. Do not assume a number on file or a timezone. Do not ask for passwords, payment card details, SSNs, or other unrelated sensitive information.

## Confirm the request and share relevant context

Before `request_attorney_review`, show the chosen attorney, date/time/timezone, matter, callback number, specific questions, and the summary, AI findings, and uploaded documents you intend to share. Include only relevant information from the current conversation. Wait for the user's approval of this request and context sharing.

Call `request_attorney_review` using the live schema:

- Copy `workspaceId` from the entitlement result and the exact `topic` from consultation topics; use the validated `location`.
- Set `contactMethod: scheduled_call`. Copy the slot's date/time, duration, attorney `lzUserId` as `expertId`, and `partnerId` into the required `scheduling` fields. Use the confirmed timezone and callback number. Never invent missing scheduling IDs.
- Populate `conversationSummary`, `specificQuestions`, and, when relevant, `aiAnalysis.keyFindings` and `aiAnalysis.recommendations` with the approved briefing. If including `aiAnalysis.riskLevel`, use a supported uppercase value from the live schema, not the contract skill's color labels.
- Include only confirmed uploaded references in `lzFiles` and indicate document review as required by the schema. Do not claim that the entire chat or original documents are automatically delivered.

Confirm only after an explicit successful tool result. Report the actual confirmation number/link, attorney and appointment details from the response, along with the context and documents actually accepted. Report partial attachment failures plainly. Do not fabricate a session ID, response time, calendar invite, or successful attachment.

Do not offer to add documents to a booked phone consultation or modify/reschedule an existing appointment. Direct the user to LegalZoom's account consultation management when a change is needed; do not cancel or create a replacement automatically.

## Errors and uncertain outcomes

- On missing tools, 401/403, or failed entitlement checks, stop and explain that service access could not be verified. Direct the user to Antigravity's MCP authentication settings or legalzoom.com. Do not guess plan access or retry credentials blindly.
- Correct invalid inputs only from the tool's error and real returned values. Do not switch topics or jurisdictions just to bypass an error.
- For a failed or timed-out booking, distinguish a confirmed failure from an uncertain outcome. Do not repeat `request_attorney_review` after a timeout: ask the user to check their LegalZoom consultations first to avoid duplicate bookings.
- Never simulate service responses or continue to dependent steps after a failure. Offer AI-only help where useful and label it clearly.

Explain that this is general information from LegalZoom, not legal advice; LegalZoom is not a law firm. An independent licensed attorney provides advice specific to the user's situation.
