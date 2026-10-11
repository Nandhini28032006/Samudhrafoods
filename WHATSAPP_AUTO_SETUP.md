# SAMUDHRA FOODS — Automatic WhatsApp Setup

The web app is prepared to automatically send a WhatsApp Business message after a quotation or invoice is successfully saved.

## What is wired in the app
- Saving a quotation calls the Supabase Edge Function `send-whatsapp` automatically.
- Saving an invoice calls the same function automatically.
- The automatic WhatsApp message includes the customer name, document number, date, itemized bill/quotation summary, and total.
- The message template is designed as **full English first, then full Tamil**.
- Emoji are included as real Unicode characters (for example: 🙏 ❤️ ⭐ 🤝 🧾 📋) in the approved WhatsApp template. WhatsApp Cloud API sends UTF-8 text; do not replace these with ASCII question marks.
- The browser never stores the WhatsApp access token.
- If WhatsApp is not configured, the document remains saved in Supabase and the app reports the messaging configuration error.

## Important: WhatsApp sends the bill/quotation details in the message
The automatic message contains the itemized details, not just a generic notification. The current integration sends five template variables in this order:

1. Customer name
2. Invoice / Quotation number
3. Date
4. Item summary (one line per item)
5. Grand total

Example item summary:
`1. Idli x 2 = ₹20.00`
`2. Dosa x 1 = ₹40.00`

## Create the two approved WhatsApp templates
In Meta WhatsApp Manager, create/approve one invoice template and one quotation template. Both templates should use **five body variables in exactly this order**.

### Invoice template body

🙏 Hello from SAMUDHRA FOODS!

Thank you for choosing SAMUDHRA FOODS. ❤️

Dear {{1}},

🧾 Your bill / invoice has been prepared successfully.
Invoice No: {{2}}
Date: {{3}}

Items:
{{4}}

💰 Total: {{5}}

We hope you enjoyed your food and had a wonderful experience with us.

⭐ Your feedback matters to us!
Please take a moment to rate your experience and share your valuable feedback. Your suggestions help us improve our food and service.

We truly appreciate your support and look forward to serving you again! 🤝

Thank you for dining with SAMUDHRA FOODS! ❤️

SAMUDHRA FOODS
📞 +91 81222 17228
📸 @SAMUDHRAFOODS

--- தமிழ் ---

🙏 SAMUDHRA FOODS சார்பாக வணக்கம்!

SAMUDHRA FOODS-ஐ தேர்வு செய்ததற்கு மிக்க நன்றி. ❤️

உங்கள் பில் / Invoice வெற்றிகரமாக தயாரிக்கப்பட்டுள்ளது.
Invoice No: {{2}}
தேதி: {{3}}

உணவு விவரங்கள்:
{{4}}

💰 மொத்தம்: {{5}}

எங்கள் உணவை ருசித்து, எங்களுடைய சேவையை மகிழ்ச்சியுடன் அனுபவித்திருப்பீர்கள் என்று நம்புகிறோம்.

⭐ உங்கள் கருத்து எங்களுக்கு மிகவும் முக்கியம்!
உங்கள் அனுபவத்திற்கு ஒரு Rating வழங்கி, உங்களுடைய மதிப்புமிக்க கருத்தைப் பகிர்ந்து கொள்ளுங்கள். உங்கள் கருத்துகள் எங்கள் உணவு மற்றும் சேவையை மேலும் மேம்படுத்த உதவும்.

உங்கள் ஆதரவிற்கு மனமார்ந்த நன்றி. மீண்டும் உங்களை வரவேற்க ஆவலுடன் காத்திருக்கிறோம்! 🤝

SAMUDHRA FOODS-ல் உணவருந்தியதற்கு நன்றி! ❤️

SAMUDHRA FOODS
📞 +91 81222 17228
📸 @SAMUDHRAFOODS

### Quotation template body

🙏 Hello from SAMUDHRA FOODS!

Thank you for considering SAMUDHRA FOODS for your requirements. ❤️

Dear {{1}},

📋 Your quotation has been prepared based on your requirements.
Quotation No: {{2}}
Date: {{3}}

Items:
{{4}}

💰 Total: {{5}}

Please review the quotation and let us know if you would like to make any changes or have any questions.

We are happy to assist you and look forward to serving you. 🤝

Your trust means a lot to us!
Thank you for giving SAMUDHRA FOODS the opportunity to serve you.

We look forward to serving you soon! ❤️

SAMUDHRA FOODS
📞 +91 81222 17228
📸 @SAMUDHRAFOODS

--- தமிழ் ---

🙏 SAMUDHRA FOODS சார்பாக வணக்கம்!

உங்கள் தேவைகளுக்காக SAMUDHRA FOODS-ஐ பரிசீலித்ததற்கு மிக்க நன்றி. ❤️

உங்கள் தேவைகளுக்கு ஏற்ப Quotation தயாரிக்கப்பட்டுள்ளது.
Quotation No: {{2}}
தேதி: {{3}}

உணவு விவரங்கள்:
{{4}}

💰 மொத்தம்: {{5}}

தயவுசெய்து Quotation-ஐ சரிபார்த்து பாருங்கள். ஏதேனும் மாற்றங்கள் அல்லது கேள்விகள் இருந்தால் எங்களை தொடர்பு கொள்ளுங்கள்.

உங்களுக்கு தேவையான உதவிகளை வழங்க நாங்கள் மகிழ்ச்சியுடன் தயாராக இருக்கிறோம். உங்களுக்கு சிறந்த சேவையை வழங்க ஆவலுடன் காத்திருக்கிறோம். 🤝

எங்கள் மீது நீங்கள் வைத்துள்ள நம்பிக்கை எங்களுக்கு மிகவும் முக்கியம்!
SAMUDHRA FOODS உங்களுக்கு சேவை செய்யும் வாய்ப்பை வழங்கியதற்கு மிக்க நன்றி.

விரைவில் உங்களுக்கு சேவை செய்ய ஆவலுடன் காத்திருக்கிறோம்! ❤️

SAMUDHRA FOODS
📞 +91 81222 17228
📸 @SAMUDHRAFOODS

## Supabase Edge Function secrets
Set these secrets for the project:

- `WHATSAPP_ACCESS_TOKEN` = WhatsApp API access token
- `WHATSAPP_PHONE_NUMBER_ID` = WhatsApp sender phone number ID
- `META_GRAPH_VERSION` = Graph API version supported by the WhatsApp Business Platform account
- `WHATSAPP_INVOICE_TEMPLATE` = approved invoice template name
- `WHATSAPP_QUOTATION_TEMPLATE` = approved quotation template name
- `WHATSAPP_TEMPLATE_LANGUAGE` = template language code (for example `en_US` if that is the approved template language)

Then deploy `supabase/functions/send-whatsapp` to the same Supabase project.

**Never** put the access token in `config.js`, HTML, JavaScript, GitHub, or any public frontend file.

## If emojis appear as boxes/question marks
That normally means the WhatsApp template itself was created with incorrect encoding or the template version does not contain the emoji characters. Re-create/edit the approved template using the exact UTF-8 text above and make sure the template language matches the configured `WHATSAPP_TEMPLATE_LANGUAGE`.
