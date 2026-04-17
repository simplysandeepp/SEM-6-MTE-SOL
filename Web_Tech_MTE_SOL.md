# Web Technology – Complete Practice Set Solutions

---

## UNIT 1: Internet, Web Basics & HTML5

---

### 1. Define Internet and WWW

**Internet**
The Internet is a global network of interconnected computers communicating via standard protocols (mainly TCP/IP). It is the underlying infrastructure that includes websites, emails, FTP, online gaming, APIs, etc.

**World Wide Web (WWW)**
The WWW is a service built on top of the Internet that allows access to web pages via browsers. It uses HTTP/HTTPS to deliver content.

| | Internet | WWW |
|---|---|---|
| What it is | Network infrastructure | Service on the network |
| Analogy | Roads + cables + routers | Websites you open (Google, YouTube) |

---

### 2. What is HTTP?

**HTTP (HyperText Transfer Protocol)** is a communication protocol used between client (browser) and server.

- Defines how data is requested and transferred
- **Stateless** – each request is independent

**HTTP Methods:**

| Method | Purpose |
|--------|---------|
| GET | Fetch data |
| POST | Send data |
| PUT | Update data |
| DELETE | Remove data |

**Example Flow:**
```
Browser → GET /index.html → Server → HTML/CSS/JSON Response
```

---

### 3. Define Web Server

A **Web Server** is a system (hardware + software) that:
- Stores websites
- Processes HTTP requests
- Sends responses back to clients

**Examples:** Apache HTTP Server, Nginx, Node.js

**Flow:** Receives request → Processes logic (via backend) → Sends response (HTML/JSON)

---

### 4. What is DNS?

**DNS (Domain Name System)** is the phonebook of the Internet. It converts human-readable domain names to IP addresses.

```
google.com  →  142.250.xxx.xxx
```

**Flow:**
1. You type `google.com`
2. DNS resolves it to an IP
3. Browser connects to that IP

**Why needed?** Humans remember names; computers use IPs.

---

### 5. Define Client–Server Model

A system where:
- **Client** → Requests services (browser, mobile app)
- **Server** → Provides services (backend, database)

**Example:** Chrome browser (client) ↔ Website server (server)

---

### 6. Explain Client–Server Architecture with Diagram

**Flow:**
1. Client sends HTTP request
2. Server processes request
3. Server sends response
4. Client displays result

**Diagram:**
```
[Tablet]
[Desktop Browser]  ──── Request ────►  [Backend Server]
[Mobile]           ◄─── Response ───
```

**DNS + Web Server Full Flow:**
```
User → DNS Server (resolves IP) → Web Server → HTTP Response with Data → User
```

**Real JS Example:**
```javascript
// Client (React)
fetch('/api/data')

// Server (Node.js)
app.get('/api/data', (req, res) => {
  res.json({ message: "Hello" });
});
```

**Connecting the Dots:**
```
Internet → Infrastructure
DNS      → Finds server IP
HTTP     → Communication protocol
Client   → Requests
Server   → Responds
```

---

### 7. What is a Static and Dynamic Web Page?

**Static Web Page**
- Pre-built, always shows same content
- Written in HTML, CSS only
- No server-side processing

```html
<h1>Welcome</h1>
<p>This content never changes</p>
```

**Dynamic Web Page**
- Generated at runtime based on user/data
- Uses JavaScript, backend (Node.js, PHP, etc.)
- Content changes per user, time, database

```javascript
app.get('/user', (req, res) => {
  res.send("Hello " + req.query.name);
});
```

| Feature | Static | Dynamic |
|---------|--------|---------|
| Content | Fixed | Changing |
| Backend | Not needed | Required |
| Speed | Fast | Slightly slower |
| Example | Portfolio | Instagram |

---

### 8. Explain HTTP Request and Response Structure

**HTTP Request:**
```
METHOD /path HTTP/1.1
Host: example.com
Content-Type: application/json

{ "name": "Sandeep" }
```

Parts:
1. **Request Line** → Method + URL + HTTP Version
2. **Headers** → Metadata (Content-Type, Host, etc.)
3. **Body** → Data (optional, used in POST/PUT)

**HTTP Response:**
```
HTTP/1.1 200 OK
Content-Type: application/json

{ "message": "Success" }
```

Parts:
1. **Status Line** → HTTP version + status code
2. **Headers**
3. **Body**

**Common Status Codes:**

| Code | Meaning |
|------|---------|
| 200 | OK |
| 404 | Not Found |
| 500 | Internal Server Error |

---

### 9. Explain HTTP Methods (GET, POST) with Examples

**GET Method** – Used to fetch data
- No body; data sent via URL
- Safe (no modification)

```javascript
fetch('/users?id=10')

app.get('/users', (req, res) => {
  res.send("User ID: " + req.query.id);
});
```

**POST Method** – Used to send/create data
- Has body; data hidden from URL

```javascript
fetch('/users', {
  method: 'POST',
  body: JSON.stringify({ name: "Sandeep" }),
  headers: { "Content-Type": "application/json" }
});

app.post('/users', (req, res) => {
  res.send("User created");
});
```

| Feature | GET | POST |
|---------|-----|------|
| Purpose | Fetch data | Send data |
| Data location | URL | Body |
| Cacheable | Yes | No |

---

### 10. Explain XML Structure and DTD

**XML (Extensible Markup Language)** – Used to store and transport data.

```xml
<user>
  <name>Sandeep</name>
  <age>21</age>
</user>
```

Rules: Custom tags allowed, must be properly nested, case-sensitive.

**DTD (Document Type Definition)** – Defines the structure/rules of XML.

```xml
<!DOCTYPE user [
  <!ELEMENT user (name, age)>
  <!ELEMENT name (#PCDATA)>
  <!ELEMENT age (#PCDATA)>
]>
```

This means: `<user>` must contain `name` and `age` elements.

---

### 11. Explain XHTML Syntax Rules

**XHTML = HTML + strict XML rules**

| Rule | Example |
|------|---------|
| Proper closing | `<br />` |
| Lowercase tags | `<p>Text</p>` |
| Proper nesting | `<b><i>Text</i></b>` |
| Attributes need values | `<input type="text" />` |
| Single root element | `<html>...</html>` |

| Feature | HTML | XHTML |
|---------|------|-------|
| Syntax | Flexible | Strict |
| Tag case | Case-insensitive | Lowercase required |
| Empty tags | `<br>` | `<br />` |

---

### 12. Static Website with Multiple Pages (Home, About, Contact)

**Folder Structure:**
```
/website
  ├── index.html
  ├── about.html
  └── contact.html
```

**index.html (Home):**
```html
<!DOCTYPE html>
<html>
<head><title>Home</title></head>
<body>
  <h1>Welcome to My Website</h1>
  <nav>
    <a href="index.html">Home</a> |
    <a href="about.html">About</a> |
    <a href="contact.html">Contact</a>
  </nav>
  <p>This is the home page.</p>
</body>
</html>
```

**about.html:**
```html
<!DOCTYPE html>
<html>
<head><title>About</title></head>
<body>
  <h1>About Us</h1>
  <nav>
    <a href="index.html">Home</a> |
    <a href="contact.html">Contact</a>
  </nav>
  <p>We are learning HTML5.</p>
</body>
</html>
```

**contact.html:**
```html
<!DOCTYPE html>
<html>
<head><title>Contact</title></head>
<body>
  <h1>Contact</h1>
  <nav>
    <a href="index.html">Home</a> |
    <a href="about.html">About</a>
  </nav>
  <p>Email: example@mail.com</p>
</body>
</html>
```

---

### 13. HTML5 Canvas and SVG

**Canvas (pixel-based, JS-driven):**
```html
<canvas id="myCanvas" width="200" height="100"></canvas>
<script>
  const c = document.getElementById("myCanvas");
  const ctx = c.getContext("2d");
  ctx.fillStyle = "blue";
  ctx.fillRect(20, 20, 150, 50);
</script>
```

**SVG (vector-based, HTML-native):**
```html
<svg width="200" height="200">
  <circle cx="100" cy="100" r="50" fill="red" />
</svg>
```

| Feature | Canvas | SVG |
|---------|--------|-----|
| Driven by | JavaScript | HTML |
| Type | Pixel (raster) | Vector |
| Best for | Games, animations | Icons, graphics |

---

### 14. HTML5 Form with Validation

```html
<!DOCTYPE html>
<html>
<body>
<form>
  Name:     <input type="text" required><br><br>
  Email:    <input type="email" required><br><br>
  Password: <input type="password" minlength="6" required><br><br>
  Age:      <input type="number" min="18" max="60"><br><br>
  Phone:    <input type="tel" pattern="[0-9]{10}" required><br><br>
  <input type="submit">
</form>
</body>
</html>
```

**Key Validation Attributes:**

| Attribute | Purpose |
|-----------|---------|
| `required` | Field must be filled |
| `type="email"` | Checks email format |
| `min`, `max` | Number range |
| `minlength` | Minimum character count |
| `pattern` | Regex validation |

---

### 15. Multimedia Webpage with Audio, Video, Semantic Tags

```html
<!DOCTYPE html>
<html>
<body>
<header>
  <h1>My Multimedia Page</h1>
</header>

<section>
  <h2>Audio</h2>
  <audio controls>
    <source src="audio.mp3" type="audio/mpeg">
  </audio>
</section>

<section>
  <h2>Video</h2>
  <video width="300" controls>
    <source src="video.mp4" type="video/mp4">
  </video>
</section>

<footer>
  <p>© 2026 My Site</p>
</footer>
</body>
</html>
```

Semantic tags used: `<header>`, `<section>`, `<footer>` → improves SEO + readability.

---

### 16. Multi-Page HTML5 College Website

**Folder Structure:**
```
/college-site
  ├── index.html
  ├── courses.html
  ├── admissions.html
  └── contact.html
```

**index.html:**
```html
<!DOCTYPE html>
<html>
<body>
<header><h1>ABC College</h1></header>
<nav>
  <a href="index.html">Home</a>
  <a href="courses.html">Courses</a>
  <a href="admissions.html">Admissions</a>
  <a href="contact.html">Contact</a>
</nav>
<section>
  <h2>Welcome</h2>
  <p>Top college for engineering.</p>
</section>
<footer><p>© ABC College</p></footer>
</body>
</html>
```

**courses.html:**
```html
<h2>Courses</h2>
<ul>
  <li>B.Tech</li>
  <li>M.Tech</li>
  <li>BCA</li>
</ul>
```

**admissions.html:**
```html
<h2>Admissions</h2>
<p>Apply online through our portal.</p>
```

**contact.html:**
```html
<h2>Contact</h2>
<p>Email: college@mail.com</p>
```

---

## UNIT 2: CSS and Bootstrap

---

### 1. Types of CSS with Examples

CSS can be applied in three main ways:

**a) Inline CSS** – Applied directly using `style` attribute.
```html
<p style="color: blue; font-size: 20px;">Hello World</p>
```
- Highest priority, not reusable, hard to maintain.

**b) Internal CSS** – Written inside `<style>` tag in `<head>`.
```html
<head>
  <style>
    p { color: green; font-size: 18px; }
  </style>
</head>
```
- Good for single-page use.

**c) External CSS** – Written in a separate `.css` file.

`style.css`:
```css
p { color: red; font-size: 18px; }
```

`index.html`:
```html
<head>
  <link rel="stylesheet" href="style.css">
</head>
```
- Best for large projects, fully reusable.

| Type | Location | Reusable | Best For |
|------|----------|----------|----------|
| Inline | Inside element | No | Quick one-time changes |
| Internal | `<style>` tag | Limited | Single page |
| External | Separate file | Yes | Large websites |

---

### 2. CSS Selectors with Examples

**a) Element selector** – All `<p>` tags:
```css
p { color: blue; }
```

**b) Class selector** – Elements with class `box`:
```css
.box { background: yellow; }
```
```html
<div class="box">Content</div>
```

**c) ID selector** – Unique element:
```css
#header { color: white; }
```

**d) Group selector** – Multiple selectors at once:
```css
h1, h2, p { font-family: Arial; }
```

**e) Descendant selector** – `<p>` inside `<div>`:
```css
div p { color: purple; }
```

**f) Child selector** – Direct children only:
```css
div > p { color: orange; }
```

**g) Universal selector** – All elements:
```css
* { margin: 0; padding: 0; }
```

**h) Attribute selector:**
```css
input[type="text"] { border: 1px solid black; }
```

---

### 3. Difference Between class, id, and Element Selectors

| Selector | Syntax | Can Repeat? | Specificity |
|----------|--------|-------------|-------------|
| Element | `p` | Yes | Low (1) |
| Class | `.box` | Yes | Medium (10) |
| ID | `#main` | No (ideally) | High (100) |

**Example – Specificity in action:**
```html
<p class="text" id="unique">Hello</p>
```
```css
p { color: blue; }
.text { color: green; }
#unique { color: red; }
```
**Result: Red** – ID wins due to highest specificity.

---

### 4. Bootstrap Grid System and Responsive Design

Bootstrap uses a **12-column layout system** that adapts to different screen sizes.

**Basic Example:**
```html
<div class="container">
  <div class="row">
    <div class="col-6">Left</div>
    <div class="col-6">Right</div>
  </div>
</div>
```

**Responsive Breakpoints:**

| Class | Screen Size |
|-------|-------------|
| `col-` | Extra small |
| `col-sm-` | Small |
| `col-md-` | Medium |
| `col-lg-` | Large |
| `col-xl-` | Extra large |

**Example (adaptive columns):**
```html
<div class="col-md-6 col-lg-4">Box</div>
```
- On medium: 6 columns wide
- On large: 4 columns wide

**Importance:** Makes websites mobile-friendly, reduces custom CSS, speeds up development.

---

### 5. CSS Specificity

**Specificity** decides which CSS rule wins when multiple rules target the same element.

**Priority Order (lowest to highest):**
1. Element selector → `1`
2. Class selector → `10`
3. ID selector → `100`
4. Inline CSS → `1000`

**Example:**
```css
p { color: blue; }        /* specificity: 1 */
.text { color: green; }   /* specificity: 10 */
#para1 { color: red; }    /* specificity: 100 */
```
```html
<p class="text" id="para1">Hello</p>
```
**Result: Red** – ID has highest specificity.

---

### 6. CSS Box Model with Example

Every HTML element is a rectangular box with 4 layers:

```
┌─────────────────────────┐
│         Margin          │
│  ┌───────────────────┐  │
│  │      Border       │  │
│  │  ┌─────────────┐  │  │
│  │  │   Padding   │  │  │
│  │  │ ┌─────────┐ │  │  │
│  │  │ │ Content │ │  │  │
│  │  │ └─────────┘ │  │  │
│  │  └─────────────┘  │  │
│  └───────────────────┘  │
└─────────────────────────┘
```

```css
.box {
  width: 200px;     /* content width */
  padding: 20px;    /* inner space */
  border: 5px solid black;
  margin: 10px;     /* outer space */
}
```

**Pro tip – use `box-sizing: border-box`** to include padding and border in width:
```css
* { box-sizing: border-box; }
```

---

### 7. Flexbox with Example

**Flexbox** is a CSS one-dimensional layout system for aligning and distributing items.

```html
<div class="container">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
</div>
```
```css
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

**Key Properties:**

*On parent:*
- `display: flex`
- `justify-content` (main axis alignment)
- `align-items` (cross axis alignment)
- `flex-direction`, `gap`

*On children:*
- `flex-grow`, `flex-shrink`, `flex-basis`

**Center alignment example:**
```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
}
```

**Use cases:** Navigation bars, card layouts, vertically centered content, equal-height columns.

---

### 8. CSS Animations with Example

**Two approaches:**
- `transition` – for simple hover/state changes
- `@keyframes` – for looping or complex animations

**@keyframes Example:**
```css
.box {
  width: 100px;
  height: 100px;
  background: blue;
  animation: moveBox 3s infinite;
}

@keyframes moveBox {
  0%   { transform: translateX(0); }
  50%  { transform: translateX(200px); }
  100% { transform: translateX(0); }
}
```

**Animation Properties:**
- `animation-name`
- `animation-duration`
- `animation-delay`
- `animation-iteration-count`
- `animation-timing-function`

| Feature | Transition | Animation |
|---------|-----------|-----------|
| Trigger | On event (hover) | Runs automatically |
| Complexity | Simple | Advanced |
| Example | Hover effect | Loading spinner |

---

### 9. CSS Selectors and Pseudo-classes

Pseudo-classes style elements in a **special state**.

```css
/* :hover – on mouse over */
button:hover { background: black; color: white; }

/* :focus – when input is selected */
input:focus { border-color: blue; }

/* :first-child */
li:first-child { color: red; }

/* :last-child */
li:last-child { color: green; }

/* :nth-child(n) */
li:nth-child(2) { color: purple; }
```

**Combined example:**
```css
nav a:hover { text-decoration: underline; }
```

---

### 10. CSS 3D Transformations

3D transforms move elements in 3D space.

**Main Functions:**
- `translateZ()` – Move on Z-axis
- `rotateX()`, `rotateY()`, `rotateZ()`
- `scale3d()`

**Example:**
```css
.card {
  width: 150px;
  height: 150px;
  background: lightblue;
  transform: rotateY(45deg);
}
```

**With perspective (required for realistic 3D):**
```css
.container { perspective: 800px; }
.card { transform: rotateX(45deg); }
```

**Use cases:** Card flip effects, product showcases, modern UI interactions.

---

### 11. Responsive Webpage Using Bootstrap Grid

```html
<!DOCTYPE html>
<html>
<head>
  <title>Responsive Layout</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
  <div class="container py-4">
    <div class="row">
      <div class="col-12 col-md-6 col-lg-4 bg-primary text-white p-3">Box 1</div>
      <div class="col-12 col-md-6 col-lg-4 bg-success text-white p-3">Box 2</div>
      <div class="col-12 col-md-12 col-lg-4 bg-danger text-white p-3">Box 3</div>
    </div>
  </div>
</body>
</html>
```

**Behavior:**
- Mobile: each box takes full width
- Medium: first two boxes share a row
- Large: all three in one row

---

### 12. Bootstrap Form with Validation and Input Groups

```html
<!DOCTYPE html>
<html>
<head>
  <title>Bootstrap Form</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container mt-4">
  <form class="needs-validation" novalidate>
    <div class="mb-3">
      <label class="form-label">Full Name</label>
      <input type="text" class="form-control" required>
      <div class="invalid-feedback">Please enter your name.</div>
    </div>

    <div class="mb-3">
      <label class="form-label">Email</label>
      <div class="input-group">
        <span class="input-group-text">@</span>
        <input type="email" class="form-control" required>
        <div class="invalid-feedback">Enter a valid email.</div>
      </div>
    </div>

    <div class="mb-3">
      <label class="form-label">Amount</label>
      <div class="input-group">
        <span class="input-group-text">₹</span>
        <input type="number" class="form-control" required>
      </div>
    </div>

    <button class="btn btn-primary" type="submit">Submit</button>
  </form>
</div>
</body>
</html>
```

**Bootstrap Validation Classes:** `is-valid`, `is-invalid`, `valid-feedback`, `invalid-feedback`

---

### 13. Responsive Dashboard Using Bootstrap (Cards, Alerts, Tables)

```html
<!DOCTYPE html>
<html>
<head>
  <title>Dashboard</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container-fluid">
  <div class="row">
    <!-- Sidebar -->
    <div class="col-md-3 bg-dark text-white p-3 min-vh-100">
      <h4>Dashboard</h4>
      <p>Sidebar menu</p>
    </div>

    <!-- Main Content -->
    <div class="col-md-9 p-4">
      <div class="alert alert-success">Welcome back, user!</div>

      <!-- Cards -->
      <div class="row mb-4">
        <div class="col-md-4">
          <div class="card p-3"><h5>Total Users</h5><p>1200</p></div>
        </div>
        <div class="col-md-4">
          <div class="card p-3"><h5>Orders</h5><p>340</p></div>
        </div>
        <div class="col-md-4">
          <div class="card p-3"><h5>Revenue</h5><p>₹85,000</p></div>
        </div>
      </div>

      <!-- Table -->
      <table class="table table-striped">
        <thead>
          <tr><th>ID</th><th>User</th><th>Status</th></tr>
        </thead>
        <tbody>
          <tr><td>1</td><td>Aman</td><td>Active</td></tr>
          <tr><td>2</td><td>Riya</td><td>Pending</td></tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
</body>
</html>
```

---

### 14. Convert Existing HTML Page to Responsive Using Bootstrap

**Original (non-responsive):**
```html
<div class="header">My Site</div>
<div class="menu">Home About Contact</div>
<div class="content">
  <div class="left">Left</div>
  <div class="right">Right</div>
</div>
```

**Responsive Bootstrap Version:**
```html
<!DOCTYPE html>
<html>
<head>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container">
  <div class="row bg-dark text-white p-3">
    <div class="col-12"><h1>My Site</h1></div>
  </div>

  <div class="row bg-light p-2">
    <div class="col-12 col-md-4">Home</div>
    <div class="col-12 col-md-4">About</div>
    <div class="col-12 col-md-4">Contact</div>
  </div>

  <div class="row mt-3">
    <div class="col-12 col-md-6 bg-primary text-white p-3">Left Content</div>
    <div class="col-12 col-md-6 bg-success text-white p-3">Right Content</div>
  </div>
</div>
</body>
</html>
```

**What changed:** Added Bootstrap CDN, used `container`, `row`, `col-*` classes, mobile-first design with automatic stacking.

---

### 15. Bootstrap Dashboard Using Cards, Tables, and Alerts

```html
<!DOCTYPE html>
<html>
<head>
  <title>Admin Dashboard</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container-fluid">

  <div class="row">
    <div class="col-12 bg-primary text-white p-3">
      <h2>Admin Dashboard</h2>
    </div>
  </div>

  <div class="row mt-3">
    <div class="col-12">
      <div class="alert alert-warning">System maintenance scheduled tonight.</div>
    </div>
  </div>

  <div class="row g-3 mt-1">
    <div class="col-md-4">
      <div class="card shadow-sm p-3"><h5>Users</h5><p>1500 registered users</p></div>
    </div>
    <div class="col-md-4">
      <div class="card shadow-sm p-3"><h5>Sales</h5><p>₹2,50,000 this month</p></div>
    </div>
    <div class="col-md-4">
      <div class="card shadow-sm p-3"><h5>Orders</h5><p>430 completed orders</p></div>
    </div>
  </div>

  <div class="row mt-4">
    <div class="col-12">
      <table class="table table-bordered table-hover">
        <thead class="table-dark">
          <tr><th>Order ID</th><th>Customer</th><th>Amount</th></tr>
        </thead>
        <tbody>
          <tr><td>101</td><td>Rahul</td><td>₹1200</td></tr>
          <tr><td>102</td><td>Priya</td><td>₹2200</td></tr>
        </tbody>
      </table>
    </div>
  </div>

</div>
</body>
</html>
```

---

## UNIT 3: JavaScript Fundamentals

---

### 1. JavaScript Data Types

**A) Primitive Data Types:**

| Type | Description | Example |
|------|-------------|---------|
| Number | Integers & decimals | `10`, `3.14` |
| String | Text | `"Hello"` |
| Boolean | True/False | `true`, `false` |
| Undefined | Declared but not assigned | `let x;` |
| Null | Empty value | `let x = null;` |
| BigInt | Large numbers | `123n` |
| Symbol | Unique identifier | `Symbol()` |

```javascript
let age = 21;         // Number
let name = "Sandeep"; // String
let isActive = true;  // Boolean
```

**B) Non-Primitive (Reference) Types:**

| Type | Description |
|------|-------------|
| Object | Key-value pairs |
| Array | Ordered collection |
| Function | Reusable block of code |

```javascript
let user = { name: "Sandeep", age: 21 };
let arr = [1, 2, 3];
```

---

### 2. var, let, and const with Differences

**var** – Function-scoped, can be redeclared and updated, hoisted.
```javascript
var x = 10;
var x = 20; // ✅ allowed
```

**let** – Block-scoped, can be updated but NOT redeclared.
```javascript
let x = 10;
x = 20;     // ✅ allowed
// let x = 30; ❌ error
```

**const** – Block-scoped, cannot be updated or redeclared.
```javascript
const x = 10;
// x = 20; ❌ error
```

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Redeclare | Yes | No | No |
| Update | Yes | Yes | No |
| Hoisting | Yes (undefined) | Yes (TDZ) | Yes (TDZ) |

---

### 3. JavaScript Operators

**Arithmetic:**
```javascript
let a = 10, b = 5;
console.log(a + b);  // 15
console.log(a - b);  // 5
console.log(a * b);  // 50
console.log(a / b);  // 2
console.log(a % b);  // 0
```

**Comparison:**
```javascript
console.log(10 == "10");  // true  (loose)
console.log(10 === "10"); // false (strict)
```

**Logical:**
```javascript
console.log(true && false); // false
console.log(true || false); // true
console.log(!true);         // false
```

**Assignment:**
```javascript
let x = 5;
x += 2; // x = 7
x -= 1; // x = 6
```

**Unary:**
```javascript
let x = 5;
x++; // 6 (post-increment)
x--; // 5 (post-decrement)
```

---

### 4. JavaScript Functions

A function is a **reusable block of code**.

**Declaration:**
```javascript
function greet() {
  console.log("Hello");
}
greet();
```

**With Parameters:**
```javascript
function add(a, b) {
  return a + b;
}
console.log(add(3, 4)); // 7
```

**Arrow Function (ES6):**
```javascript
const add = (a, b) => a + b;
```

**Why use functions?** Reusability, cleaner code, modular programming.

---

### 5. Control Statements in JavaScript

**if statement:**
```javascript
let age = 18;
if (age >= 18) {
  console.log("Adult");
}
```

**if-else:**
```javascript
if (age >= 18) {
  console.log("Adult");
} else {
  console.log("Minor");
}
```

**else-if chain:**
```javascript
if (age < 13) {
  console.log("Child");
} else if (age < 18) {
  console.log("Teenager");
} else {
  console.log("Adult");
}
```

**switch:**
```javascript
let day = 1;
switch (day) {
  case 1:
    console.log("Monday");
    break;
  case 2:
    console.log("Tuesday");
    break;
  default:
    console.log("Other day");
}
```

---

### 6. Check Even or Odd (Control Statement Program)

```javascript
let num = 7;

if (num % 2 === 0) {
  console.log(num + " is Even");
} else {
  console.log(num + " is Odd");
}
```

**Logic:** `%` gives remainder. If remainder is `0` → Even, else → Odd.

---

### 7. JavaScript Loops

**for loop:**
```javascript
for (let i = 1; i <= 5; i++) {
  console.log(i);
}
// Output: 1 2 3 4 5
```

**while loop:**
```javascript
let i = 1;
while (i <= 5) {
  console.log(i);
  i++;
}
```

**do-while loop:**
```javascript
let i = 1;
do {
  console.log(i);
  i++;
} while (i <= 5);
```

**for...of (arrays):**
```javascript
let arr = [10, 20, 30];
for (let val of arr) {
  console.log(val);
}
```

---

### 8. JavaScript Program Using Functions

```javascript
// Function to calculate square
function square(num) {
  return num * num;
}
console.log(square(4)); // 16

// Function to find factorial
function factorial(n) {
  if (n === 0) return 1;
  return n * factorial(n - 1);
}
console.log(factorial(5)); // 120

// Arrow function
const multiply = (a, b) => a * b;
console.log(multiply(3, 4)); // 12
```

---

### 9. JavaScript Program Using Arrays

```javascript
let arr = [10, 20, 30, 40, 50];

// Access
console.log(arr[0]); // 10

// Loop
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}

// for...of
for (let val of arr) {
  console.log(val);
}

// Common Methods
arr.push(60);        // Add to end → [10,20,30,40,50,60]
arr.pop();           // Remove last → [10,20,30,40,50]
arr.shift();         // Remove first → [20,30,40,50]
arr.unshift(5);      // Add to front → [5,20,30,40,50]
console.log(arr.length);  // 5

// Find sum
let sum = 0;
arr.forEach(val => sum += val);
console.log("Sum:", sum);
```

---

### 10. JavaScript Program Using Objects

```javascript
let person = {
  name: "Sandeep",
  age: 21,
  city: "Delhi"
};

// Access
console.log(person.name);     // dot notation
console.log(person["age"]);   // bracket notation

// Modify
person.city = "Mumbai";

// Add new property
person.email = "s@example.com";

// Loop through object
for (let key in person) {
  console.log(key + ": " + person[key]);
}

// Method inside object
let student = {
  name: "Ravi",
  greet: function() {
    console.log("Hello, I am " + this.name);
  }
};
student.greet(); // Hello, I am Ravi
```

---

## EXTRA: DOM Manipulation & Async JavaScript

---

### DOM (Document Object Model)

**DOM = Document Object Model** – represents HTML as a tree of JS objects. JavaScript can read, modify, add, and delete elements.

**Selecting Elements:**
```javascript
document.getElementById("title");
document.getElementsByClassName("item");
document.querySelector("#title");        // first match
document.querySelectorAll(".item");      // all matches
```

**Changing Content:**
```javascript
let el = document.getElementById("title");
el.innerText = "New Text";
el.innerHTML = "<b>Bold Text</b>";
```

**Changing Styles:**
```javascript
el.style.color = "red";
el.style.backgroundColor = "yellow";
```

**Creating and Removing Elements:**
```javascript
let newEl = document.createElement("p");
newEl.innerText = "Hello";
document.body.appendChild(newEl);

el.remove(); // remove element
```

**Event Handling:**
```javascript
document.getElementById("btn").addEventListener("click", () => {
  alert("Button clicked!");
});
```

**Mini App (DOM + Events):**
```html
<input id="input" />
<button id="add">Add</button>
<ul id="list"></ul>

<script>
document.getElementById("add").addEventListener("click", () => {
  let value = document.getElementById("input").value;
  let li = document.createElement("li");
  li.innerText = value;
  document.getElementById("list").appendChild(li);
});
</script>
```

---

### Async JavaScript (Promises, Fetch, Async/Await)

**Synchronous vs Asynchronous:**
```javascript
// Synchronous – executes in order
console.log("A");
console.log("B"); // Output: A, B

// Asynchronous – does not wait
setTimeout(() => console.log("A"), 2000);
console.log("B"); // Output: B, A
```

**Promises:**
```javascript
let promise = new Promise((resolve, reject) => {
  let success = true;
  if (success) resolve("Done");
  else reject("Error");
});

promise
  .then(res => console.log(res))
  .catch(err => console.log(err));
```

**Fetch API:**
```javascript
fetch("https://jsonplaceholder.typicode.com/users")
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.log(err));
```

**Async/Await (cleanest approach):**
```javascript
async function getData() {
  try {
    let res = await fetch("https://jsonplaceholder.typicode.com/users");
    let data = await res.json();
    console.log(data);
  } catch (err) {
    console.log(err);
  }
}
getData();
```

| Method | Complexity | Readability |
|--------|-----------|-------------|
| Callback | High | Low |
| Promise | Medium | Medium |
| Async/Await | Low | High |

**Full Real-World Flow (DOM + Fetch):**
```javascript
document.getElementById("load").addEventListener("click", async () => {
  let res = await fetch("https://jsonplaceholder.typicode.com/users");
  let users = await res.json();
  let list = document.getElementById("list");
  users.forEach(user => {
    let li = document.createElement("li");
    li.innerText = user.name;
    list.appendChild(li);
  });
});
```

---

## Quick Revision Summary

### Unit 1
- **Internet** = Network | **WWW** = Service on network
- **HTTP** = Stateless communication protocol (GET, POST, PUT, DELETE)
- **DNS** = Converts domain names to IP addresses
- **Client–Server** = Request → Process → Response
- **Static** = Fixed HTML/CSS | **Dynamic** = JS + Backend
- **XML** = Data transport format | **DTD** = XML validation rules
- **XHTML** = Strict HTML (lowercase, closed tags, nested properly)

### Unit 2
- **CSS types**: inline > internal > external (priority order)
- **Specificity**: Inline (1000) > ID (100) > Class (10) > Element (1)
- **Box model**: Content + Padding + Border + Margin
- **Flexbox**: `display: flex` → one-dimensional layout
- **Animations**: `@keyframes` + `animation` property
- **Pseudo-classes**: `:hover`, `:focus`, `:nth-child`
- **Bootstrap grid**: 12-column system, breakpoints: sm/md/lg/xl
- **Bootstrap components**: cards, alerts, tables, forms

### Unit 3
- **Data types**: Number, String, Boolean, Undefined, Null, BigInt, Symbol (primitives) + Object, Array, Function (reference)
- **var/let/const**: Function scope / Block scope / Block scope (no update)
- **Functions**: `function`, arrow functions `=>`
- **Loops**: `for`, `while`, `do-while`, `for...of`
- **Arrays**: `push`, `pop`, `shift`, `unshift`, `forEach`, `length`
- **Objects**: key-value pairs, dot/bracket notation, `for...in`
- **DOM**: `querySelector`, `addEventListener`, `innerHTML`, `createElement`
- **Async**: Callback → Promise → Async/Await (preferred)

---

*End of Web Technology Practice Set Solutions*
