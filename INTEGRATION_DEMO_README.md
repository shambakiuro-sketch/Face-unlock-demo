# Face Unlock Integration Demo

A complete working example showing how to integrate the Face Unlock Service into a web app.

## 🎯 What This Does

Shows a **split-screen app**:
- **Left side**: Your protected app (initially locked)
- **Right side**: The Face Unlock Service embedded
- **Center**: Communication between them

When you enroll/verify your face on the right, the left side **unlocks automatically**.

---

## 🚀 Deploy to Vercel (5 Minutes)

### Option 1: Simple (Recommended)

1. Open this file in your browser
2. Save as `index.html`
3. Create a GitHub repo with just this file
4. Connect to Vercel
5. Done!

### Option 2: Full Setup

1. Create a folder: `face-unlock-demo`
2. Put `integration-demo.html` inside as `index.html`
3. Create `package.json`:

```json
{
  "name": "face-unlock-demo",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "build": "echo 'No build needed'",
    "start": "python3 -m http.server 3000"
  }
}
```

4. Push to GitHub
5. Connect to Vercel
6. Set **Output Directory** to `.` (current folder)

---

## 📱 How to Use

### On Desktop or Mobile:

1. **Open the app** (left side shows 🔒 LOCKED)
2. **Click "Unlock with Face"** button
3. **On the right side**, click "Enroll Face" first
4. **Allow camera** when prompted
5. **Capture your face** (click Capture button)
6. **Click "Verify Face"**
7. **Allow camera** and verify
8. **Left side unlocks** when successful ✅

---

## 🔌 How Integration Works

### Simple Explanation:

```
Your App (LEFT)              Face Unlock Service (RIGHT)
    |                              |
    |--- Click "Unlock" ---------> |
    |                              |
    |                    User enrolls/verifies face
    |                              |
    |<---- Success Message --------|
    |
    ✅ App Unlocks!
```

### Technical Flow:

1. **Your app** opens Face Unlock in an iframe
2. **User enrolls face** in the service
3. **Face Unlock Service** sends message via `postMessage` API:
   ```javascript
   window.parent.postMessage({ 
     type: 'FACE_UNLOCK_SUCCESS', 
     confidence: 0.95 
   }, '*');
   ```
4. **Your app receives** the message:
   ```javascript
   window.addEventListener('message', (e) => {
     if (e.data.type === 'FACE_UNLOCK_SUCCESS') {
       unlockApp();  // Your unlock logic
     }
   });
   ```

---

## 🔧 Customize for Your App

### Change the Protected Content:

Edit the left side section in the HTML:

```html
<div class="protected-app">
    <h3>🎮 Secret Dashboard</h3>
    <p>Your app content here</p>
</div>
```

### Change the App Name:

```html
<h2>🏠 Your Protected App</h2>
<p>Your app description</p>
```

### Add Your Own Unlock Logic:

In the `unlockApp()` function:

```javascript
function unlockApp() {
    // Your logic here
    console.log('App is now unlocked!');
    // Can redirect, show content, etc.
}
```

---

## 🌐 For Android Integration

This demo shows the **exact same integration** Android apps need:

### Android equivalent:

```java
// This WebView loads the face unlock service
WebView webView = new WebView(this);
webView.loadUrl("https://face-unlock-app-henna.vercel.app");

// Listen for messages (like the postMessage in this demo)
webView.addJavascriptInterface(new FaceUnlockBridge(this), "Android");

// When verification succeeds:
// → Android receives message
// → Unlocks the app
```

The concept is **identical** — just in native Android code instead of JavaScript.

---

## 📋 Features

✅ Split-screen view (app + service)  
✅ Real-time logging of events  
✅ Status indicator (locked/unlocked)  
✅ Lock/unlock buttons  
✅ Mobile responsive  
✅ Dark theme (matches face unlock service)  
✅ Works entirely client-side  
✅ No backend needed  

---

## 🔐 Security Notes

- Face data is stored only in the Face Unlock Service's IndexedDB
- This demo has **no real security** (just shows flow)
- For production, add proper authentication
- Verify unlock confidence score (currently hardcoded at 0.95)

---

## 📲 Test It Yourself

1. **Deploy to Vercel**
2. **Open on phone** (mobile browser)
3. **Click "Unlock with Face"**
4. **Enroll your face** once
5. **Lock the app** (red button)
6. **Verify your face** to unlock
7. **See the split-screen communication**

---

## 🆘 Troubleshooting

| Issue | Fix |
|-------|-----|
| Face Unlock service shows blank | Wait 30s, reload page |
| Camera not working | Grant permission when prompted |
| Left side not updating | Check browser console for errors |
| Service loads but nothing happens | Check that iframe can access vercel.app |

---

## 💡 Next Steps

1. **Test this demo** with your face
2. **Understand the postMessage flow** (left ↔ right communication)
3. **Apply same logic to Android** (WebView + message bridge)
4. **Build your protected app** with real authentication

---

## 📝 Files

- `integration-demo.html` - Complete demo (this file)
- `INTEGRATION_GUIDE.md` - Android implementation details
- `https://face-unlock-app-henna.vercel.app` - Live Face Unlock Service

---

**Your Face Unlock Service is ready. This demo shows exactly how to use it.** 🔓
