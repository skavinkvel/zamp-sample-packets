# Vendor onboarding: SAMPLE test packets

Test documents for a vendor-onboarding process (Zamp AI Solutions Associate case study, Problem Statement 2). The link to the live app is in my submission email.

**Everything here is made up for testing.** Names, PAN, GSTIN, account numbers, emails and phone numbers are fictional, and every PDF is stamped SAMPLE. The IFSC codes are real, public bank-branch codes; the account numbers are not.

## How to try a packet

1. Open the live app and stay on the **Submit** tab.
2. Choose a packet in **Demo helper**. It fills the typed fields (the same values are in each `packet.json`).
3. Attach the packet's files from the folder here: the PAN card, the GST certificate, and the cancelled cheque or bank letter. (P6 has no files.)
4. Press submit and watch the **Run** tab. The **Dashboard** tab keeps the history.

## What each packet should do

| Packet | What it tests | Expected result |
|---|---|---|
| **P1** | India happy path | approved |
| **P1b** | A second happy path (own PAN and bank account) | approved |
| **P2** | The PAN inside the GSTIN differs from the PAN card | rejected |
| **P3** | The bank account holder's name differs from the legal name | pending (needs a human to review) |
| **P4** | The PAN card is a blurry phone photo (`pan_card_photo.jpg`: attach it as the PAN card) | pending (ask for a clearer upload) |
| **P5** | Same PAN as P1 but a different bank account; run it after P1 is approved | pending (bank change: callback needed) |
| **P6** | Vendor outside India, no files | pending (country not supported yet) |
| **P8** | Corrected resubmission of P2; run it after P2 has been rejected | pending (flagged: matches an earlier rejected submission) |

## Good to know

- **The app remembers approved vendors.** P1 is approved the first time. Run it again and it comes back pending as a duplicate; that is the duplicate check working. If P1 was already used, try P1b. If both were used, email me and I will reset the vendor list.
- **Who decides what:** Claude reads the documents. Fixed rules make the decision and write every sentence the vendor would see. No AI model decides anything.
- **No email is sent.** The message to the vendor is shown on the Run tab and stored.
