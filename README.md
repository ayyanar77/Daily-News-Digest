# 📰 Daily News Digest Generator

A **TypeScript-based automated news aggregation tool** that fetches the latest headlines from multiple categories using the **NewsAPI**, removes duplicate articles, and generates a clean, responsive HTML news digest.

The project is designed with a modular architecture and demonstrates practical usage of **TypeScript, Node.js, REST API integration, asynchronous programming, design patterns, dependency injection, and unit testing**.

---

## 🚀 Features

* 📰 Fetch latest news from multiple categories
* 🌐 Integrate with the NewsAPI REST API
* 🔄 Remove duplicate articles automatically
* 🔗 Normalize URLs before duplicate detection
* 🧩 Modular and maintainable architecture
* 🎯 Strong TypeScript type safety
* ⚡ Asynchronous API communication using `async/await`
* 🏗️ Builder Pattern for HTML generation
* 🔒 Singleton Pattern for configuration management
* 💉 Dependency Injection for loosely coupled components
* 🧪 Unit testing with Jest
* 🎨 Generate a responsive HTML news digest
* ⚙️ Configuration-driven category selection

---

## 🛠️ Tech Stack

| Technology     | Purpose                          |
| -------------- | -------------------------------- |
| **TypeScript** | Main programming language        |
| **Node.js**    | JavaScript runtime               |
| **ts-node**    | Execute TypeScript directly      |
| **Axios**      | HTTP client for API requests     |
| **NewsAPI**    | External news data provider      |
| **Jest**       | Unit testing framework           |
| **ts-jest**    | Jest integration with TypeScript |
| **HTML5**      | Generated output                 |
| **CSS3**       | Styling and responsive layout    |

---

## 🏗️ Architecture

The application follows a modular architecture where each component has a clearly defined responsibility.

```text
                         config.json
                              │
                              ▼
                     ┌─────────────────┐
                     │ ConfigService   │
                     │   Singleton     │
                     └────────┬────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │    index.ts     │
                     │  Orchestrator   │
                     └────────┬────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │   NewsClient    │
                     │ Axios + API     │
                     └────────┬────────┘
                              │
                              ▼
                         NewsAPI
                              │
                              ▼
                       Article Objects
                              │
                              ▼
                  ┌─────────────────────┐
                  │   Deduplication     │
                  │    Set<URL>         │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │    HTMLBuilder      │
                  │    Builder Pattern  │
                  └──────────┬──────────┘
                             │
                             ▼
                    output/digest.html
```

---

## 🔄 How It Works

### 1. Configuration

The application reads settings from `config.json`.

Example:

```json
{
  "apiKey": "YOUR_NEWS_API_KEY",
  "country": "us",
  "categories": [
    "technology",
    "sports",
    "business",
    "health",
    "entertainment"
  ]
}
```

The `ConfigService`:

* Reads the configuration file
* Parses the JSON
* Validates required fields
* Provides configuration to other components

---

### 2. Fetch News

`NewsClient` communicates with NewsAPI using Axios.

For each configured category, the application sends a request similar to:

```text
GET /v2/top-headlines
```

with parameters such as:

```text
apiKey
category
country
pageSize
```

The API response is converted into strongly typed `Article` objects.

---

### 3. Duplicate Removal

The same article may appear under multiple categories.

For example:

```text
Technology
 ├── Article A
 ├── Article B
 └── Article C

Business
 ├── Article B
 ├── Article D
 └── Article E
```

Without deduplication:

```text
A
B
C
B
D
E
```

After deduplication:

```text
A
B
C
D
E
```

The project uses a JavaScript `Set` to efficiently track URLs that have already been processed.

```typescript
const seenUrls = new Set<string>();
```

URLs are normalized before being inserted into the set to handle minor formatting differences such as:

```text
https://example.com/article
https://www.example.com/article/
```

---

## 🏗️ Design Patterns

### 1. Singleton Pattern

Used in:

```text
ConfigService
```

The configuration service provides a single shared instance throughout the application.

```typescript
const configService = ConfigService.getInstance();
```

### Why?

Configuration is application-wide, so a centralized instance provides consistent access to configuration data.

---

### 2. Builder Pattern

Used in:

```text
HTMLBuilder
```

The HTML document is constructed step by step.

```typescript
htmlBuilder
  .addHeader("Daily News Digest")
  .addSection("technology", articles)
  .addSection("sports", articles)
  .addFooter();

const html = htmlBuilder.build();
```

### Why?

It keeps the HTML construction logic organized and makes it easier to add or remove sections without creating one large HTML-building method.

---

### 3. Dependency Injection

`NewsClient` receives configuration through its constructor instead of creating or retrieving configuration internally.

```typescript
const newsClient = new NewsClient(config);
```

This reduces coupling and makes components easier to test and maintain.

---

## 📁 Project Structure

```text
daily-news-digest/
│
├── src/
│   ├── config/
│   │   └── ConfigService.ts
│   │
│   ├── clients/
│   │   └── NewsClient.ts
│   │
│   ├── utils/
│   │   └── deduplicate.ts
│   │
│   ├── builders/
│   │   └── HTMLBuilder.ts
│   │
│   ├── types/
│   │   └── Article.ts
│   │
│   └── index.ts
│
├── tests/
│   ├── ConfigService.test.ts
│   ├── deduplicate.test.ts
│   └── HTMLBuilder.test.ts
│
├── output/
│   └── digest.html
│
├── config.json
├── package.json
├── tsconfig.json
├── jest.config.js
└── README.md
```

---

## ⚙️ Installation

### Prerequisites

Make sure you have installed:

* Node.js
* npm
* Git

Check your versions:

```bash
node --version
npm --version
```

---

## 📥 Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/daily-news-digest.git
```

Navigate into the project:

```bash
cd daily-news-digest
```

---

## 📦 Install Dependencies

```bash
npm install
```

---

## 🔑 Configure NewsAPI

Create or update `config.json`:

```json
{
  "apiKey": "YOUR_NEWS_API_KEY",
  "country": "us",
  "categories": [
    "technology",
    "sports",
    "business",
    "health",
    "entertainment"
  ]
}
```

Replace:

```text
YOUR_NEWS_API_KEY
```

with your actual NewsAPI key.

### ⚠️ Security

Do not commit real API keys to GitHub.

For a production version, sensitive credentials should be stored using environment variables or a secure secrets-management solution.

---

## ▶️ Run the Project

Using `ts-node`:

```bash
npx ts-node src/index.ts
```

After successful execution, the generated file will be available at:

```text
output/digest.html
```

Open the HTML file in your browser to view the generated news digest.

---

## 🧪 Run Tests

Execute the Jest test suite:

```bash
npm test
```

Run tests in watch mode:

```bash
npm test -- --watch
```

The tests cover important components such as:

* Configuration validation
* Duplicate article detection
* URL normalization
* HTML generation
* Builder functionality

---

## 🔬 Example Workflow

Suppose the configuration contains:

```json
{
  "categories": [
    "technology",
    "sports",
    "business"
  ]
}
```

The application performs:

```text
1. Load configuration
        ↓
2. Validate API key
        ↓
3. Fetch Technology news
        ↓
4. Fetch Sports news
        ↓
5. Fetch Business news
        ↓
6. Normalize article URLs
        ↓
7. Remove duplicate articles
        ↓
8. Build HTML document
        ↓
9. Write digest.html
```

---

## ⚡ Performance Considerations

The application uses a `Set<string>` for duplicate detection.

If there are `n` articles:

```text
Average duplicate lookup: O(1)

Overall deduplication: O(n)
```

This is more efficient than repeatedly searching through an array.

### Future optimization

Independent API requests can be executed concurrently using `Promise.all()`:

```typescript
const results = await Promise.all(
  categories.map(category =>
    newsClient.fetchHeadlines(category)
  )
);
```

This can reduce total waiting time when multiple categories need to be fetched.

---

## 🧪 Testing Strategy

The project follows a unit-testing approach.

### ConfigService

Tests include:

```text
Valid configuration
Invalid API key
Missing configuration
```

### Deduplication

Tests include:

```text
Unique articles
Duplicate URLs
Normalized duplicate URLs
Empty article list
```

### HTMLBuilder

Tests include:

```text
Header generation
Section generation
Article rendering
Footer generation
Complete HTML generation
```

---

## 🛡️ Error Handling

The application should handle possible failures such as:

* Invalid API key
* Network failure
* NewsAPI errors
* Invalid API responses
* Missing configuration
* Empty API results

Example:

```typescript
try {
  const articles = await newsClient.fetchHeadlines(category);
} catch (error) {
  console.error("Failed to fetch news:", error);
}
```

---

## 🔮 Future Enhancements

Possible improvements include:

* [ ] Use environment variables for API keys
* [ ] Add API request timeout handling
* [ ] Implement retry with exponential backoff
* [ ] Fetch categories concurrently using `Promise.all()`
* [ ] Add caching to reduce repeated API requests
* [ ] Add structured logging
* [ ] Add pagination support
* [ ] Add search functionality
* [ ] Add article filtering by date
* [ ] Add dark/light theme
* [ ] Add email delivery of the generated digest
* [ ] Schedule automatic daily generation
* [ ] Deploy the application using a cloud platform
* [ ] Add CI/CD using GitHub Actions

---

## 🎯 What I Learned

Through this project, I gained practical experience with:

* TypeScript and static typing
* Node.js application development
* REST API integration
* Axios
* Async/await and Promises
* JSON data processing
* Set and hashing concepts
* URL normalization
* Singleton Pattern
* Builder Pattern
* Dependency Injection
* Separation of concerns
* Modular software architecture
* Unit testing with Jest
* Error handling
* HTML/CSS generation

---

## 💡 Key Technical Concepts Demonstrated

```text
                    Daily News Digest
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
     Backend             DSA              Design
        │                  │                  │
     Node.js              Set            Singleton
     TypeScript           Hashing        Builder
     Axios                O(n)           DI
     REST API                            Modular Design
        │
        ▼
   Async Programming
        │
        ▼
    async/await
    Promises
    Non-blocking I/O
```

---

## 📌 Project Highlights

* **Language:** TypeScript
* **Runtime:** Node.js
* **API:** NewsAPI
* **HTTP Client:** Axios
* **Testing:** Jest
* **Architecture:** Modular
* **Design Patterns:** Singleton, Builder
* **Data Structure:** Set
* **Output:** Static HTML
* **API Communication:** REST + HTTP GET
* **Programming Model:** Asynchronous

---

## 👨‍💻 Author

**Your Name**

Computer Science & Engineering Student

Interested in:

* Backend Development
* Software Engineering
* Java & Spring Boot
* TypeScript & Node.js
* React.js
* Data Structures & Algorithms

---

## 📄 License

This project is intended for educational and portfolio purposes.
