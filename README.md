# InterviewPrep — AI-Powered Mock Interview Platform

InterviewPrep is a web application for practicing job interviews with an AI voice interviewer. Users can create an account, generate interview questions for a target role and technology stack, complete a voice-based mock interview, and review AI-generated feedback on their performance.

## Features

- **Authentication:** Sign up, sign in, and sign out using Firebase Authentication and server-managed session cookies.
- **Customized interview generation:** Generate interview questions based on job role, experience level, technology stack, interview type, and desired question count.
- **Voice-based mock interviews:** Conduct an interview with an AI interviewer using Vapi.
- **AI-generated evaluation:** Analyze interview transcripts with Google Gemini and generate a score, category breakdown, strengths, areas for improvement, and a final assessment.
- **Interview dashboard:** View your previous interviews and browse other finalized interviews available in the app.
- **Persistent data:** Store user profiles, interviews, and feedback in Cloud Firestore.
- **Responsive interface:** Built with reusable React components and Tailwind CSS utilities.

## Tech Stack

| Area | Technologies |
|---|---|
| Framework | Next.js 15 (App Router), React 19, TypeScript |
| Styling and UI | Tailwind CSS 4, Radix UI, Lucide React, Sonner |
| Authentication | Firebase Authentication, Firebase Admin SDK, session cookies |
| Database | Cloud Firestore |
| Voice interviews | Vapi Web SDK |
| AI question generation and evaluation | Google Gemini through the Vercel AI SDK |
| Forms and validation | React Hook Form, Zod |

## Application Flow

1. A user creates an account or signs in.
2. From the dashboard, the user starts interview generation and provides the role, experience level, technology stack, interview type, and number of questions.
3. The backend asks Google Gemini to generate role-specific questions and saves the interview to Firestore.
4. The user starts a voice interview powered by Vapi. The app collects final transcript messages during the session.
5. When the interview ends, the transcript is submitted for AI evaluation. The generated feedback is saved to Firestore and shown on the feedback page.

## Project Structure

```text
InterviewPrep/
├── app/
│   ├── (auth)/                 # Sign-in and sign-up pages
│   ├── (root)/                 # Dashboard and interview pages
│   └── api/vapi/generate/      # API route for generating interview questions
├── components/                 # Interview agent, forms, cards, and UI components
├── constants/                  # Shared constants and feedback schema
├── firebase/                   # Firebase client and Admin SDK setup
├── lib/
│   ├── action/                 # Authentication and Firestore server actions
│   └── vapu.sdk.ts             # Vapi client initialization
├── public/                     # Images, icons, and static assets
├── package.json
└── README.md
```

## Prerequisites

- Node.js (use a current LTS release)
- npm
- A Firebase project with **Authentication** and **Cloud Firestore** enabled
- A Firebase service account for server-side access
- A Google AI API key for Gemini
- A Vapi account with a configured assistant/workflow suitable for the app

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

Replace the URL and directory name with your GitHub repository details.

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Create a `.env.local` file in the project root. Add the following values:

```env
# Firebase Admin SDK — server-side credentials
FIREBASE_PROJECT_ID=your_firebase_project_id
FIREBASE_CLIENT_EMAIL=your_service_account_email
FIREBASE_PRIVATE_KEY="your_service_account_private_key_with_escaped_newlines"

# Google Gemini — server-side API key
GOOGLE_GENERATIVE_AI_API_KEY=your_google_ai_api_key

# Vapi — browser-side public token and workflow ID
NEXT_PUBLIC_VAPI_WEB_TOKEN=your_vapi_public_web_token
NEXT_PUBLIC_VAPI_WORKFLOW_ID=your_vapi_workflow_id
```

Where to find these values:

- **Firebase Admin credentials:** Firebase Console → Project settings → Service accounts. Keep the private key private. The code converts escaped `\n` sequences in `FIREBASE_PRIVATE_KEY` into newlines.
- **Google Gemini API key:** Create one through Google AI Studio. The key is used by server-side code; do not prefix it with `NEXT_PUBLIC_`.
- **Vapi values:** Copy the public web token and workflow ID from your Vapi configuration. Values prefixed with `NEXT_PUBLIC_` are included in browser-side code, so only use credentials intended to be public in a client application.

The Firebase web configuration is currently defined in `firebase/client.ts`. If you are connecting your own Firebase project, update that configuration using the web app details from Firebase Console. Firebase Authentication must have the sign-in method used by the app enabled, and Cloud Firestore must be initialized.

> **Security:** Never commit `.env.local`, Firebase service-account JSON files, private keys, or server API keys. Check Firebase Authentication settings, Firestore rules, and API key restrictions before deploying. The Firebase web API key is a client configuration value, not a substitute for Firestore security rules.

### 4. Start the development server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

### 5. Create a production build (optional)

```bash
npm run build
npm run start
```

The production server runs after the build completes. For linting, run:

```bash
npm run lint
```

## Main Routes

| Route | Purpose |
|---|---|
| `/sign-in` | Sign in to an existing account |
| `/sign-up` | Create an account |
| `/` | Dashboard showing interviews |
| `/interview` | Generate a new interview |
| `/interview/[id]` | Start a generated mock interview |
| `/interview/[id]/feedback` | Review interview feedback |
| `/api/vapi/generate` | Generate interview questions through the API |

## Troubleshooting

- **Firebase Admin initialization fails:** Verify `FIREBASE_PROJECT_ID`, `FIREBASE_CLIENT_EMAIL`, and `FIREBASE_PRIVATE_KEY`. Ensure newline characters in the private key are represented as `\n` in `.env.local`.
- **Gemini requests fail:** Verify `GOOGLE_GENERATIVE_AI_API_KEY` and confirm that the associated Google AI project can access the selected Gemini model.
- **Voice interview does not start:** Verify the Vapi public web token, workflow ID, and Vapi assistant/workflow configuration.
- **Firestore permission or query errors:** Confirm that Firestore is enabled and review security rules and any index requirement shown in the Firebase error message.
- **Environment changes are not picked up:** Stop the development server and run `npm run dev` again after editing `.env.local`.

## Contributing

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-change`.
3. Commit your changes with a descriptive message.
4. Push the branch and open a pull request.

## License

No license is currently specified. Unless a license is added to the repository, reuse and redistribution are not automatically granted.
