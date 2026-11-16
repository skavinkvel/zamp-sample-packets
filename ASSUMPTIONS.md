# Vendor onboarding (PS-2): assumptions and limits

Kavin Keerthivel Shanmugavel, 17 Nov 2026. Everything below describes what is built and what I assumed. Demo data is fictional.

## What the process does
- A vendor submission (8 typed fields and up to 3 documents: PAN card, GST certificate, cancelled cheque or bank letter) runs through 8 stages and ends in exactly one of three outcomes: **approved, pending or rejected**, with the reasons stored.
- **Claude only reads the documents** and returns structured fields. **Fixed rules make every decision**, and every sentence the vendor would see comes from fixed templates. (This is a refinement of my first email, which said the model "explains".)
- **Fail-closed:** the decision step compares the results against a fixed list of 20 expected checks. A missing, skipped or failed-to-run check means pending, never approved.
- Reject only when the evidence is verified and conclusive (for example, the PAN inside the GSTIN differs from the PAN card for the same type of holder). Anything fixable or uncertain is pending, with a message saying what is needed.
- Every run is logged and a finished run is never edited. The Dashboard shows the history.

## Assumptions
1. **India first.** Vendors outside India go to pending ("not supported yet") for human onboarding; no document checks are applied to them.
2. **Name matching:** names are normalised and compared with a similarity score; at or above 0.85 passes, between 0.60 and 0.85 goes to a human, below 0.60 fails. These thresholds are my assumptions, tuned only on the sample packets.
3. **A partly readable document counts as unreadable.** There is no numeric confidence score from the reader.
4. **Vendor identity is the PAN** (one vendor per PAN; a second GSTIN under the same PAN is the same vendor).
5. **Bank identity is account number plus the first 4 characters of the IFSC** (the bank code). Two unverified premises: many banks keep the account number when an account moves branch, and two banks sharing a prefix is very unlikely.
6. **Duplicates are checked against approved vendors only.** A new submission that matches an earlier rejected one on PAN, GSTIN or bank account is flagged for the internal team and goes to pending.
7. **No email is sent in this version.** The message to the vendor is shown on the Run tab and stored.

## What is verified, and what is not
- **GSTIN structure and the state-code table:** checked against the official GST e-invoice master codes (read 12 Nov 2026). The state-code mapping is deliberately lenient for codes 25 and 26.
- **GSTIN check character:** verified empirically against 120 real GSTINs. I found no official text that states the rule. A GSTIN with a wrong check character is rejected when the typed and the certificate values agree, so this rule rests on that empirical check.
- **13th and 14th GSTIN characters:** not checked.
- **IFSC shape:** 11 characters, first 4 letters, fifth a zero (RBI FAQ). **IFSC existence** comes from Razorpay's public lookup, a third-party mirror of RBI data, not from RBI itself. The three sample IFSC codes were checked on 14 Nov 2026.
- **Not verified:** the name-match thresholds, and how the document reader behaves on real-world scans. The sample documents are born-digital PDFs, except one deliberately blurry phone photo (P4).

## Not built
- Document download from the Run tab (it lists file names and sizes only).
- A resubmission loop (a corrected submission starts a new run), a reviewer decision step, and sanctions screening.
- Access control on the page: it runs on my own server with sample data only.
