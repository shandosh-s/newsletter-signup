# 📋 LOVABLE.DEV PROMPT FOR NEWSLETTER SUBSCRIPTION PAGE

## How to Use This Prompt:
1. Go to https://lovable.dev
2. Create new project
3. Copy EVERYTHING below the line and paste into Lovable chat
4. Wait for generation
5. Update the webhook URL in the code
6. Deploy!

---
## COPY FROM HERE ⬇️
---

Create a beautiful newsletter subscription page called "Daily Chronicle" with these specifications:

## App Overview
A single-page newsletter subscription form with a classic newspaper aesthetic. Users can enter their name, email, and select topics they're interested in.

## Design Requirements

### Color Scheme
- Primary: Navy blue (#1a365d)
- Accent: Gold (#d4af37)
- Background: White (#ffffff)
- Text: Dark gray (#333333)

### Typography
- Import Google Fonts: Playfair Display (headers) and Source Sans Pro (body)
- Headers: Playfair Display, serif
- Body: Source Sans Pro, sans-serif

## Page Structure

### Header Section
Create a header with:
- Navy blue background (#1a365d)
- Large title "★ DAILY CHRONICLE ★" in white, uppercase, letter-spacing: 4px
- Subtitle "Your Personalized News Digest" in smaller gray text
- A gold gradient line (4px height) below the header

### Features Section  
Display 3 feature cards in a horizontal row:
1. 📰 "Multi-Source News" - "Reddit, Google, Yahoo, YouTube, HackerNews"
2. 📖 "Page-Flip Reader" - "Beautiful e-newspaper format"
3. ⏰ "Daily Delivery" - "Fresh news every morning"

Each card should have a light gray background, centered content, and the emoji as a large icon.

### Subscription Form
Create a form with:

1. **Name Input**
   - Label: "Your Name"
   - Placeholder: "Enter your full name"
   - Required field

2. **Email Input**
   - Label: "Email Address"  
   - Placeholder: "you@example.com"
   - Required field, email validation

3. **Topics Selection**
   - Label: "Select Your Topics (choose at least one)"
   - Display as a 2-column grid of clickable cards
   - Each card shows: emoji icon, topic name, checkmark when selected
   - Topics to include:
     - 💻 Technology
     - 🔬 Science
     - 📈 Business
     - 🏥 Health
     - ⚽ Sports
     - 🎬 Entertainment
   - Cards should have a light background, border when selected
   - Clicking toggles selection

4. **Custom Topic Input**
   - Optional text field
   - Placeholder: "Or add a custom topic (e.g., AI, Climate)"

5. **Submit Button**
   - Full width
   - Gold gradient background
   - Text: "📬 SUBSCRIBE NOW"
   - Hover effect: slight lift and shadow

### Footer
- Light gray background
- Small text: "By subscribing, you agree to receive daily newsletters. Unsubscribe anytime."
- Copyright: "© 2024 Daily Chronicle"

## Functionality Requirements

### State Management
Use React useState for:
- name (string)
- email (string)
- selectedTopics (array of strings)
- customTopic (string)
- isLoading (boolean)
- message (object: {type: 'success'|'error', text: string})

### Form Validation
Before submit, validate:
- Name is at least 2 characters
- Email contains @ symbol
- At least one topic is selected (from checkboxes OR custom topic)

### API Integration
On form submit, send POST request:

```javascript
const handleSubmit = async (e) => {
  e.preventDefault();
  setIsLoading(true);
  setMessage(null);
  
  // Combine selected topics with custom topic
  const allTopics = [...selectedTopics];
  if (customTopic.trim()) {
    allTopics.push(customTopic.trim());
  }
  
  // Validation
  if (name.length < 2) {
    setMessage({type: 'error', text: 'Please enter your name'});
    setIsLoading(false);
    return;
  }
  if (!email.includes('@')) {
    setMessage({type: 'error', text: 'Please enter a valid email'});
    setIsLoading(false);
    return;
  }
  if (allTopics.length === 0) {
    setMessage({type: 'error', text: 'Please select at least one topic'});
    setIsLoading(false);
    return;
  }
  
  try {
    // IMPORTANT: Replace this URL with your N8N webhook URL
    const response = await fetch('YOUR_N8N_WEBHOOK_URL', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        name: name,
        email: email,
        topics: allTopics
      })
    });
    
    const data = await response.json();
    
    if (data.success) {
      setMessage({type: 'success', text: '🎉 Welcome! Check your email for confirmation.'});
      // Reset form
      setName('');
      setEmail('');
      setSelectedTopics([]);
      setCustomTopic('');
    } else {
      setMessage({type: 'error', text: data.error || 'Something went wrong'});
    }
  } catch (error) {
    setMessage({type: 'error', text: 'Connection error. Please try again.'});
  } finally {
    setIsLoading(false);
  }
};
```

### Topic Card Click Handler
```javascript
const toggleTopic = (topic) => {
  setSelectedTopics(prev => 
    prev.includes(topic) 
      ? prev.filter(t => t !== topic)
      : [...prev, topic]
  );
};
```

### Message Display
Show success messages with green background (#d4edda, border #c3e6cb)
Show error messages with red background (#f8d7da, border #f5c6cb)

### Loading State
When isLoading is true:
- Show a spinner
- Disable the submit button
- Show "Subscribing..." text

## Responsive Design
- On mobile (< 640px): 
  - Feature cards stack vertically
  - Topic grid becomes single column
  - Reduce padding

## Additional Notes
- Use Tailwind CSS for styling
- Make it production-ready
- Ensure accessibility (proper labels, focus states)
- Add smooth transitions on hover/focus

Generate the complete React component with all the above specifications.

---
## COPY UNTIL HERE ⬆️
---

## After Generation - UPDATE THE WEBHOOK URL!

Find this line in the generated code:
```javascript
fetch('YOUR_N8N_WEBHOOK_URL',
```

Replace `YOUR_N8N_WEBHOOK_URL` with your actual N8N webhook URL, for example:
```javascript
fetch('https://your-n8n-instance.app/webhook/subscribe',
```

Then click Deploy in Lovable!
