# SafeMailer-TS

A robust TypeScript project that demonstrates a safe and extensible email sending service using multiple providers and retry logic. This project is designed for reliability and easy extensibility, making it suitable for production-grade applications where email delivery is critical.

## Features

- **Multiple Email Providers:** Easily add or remove email service providers.
- **Retry Logic:** Automatically retries sending emails on failure.
- **Failover:** Tries the next provider if the current one fails after all retries.
- **Extensible:** Add new providers by implementing a simple interface.
- **Strategy Pattern:** Clean separation of concerns and easy provider swapping.

## How to Run

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd safemailer-ts
```

### 2. Install Dependencies

If you have a `package.json` file, install dependencies:

```bash
npm install
```

### 3. Compile TypeScript

If you are using TypeScript directly, compile the code:

```bash
npx tsc
```

This will output JavaScript files to the `dist/` directory (or as configured in your `tsconfig.json`).

### 4. Run the Project

If you compiled to JavaScript:

```bash
node dist/index.js
```

Or, if you want to run TypeScript directly (requires `ts-node`):

```bash
npx ts-node index.ts
```

### 5. Configuration

- By default, the email providers are hardcoded in `index.ts`.
- To add or remove providers, modify the `emailServices` array.
- To change retry logic, adjust the `retries` variable in `EmailServiceManager`.

## Project Structure

```
safemailer-ts/
├── index.ts         # Main entry point and logic
├── package.json     # (optional) Project dependencies
├── tsconfig.json    # (optional) TypeScript configuration
├── .gitignore
└── README.md
```

## Example Output

When you run the project, you will see logs indicating which provider was used and whether sending succeeded or failed, including retry attempts.

## Design Pattern Used

### Strategy Pattern (Behavioral)

This project uses the **Strategy Pattern** to provide a flexible and extensible way to manage multiple email sending strategies.

#### How It Works

- **Strategy Interface:**  
  The `IEmailInterface` defines a contract for all email providers. Any provider must implement the `sendEmail(to, from, body): Promise<boolean>` method.

- **Concrete Strategies:**  
  Each provider (`SendGridEmail`, `YahooMail`, `GmailMail`) implements the interface with its own logic for sending emails.

- **Context (Manager):**  
  The `EmailServiceManager` acts as the context. It receives a list of strategies (providers) and tries each one in order, retrying on failure and moving to the next if all retries fail.

#### Benefits

- **Open/Closed Principle:**  
  New providers can be added without modifying existing code.
- **Separation of Concerns:**  
  Provider logic is isolated from the manager logic.
- **Flexibility:**  
  Easily switch, add, or remove providers as needed.

#### Example

```typescript
const emailServices: IEmailInterface[] = [
    new SendGridEmail(),
    new YahooMail(),
    new GmailMail()
];
const emailManager = new EmailServiceManager(emailServices);
await emailManager.sendEmail('to@example.com', 'from@example.com', 'subject', 'body');
```

## Extending the Project

To add a new provider:

1. Create a new class implementing `IEmailInterface`.
2. Add an instance of your class to the `emailServices` array in `index.ts`.

## License

MIT License (add your license here if needed)
