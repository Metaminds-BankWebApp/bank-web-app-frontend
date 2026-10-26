# PrimeCore Frontend

The Next.js application for PrimeCore's customer banking tools, bank officer workspace, and administration panel.

[Live application](https://bank-web-app-theta.vercel.app/) · [Backend repository](https://github.com/Metaminds-BankWebApp/Bank-Web-App-backend) · [Project overview and screenshots](../README.md#screenshots)

![PrimeCore customer workspace](../docs/screenshots/customer-app-hub.png)

## Features

| Workspace | Main capabilities |
| --- | --- |
| CreditLens | Credit risk scores, contributing factors, trends, insights, and reports |
| SpendIQ | Expense records, category analysis, budgets, and reports |
| LoanSense | Loan eligibility, borrowing capacity, repayment estimates, and assessment history |
| Transact | Transfers, OTP verification, beneficiaries, and transaction history |
| Bank officer | Customer onboarding, financial records, credit reviews, and work queues |
| Administration | Customers, branches, officers, loan policies, audit logs, and support |

Access depends on the authenticated role and customer profile. The UI includes responsive layouts, notifications, support conversations, and profile settings.

## Local development

Use a supported Node.js LTS release compatible with Next.js 16 and npm. Run these commands from the repository root:

```bash
cd frontend
npm ci
```

Create `.env.local` inside this `frontend` directory:

```dotenv
NEXT_PUBLIC_API_BASE_URL=http://localhost:8080/api
```

Start the backend according to its [repository instructions](https://github.com/Metaminds-BankWebApp/Bank-Web-App-backend), then run:

```bash
npm run dev
```

Open [localhost:3000](http://localhost:3000). Use an account configured in your backend environment. Backend setup, database configuration, and development account seeding are maintained in the backend repository.

## Configuration

| Variable | Default | Purpose |
| --- | --- | --- |
| `NEXT_PUBLIC_API_BASE_URL` | `http://localhost:8080/api` | Base URL used by the Axios client; include the `/api` prefix |

Configuration is read in [`config/env.ts`](config/env.ts). Endpoint paths such as `/auth/login` are appended to this base URL. Restart the development server after changing `.env.local`; rebuild deployments when changing this public environment variable. `NEXT_PUBLIC_` values are exposed to the browser and must not contain secrets.

The backend must allow the frontend origin in its CORS configuration, including credentialed requests. For local development, that origin is normally `http://localhost:3000`.

## Available commands

Run these commands from `frontend/`:

| Command | Purpose |
| --- | --- |
| `npm ci` | Install the versions recorded in `package-lock.json` |
| `npm run dev` | Start the development server |
| `npm run build` | Create a production build |
| `npm start` | Serve an existing production build |
| `npm run lint` | Run ESLint |

## API integration

- Use `GET /api/auth/me` to resolve the authenticated identity and domain IDs.
- Use `bankCustomerId`, `publicCustomerId`, or `officerId` for the corresponding customer or officer requests. Use roles for access checks and workspace selection.
- Keep API wrappers and endpoint definitions in `src/api/`, request and response types in `src/types/dto/`, and authentication state in `src/store/`.
- The shared Axios client attaches the stored bearer token and enables credentials. For requests outside the public authentication endpoints, a `401` or `403` clears the session and redirects to `/login`.

See [`src/api/client.ts`](src/api/client.ts) and [`src/api/endpoints.ts`](src/api/endpoints.ts) for the current behavior and routes.

## Deploy on Vercel

1. Import the frontend repository and set the project root directory to `frontend`.
2. Use the Next.js framework preset and the `npm run build` build command.
3. Set `NEXT_PUBLIC_API_BASE_URL` to the deployed backend's HTTPS API base URL, including `/api`.
4. Configure the backend to allow the deployed frontend origin and any preview origins you intend to use.
5. Deploy, then verify sign-in and the relevant role dashboards against the backend.

The shared live application link is [bank-web-app-theta.vercel.app](https://bank-web-app-theta.vercel.app/).

## Screenshots and portable paths

The complete [screenshot gallery](../README.md#screenshots) is in the repository README. All nine supplied screenshots are copied into [`../docs/screenshots/`](../docs/screenshots/) with descriptive filenames.

Use `../docs/screenshots/<filename>.png` for images in this README. Use `docs/screenshots/<filename>.png` in the repository README. These paths work when the repository is moved or cloned; no local drive letters or OneDrive paths are required.

Application assets served to users belong in `public/`; the README screenshots live separately in `docs/screenshots/`.
