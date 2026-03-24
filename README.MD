•	4. README.md Example
•	# Weather Ranking App

## Architecture
- **Backend:** Node.js + Express + Apollo GraphQL
- **Frontend:** React + TypeScript + Apollo Client
- **Data Source:** Open-Meteo API
- **Scoring Logic:** Rank activities based on weather parameters (temperature, precipitation)

## Technical Choices
- GraphQL to decouple frontend/backend and easily extend activity scoring.
- TypeScript for maintainability and type safety.
- Separation of concerns: `services` for API calls, `utils` for business logic.

## AI Assistance
- Used ChatGPT to draft GraphQL boilerplate and scoring logic.
- Applied judgment to clean and customize AI-generated code.

## Omissions & Trade-offs
- City-to-coordinates mapping is hardcoded for demo.
- Scoring is simplified; a more realistic model would include wind, snow, surf conditions.
- No deployment scripts; can use Vercel/Heroku to deploy both frontend/backend.
