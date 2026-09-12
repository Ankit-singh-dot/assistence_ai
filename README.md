# AI Assistance — Intelligent Lead Capture & CRM Platform

An advanced, open-source AI-powered platform designed for automated lead capture, intelligent scoring, multi-channel outreach, and comprehensive CRM capabilities. Built with modern web technologies, this project provides a scalable foundation for agencies, businesses, and developers looking to integrate AI deeply into their customer acquisition and support workflows.

---

##  System Architecture Diagram

```mermaid
graph TD
    %% Core Users & Clients
    Client([User/Lead via Web, WhatsApp, Phone]) -->|Interacts| Frontend
    Admin([Agent/Admin]) -->|Dashboard| Frontend

    %% Frontend Layer
    subgraph "Frontend Layer (Next.js 16 App Router)"
        Dashboard[CRM Dashboard]
        ChatWidget[AI Chat Widget]
        AutomationsUI[Automations Builder]
        3DUI[3D Interactive UI - Three.js/Spline]
        
        Dashboard --- AutomationsUI
        ChatWidget --- 3DUI
    end
    
    Frontend <-->|Next.js Server Actions & API Routes| Backend
    
    %% Backend Layer
    subgraph "Backend Layer & Business Logic"
        Auth[Clerk Authentication & Svix Webhooks]
        CRM[Lead, Task & Appointment Engine]
        AI_Engine[Gemini AI Engine - Chat & Scoring]
        AutomationEngine[Workflow Automation Engine]
        CommsEngine[Multi-Channel Comm Engine]
        
        Auth --- CRM
        CRM --- AutomationEngine
        AI_Engine --- CommsEngine
    end
    
    Backend <-->|Prisma ORM| Database[(Neon PostgreSQL)]
    
    %% External Integrations
    CommsEngine <-->|Voice/SMS/Chat| Twilio
    CommsEngine <-->|Voice AI| Vapi
    CommsEngine <-->|Emails| Nodemailer
    AI_Engine <-->|LLM Inference| Gemini API
    Backend <-->|Custom Integrations| Webhooks[Make.com / Custom Webhooks]
```

---

##  Core Features in Depth

### 1. Intelligent Omnichannel Lead Capture & CRM
The core of the platform is a centralized Customer Relationship Management (CRM) engine that intelligently captures leads from various inbound channels (Website, WhatsApp, Email, Instagram). 
- **Dynamic Lead Profiling**: Lead profiles maintain tags, avatars, and contact data, directly connecting leads to appointments, tasks, and historical conversation data.
- **Lead Scoring & Sentiment Analysis**: When a lead interacts with the system, the AI dynamically analyzes their messages to determine sentiment (`positive`, `neutral`, `negative`) and updates the lead score (0-100) based on their buying intent and engagement.
- **Agent Handoff**: Conversations can be seamlessly handed off from the AI bot to a human agent, changing the system's `botPhase` to `handed_off`. Agents can then use **Quick Replies** to respond faster from the centralized dashboard.
- **Tasks & Appointments**: Agents can attach tasks and book appointments (demos, intros, consultations) directly to leads, ensuring a clear pipeline and proactive follow-ups.

### 2. Context-Aware AI Chat & Language Support
- **Gemini-Powered Engine**: The conversational engine understands the deep context of past messages in a thread and responds accurately as an automated assistant.
- **Multi-Language Support**: The bot can handle conversations in different languages, seamlessly switching phases (e.g., `language_select` to `ai_chat`) based on user preferences.
- **Custom Knowledge & Bot Phases**: Depending on the step of the customer journey, the bot guides the user from general inquiries to targeted data collection (e.g., dynamically prompting for an email or phone number to unlock resources).

### 3. Voice AI & Automated Outreach
- **Twilio & Nodemailer Integrations**: The platform orchestrates automated transactional and marketing emails via Nodemailer, while Twilio handles SMS and WhatsApp conversational persistence.
- **Vapi Voice Call Integration**: Not only can the system chat via text, but it also supports queuing and handling AI-driven voice calls. Outbound or inbound phone calls are tracked, durations are logged, and the AI conversation is automatically transcribed and summarized directly into the `VoiceCall` database table for rapid agent review.

### 4. Customizable Automation Engine
- **Trigger-Action Workflows**: Users can define and activate custom automations directly in the UI. For example, a trigger like `No response in 24h` can automatically initiate an action such as `Send Follow-up Email`.
- **Automation Execution Tracking**: The system tracks every execution of an automation run, maintaining a success rate and execution history to help agents refine and iterate on their automated sequences.

### 5. Third-Party Integrations Engine
- **Modular Connection System**: The database natively supports managing integrations on a per-user basis (e.g., `WhatsApp`, `Instagram`, `Gmail`, `Calendar`). It securely stores necessary metadata and API keys, and utilizes webhooks to process inbound platform data (with payload verification).

### 6. Premium 3D User Interface & Micro-Animations
- Utilizing the power of **Three.js**, **React Three Fiber**, and **Spline**, the application features a deeply immersive, highly responsive dashboard UI.
- Interactivity is further enhanced using **Framer Motion** and **GSAP** for micro-animations and smooth layout transitions, providing an agency-level, state-of-the-art digital experience right out of the box.

---

##  Technology Stack & Architecture

This project is built utilizing a bleeding-edge, robust modern tech stack.

### Core Framework & Frontend
- **[Next.js 16](https://nextjs.org/)**: The backbone of the application. Uses React 19, App Router, Server Components, and Server Actions for highly optimized client-server data flow.
- **[TypeScript](https://www.typescriptlang.org/)**: Ensures strict type-safety across the entire repository.
- **Styling**: **[Tailwind CSS v4](https://tailwindcss.com/)** paired with **[Shadcn UI](https://ui.shadcn.com/)** and **[Radix UI](https://www.radix-ui.com/)** for beautiful, accessible, and customizable components.

### Backend & Database
- **Database**: **[PostgreSQL](https://www.postgresql.org/)** (hosted via Neon DB) for robust relational data mapping.
- **ORM**: **[Prisma](https://www.prisma.io/)** serves as the bridge, executing migrations and ensuring strict typing across the entire database schema (managing highly relational structures between `User`, `Lead`, `Conversation`, `Message`, and `Automation`).

### External API & Services
- **Authentication & Webhooks**: **[Clerk](https://clerk.dev/)** provides comprehensive identity management, while **[Svix](https://www.svix.com/)** secures inbound Clerk webhooks.
- **Artificial Intelligence**: **[Google Generative AI (Gemini)](https://ai.google.dev/)** powers the NLP logic, chatbots, and sentiment classification.
- **Communications Engine**: **[Twilio SDK](https://www.twilio.com/)** (SMS/Conversations), **[Vapi](https://vapi.ai/)** (Voice AI), and **[Nodemailer](https://nodemailer.com/)** handle omnichannel outreach.

---

##  Getting Started

### Prerequisites
- **Node.js** (v20+ recommended)
- A **PostgreSQL** database (e.g., Neon)
- Accounts for **Clerk**, **Google AI (Gemini)**, and optionally **Twilio**, **Vapi**, and **Make.com**.

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/ai_assistance.git
cd ai_assistance
```

### 2. Install Dependencies
```bash
npm install
# or
yarn install
# or
pnpm install
```

### 3. Environment Variables
Create a `.env` file in the root directory. You can use the provided `.env.example` as a template:
```bash
cp .env.example .env
```
Fill in the necessary keys:
- `DATABASE_URL` (Your Postgres/Neon connection string)
- `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` & `CLERK_SECRET_KEY` (From Clerk Dashboard)
- `WEBHOOK_SECRET` (For Clerk Webhooks via Svix)
- `GEMINI_API_KEY` (From Google AI Studio)
- `NEXT_PUBLIC_APP_URL` (e.g., `http://localhost:3000`)

### 4. Database Setup
Run the Prisma migrations to set up your database schema:
```bash
npx prisma db push
# or
npx prisma migrate dev
```

### 5. Run the Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) with your browser to see the app running.

---

## 📂 Project Structure

- `app/`: Next.js App Router pages, API routes, and layouts (Dashboard, Onboarding, Authentication).
- `actions/`: Next.js Server Actions encapsulating core business logic (Chatbot logic, Lead Scoring, Outreach, Webhooks).
- `components/`: Reusable React components (UI library, complex 3D interactive elements, layout wrappers).
- `lib/`: Utility functions, Prisma client initialization, and shared configurations.
- `prisma/`: Database schema definition (`schema.prisma`) and configuration files.
- `scripts/`: Development and database testing scripts (DB checks, Webhook simulators).
- `public/`: Static assets (images, fonts, 3D model assets).

---

##  Contributing
Contributions are welcome! Whether you're fixing bugs, adding new integrations, or improving documentation, feel free to open an issue or submit a pull request.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

##  License
This project is open-source and available under the MIT License.
# assistence_ai
