A modern web-based AI image generation tool built using **HTML**, **CSS**, and **JavaScript** with integration to the **Replica API**. The application allows users to enter creative prompts, choose generation models, select aspect ratios, specify image count, and generate high‑quality AI images. The UI includes theme switching, random prompt suggestions, a responsive gallery, and image download options.

---

## 🚀 Features are:-

* **Beautiful UI / UX** with responsive layout and gradient theme
* **Light & Dark theme toggle** with preference saved in `localStorage`
* **Prompt randomizer** using pre-defined high-quality prompts
* **Model selection**, **image count**, and **aspect ratio** options
* **Parallel image generation** via Hugging Face Inference API
* **Loading states** and **error messages** per image
* **Image download button** on each generated output
* **Smart dimension scaling** ensuring model-compatible resolutions

---

## 🛠️ Tech Stack Used:-

* **Frontend:** HTML5, CSS3, Vanilla JavaScript (ES6+)
* **Styling:** Flexbox, responsive design, Font Awesome icons
* **API:** Hugging Face Inference API (text-to-image models)
* **Development:** Local static file server (VSCode Live Server / http-server)

---

## 📂 Project Structure

```
/project-root
│── index.html        # Main UI structure
│── style.css         # Complete styling
│── app.js            # Core logic (API, UI, events)
│── README.md         # Project documentation
└── assets/           # Optional folder for images/icons
```

---

## 📜 How It Works:-

### 1. User Input

The user enters a text prompt, selects a model, chooses image count and aspect ratio.

### 2. UI Validation

JavaScript ensures all required fields are filled.

### 3. Placeholder Cards

A gallery grid is dynamically created with loading animations.

### 4. Dimension Calculation

Aspect ratio (e.g., `16/9`) is scaled and adjusted to multiples of 16 (required by many models).

### 5. API Request

A POST request is sent to the Replica endpoint:

```
https://api-inference.huggingface.co/models/{MODEL_NAME}
```

The payload includes:

* prompt (`inputs`)
* width & height (`parameters`)
* cache bypass flag

### 6. Result Handling

The response is a **Blob** (image file). It is converted into a temporary URL and displayed inside its corresponding gallery card.

### 7. Download Option

Each image includes a download button with timestamps to avoid name collisions.

---

## 🔐 API Key & Security Note

> ⚠️ **Never expose private API keys in client-side code for production use.**

For testing it’s acceptable, but real deployments should use a **backend proxy**.

### Example Node.js Proxy

```js
const express = require('express');
const fetch = require('node-fetch');
const app = express();
app.use(express.json());

const HF_KEY = process.env.HF_KEY;

app.post('/api/generate', async (req, res) => {
  const url = `https://api-inference.huggingface.co/models/${req.body.model}`;
  const response = await fetch(url, {
    method: 'POST',
    headers: { Authorization: `Bearer ${HF_KEY}`, 'Content-Type': 'application/json' },
    body: JSON.stringify(req.body.payload)
  });
  const buffer = await response.arrayBuffer();
  res.set('Content-Type', response.headers.get('content-type') || 'image/png');
  res.send(Buffer.from(buffer));
});

app.listen(3000);
```

Use this to protect your key.

---

## 🧪 Testing Checklist

* [ ] Form validation prevents empty submissions
* [ ] Model selection works correctly
* [ ] Loading cards appear instantly on Generate
* [ ] Images generate and display correctly
* [ ] Download button works
* [ ] Theme toggle persists theme across reloads
* [ ] Errors display properly if inference fails
* [ ] API call visible in browser DevTools → Network

---

## ⚠️ Known Issues

* Direct browser calls may fail due to **CORS** restrictions on some Hugging Face models.
* Slow generation / timeouts may occur on heavy models.
* Some models return JSON with base64 instead of blobs (these may require decoder modification).

---

## 📌 Future Enhancements

* Add history of previous prompts
* Add a gallery view with pagination
* Add platform authentication for user‑saved images
* Add more advanced model settings (guidance scale, steps, CFG)
* Add drag‑and‑drop image input for image‑to‑image models
* Migrate to a frontend framework (React / Vue) if scaling

---

## 📄 License

This project is open for educational and non‑commercial use. Modify freely for your internal submissions.

---

If you need this README converted to **PDF**, **DOCX**, or formatted for **GitHub**, just tell me!
