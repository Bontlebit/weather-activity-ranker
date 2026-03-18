# weather-activity-ranker
Full-stack weather-based activity ranking app using Node.js, GraphQL, and React. Fetches Open-Meteo data to score travel activities over 7 days.” app”  How: “GraphQL + React + Node.js”  Why: “Helps users decide best travel activities”   Optional Add-ons  “Technical assessment project”  “Scalable architecture demo”  “Learning project”
Weather Activity Ranker Readme
🌤️ Weather Activity Ranker

A lightweight full-stack web application that ranks how desirable a city or town is for various activities over the next 7 days based on weather data.

📌 Overview

This project was built as a Lead/Senior Engineer technical assessment, focusing on clean architecture, scalability, and maintainability rather than feature completeness.

🧠 What it does

Accepts a city or town

Fetches 7-day weather data from Open-Meteo

Ranks activities based on suitability:

Skiing ❄️

Surfing 🌊

Outdoor sightseeing 🌤️

Indoor sightseeing 🏛️

🎯 Why this project

Helps users decide the best travel activities based on upcoming weather conditions.

🏗️ Architecture

Built using a modern full-stack TypeScript architecture:

Frontend: React

Backend: Node.js

API Layer: GraphQL

Testing: Jest

CI/CD: GitHub Actions

Key Design Decisions

GraphQL → Flexible querying and efficient data fetching

Service Layer → Separation of concerns and scalability

TypeScript → Type safety and maintainability

⚙️ Features

🌍 Fetches weather data via Open-Meteo API

📊 Activity ranking engine based on:

Temperature 🌡️

Wind speed 💨

Precipitation 🌧️

Snowfall ❄️

⚡ Fast and lightweight

✅ Unit tested using Jest

🔁 CI/CD pipeline with GitHub Actions
Project Structure:src/
  controllers/
  services/
  utils/
tests/
.github/
Installation:# Clone the repository
git clone https://github.com/your-username/your-repo-name.git

# Navigate into the project
cd your-repo-name

# Install dependencies
npm install
Running The APP :npm run dev
Running Tests:npm test
Example: import { getWeatherData } from '../src/services/weatherService';

describe('Weather Service', () => {
  it('should return weather data for a valid city', async () => {
    const data = await getWeatherData('Cape Town');
    expect(data).toBeDefined();
  });
});
CI/CD (GitHub Actions)

This project uses GitHub Actions to automatically:

Install dependencies

Run tests

Ensure code quality on every push and pull request

Workflow File

Create:

.github/workflows/ci.yml
Example Configuration
name: CI Pipeline


on:
  push:
    branches: [main]
  pull_request:
    branches: [main]


jobs:
  build:
    runs-on: ubuntu-latest


    steps:
      - name: Checkout code
        uses: actions/checkout@v3


      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'


      - name: Install dependencies
        run: npm install


      - name: Run tests
        run: npm test
🧮 How Ranking Works

The system evaluates weather conditions and assigns a score to each activity based on suitability.

Example logic:

Skiing → Requires snowfall + low temperature

Surfing → Requires wind + suitable wave conditions

Outdoor sightseeing → Mild temperature + low precipitation

Indoor sightseeing → Higher score during poor weather

🤖 AI Usage

AI tools (e.g., ChatGPT, Copilot) were used to:

Scaffold GraphQL structure

Assist with ranking logic ideas

Improve code structure and documentation

All outputs were reviewed and refined manually to ensure correctness and maintainability.

⚖️ Trade-offs & Omissions

Due to time constraints (2–3 hours focus):

❌ Hardcoded coordinates (no geocoding API)

❌ Basic scoring algorithm

❌ Minimal UI implementation

Why

Priority was given to:

Clean architecture

Code quality

Separation of concerns

🔮 Future Improvements

🌐 Add full React frontend UI

📍 Integrate geocoding API

⚡ Add caching layer (e.g., Redis)

🤖 Improve ranking with ML or weighted scoring

📈 Add more activities

🧪 Expand test coverage

🤝 Contributing

Contributions are welcome!

Fork the repository

Create a feature branch

Commit your changes

Open a pull request

📜 License

MIT License

🚀 Git Setup (First Push)
git init
git add .
git commit -m "Initial commit"


git remote add origin https://github.com/YOUR_USERNAME/weather-ranking-app.git
git branch -M main
git push -u origin main
💡 Project Highlights

✅ Scalable architecture demo

✅ Technical assessment project

✅ Clean separation between data, logic, and UI

✅ Focus on quality over quantity

CODE OUTLINED
🧠 Architecture Overview (High-Level)
/weather-activity-ranker
 ├── backend
 │    ├── src
 │    │    ├── index.ts
 │    │    ├── schema.ts
 │    │    ├── resolvers.ts
 │    │    ├── services/
 │    │    │      ├── weatherService.ts
 │    │    │      ├── scoringService.ts
 │    │    └── types.ts
 │
 ├── frontend
 │    ├── src
 │    │    ├── App.tsx
 │    │    ├── components/
 │    │    │      ├── Search.tsx
 │    │    │      ├── Results.tsx
 │    │    └── graphql/
 │    │           ├── queries.ts
⚙️ BACKEND (Node.js + GraphQL)
1. Install dependencies
npm init -y
npm install express apollo-server-express graphql axios cors
npm install -D typescript ts-node @types/node
2. src/index.ts
import express from "express";
import { ApolloServer } from "apollo-server-express";
import cors from "cors";
import { typeDefs } from "./schema";
import { resolvers } from "./resolvers";

async function startServer() {
  const app = express();
  app.use(cors());

  const server = new ApolloServer({
    typeDefs,
    resolvers,
  });

  await server.start();
  server.applyMiddleware({ app });

  app.listen(4000, () => {
    console.log("Server running on http://localhost:4000/graphql");
  });
}

startServer();
3. src/schema.ts
import { gql } from "apollo-server-express";

export const typeDefs = gql`
  type ActivityScore {
    date: String
    skiing: Int
    surfing: Int
    outdoor: Int
    indoor: Int
  }

  type Query {
    getActivityScores(city: String!): [ActivityScore]
  }
`;
4. src/resolvers.ts
import { getWeather } from "./services/weatherService";
import { calculateScores } from "./services/scoringService";

export const resolvers = {
  Query: {
    getActivityScores: async (_: any, { city }: { city: string }) => {
      const weatherData = await getWeather(city);
      return calculateScores(weatherData);
    },
  },
};
5. src/services/weatherService.ts
import axios from "axios";

export async function getWeather(city: string) {
  // Step 1: Convert city → lat/lon
  const geoRes = await axios.get(
    `https://geocoding-api.open-meteo.com/v1/search?name=${city}`
  );

  const { latitude, longitude } = geoRes.data.results[0];

  // Step 2: Fetch weather
  const weatherRes = await axios.get(
    `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&daily=temperature_2m_max,precipitation_sum,windspeed_10m_max&timezone=auto`
  );

  return weatherRes.data.daily;
}
6. src/services/scoringService.ts
export function calculateScores(weather: any) {
  const results = [];

  for (let i = 0; i < weather.time.length; i++) {
    const temp = weather.temperature_2m_max[i];
    const rain = weather.precipitation_sum[i];
    const wind = weather.windspeed_10m_max[i];

    const skiing = temp < 5 ? 90 : 10;

    const surfing =
      wind > 20 && rain < 5 ? 85 : 40;

    const outdoor =
      temp > 18 && rain < 2 ? 90 : 30;

    const indoor =
      rain > 5 ? 80 : 20;

    results.push({
      date: weather.time[i],
      skiing,
      surfing,
      outdoor,
      indoor,
    });
  }

  return results;
}
🎨 FRONTEND (React + GraphQL)
1. Install
npx create-react-app frontend --template typescript
npm install @apollo/client graphql
2. src/graphql/queries.ts
import { gql } from "@apollo/client";

export const GET_SCORES = gql`
  query GetScores($city: String!) {
    getActivityScores(city: $city) {
      date
      skiing
      surfing
      outdoor
      indoor
    }
  }
`;
3. src/App.tsx
import React from "react";
import { ApolloClient, InMemoryCache, ApolloProvider } from "@apollo/client";
import Search from "./components/Search";

const client = new ApolloClient({
  uri: "http://localhost:4000/graphql",
  cache: new InMemoryCache(),
});

function App() {
  return (
    <ApolloProvider client={client}>
      <div>
        <h1>Weather Activity Ranker</h1>
        <Search />
      </div>
    </ApolloProvider>
  );
}

export default App;
4. src/components/Search.tsx
import React, { useState } from "react";
import { useLazyQuery } from "@apollo/client";
import { GET_SCORES } from "../graphql/queries";
import Results from "./Results";

export default function Search() {
  const [city, setCity] = useState("");

  const [fetchScores, { data, loading }] = useLazyQuery(GET_SCORES);

  return (
    <div>
      <input
        placeholder="Enter city..."
        value={city}
        onChange={(e) => setCity(e.target.value)}
      />
      <button onClick={() => fetchScores({ variables: { city } })}>
        Search
      </button>

      {loading && <p>Loading...</p>}
      {data && <Results data={data.getActivityScores} />}
    </div>
  );
}
5. src/components/Results.tsx
export default function Results({ data }: any) {
  return (
    <div>
      {data.map((day: any) => (
        <div key={day.date} style={{ border: "1px solid #ccc", margin: 10 }}>
          <h3>{day.date}</h3>
          <p>Skiing: {day.skiing}</p>
          <p>Surfing: {day.surfing}</p>
          <p>Outdoor: {day.outdoor}</p>
          <p>Indoor: {day.indoor}</p>
        </div>
      ))}
    </div>
  );
}
