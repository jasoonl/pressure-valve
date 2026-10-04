# Pressure Valve

A venting bot for when something has you mad. Rant into the chat, watch the pressure gauge climb, then hold the release valve to blow the whole thing away.

- Pick a target ("Mad at: ...") or type your own.
- Three reply styles: **Take my side**, **Just listen**, **Cool me down**.
- Every message raises the gauge (caps, exclamation marks and angry words count for more). Above 120 PSI it hits the redline.
- Hold the valve for about a second to vent. The conversation disappears and you get a summary.
- Replies are built-in by default. Optionally turn on **Smart replies**: the page downloads a small open AI model (SmolLM2 360M, about 250 MB, cached by the browser) and runs it on your own device with [transformers.js](https://github.com/huggingface/transformers.js). Smart and built-in replies are mixed, every reply is tagged, and built-in lines fill in whenever the model is slow, fails, or says something unsuitable. Messages about self-harm or harming others never go to the model.
- Nothing is saved, and your messages are never sent to any server. It's one static page. (If you turn on Smart replies, your browser downloads the model files from jsDelivr and Hugging Face; your messages are not part of that.)

If you or someone you know is having a difficult time, free support is available. Call or text 988 (Suicide & Crisis Lifeline), text HOME to 741741 (Crisis Text Line), or call 911 for emergencies (US).

Live page: https://jasoonl.github.io/pressure-valve/
