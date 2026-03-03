# Bitespeed Backend Task — Identity Reconciliation
## Overview

This project implements the Identity Reconciliation backend challenge.

It exposes a POST /identify endpoint that links customer contacts using email or phone number. If multiple records belong to the same person, they are grouped under a single primary contact.

Tech Stack: Node.js, TypeScript, Express, PostgreSQL, Docker

## Running the Project
## Option 1: Local Setup (Node + Docker Postgres)
git clone https://github.com/guts-718/bitespeed-assignment.git
cd bitespeed_assignment
npm install

## Start PostgreSQL:

docker run --name bitespeed-postgres ^
  -e POSTGRES_USER=postgres ^
  -e POSTGRES_PASSWORD=postgres ^
  -e POSTGRES_DB=bitespeed ^
  -p 5432:5432 ^
  -d postgres

## Create .env:

PORT=3000
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/bitespeed

## Run server:

npm run dev
## Option 2: Docker Compose (Recommended)
docker compose up --build

Server runs at:
http://localhost:3000

## API
## POST /identify

## Request

{
  "email": "string (optional)",
  "phoneNumber": "string (optional)"
}

## Response

{
  "contact": {
    "primaryContactId": number,
    "emails": string[],
    "phoneNumbers": string[],
    "secondaryContactIds": number[]
  }
}
## Logic

- Creates a new primary contact if no match exists.

- Links matching records under the oldest primary.

- Merges multiple primaries if needed.

- Uses transactions to ensure consistency.

- No deletions — relationships managed via linkedId and linkPrecedence.
