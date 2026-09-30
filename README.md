# 3goods

*A Data for Life 2026 Hackathon project for coordinating donations between donors and organisations.*

[<img width="1440" alt="Updated 3goods landing page" src="./landing-updated.svg" />](./landing-updated.svg)

## Why we built it

Data for Life 2026 gave us a social-security problem statement from the 86th Command, Ministry of National Defence: build a system that coordinates donations based on actual needs. The hackathon was backed by AI Singapore and introduced to us through NUS-ISS.

<img width="1080" alt="Data for Life 2026 problem statement from the 86th Command, Ministry of National Defence" src="https://github.com/user-attachments/assets/7dde859f-1e53-4e74-bcbd-dd514c75ba58" />

The problem felt close to home. My teammate and I have both volunteered for a long time, so community-service problems like this one are close to our hearts. In this problem, generous donations can still miss the people they are meant to help. One organisation may receive too much of the wrong thing while another still faces a shortage.

We started with that gap and asked a simple question: what if organisations could say what they need, and donors could see where their items would be most useful?

## From an idea to a prototype

We brainstormed the donor and organisation journeys together, worked through the matching idea, and built the prototype with Claude Code Pro subscription in roughly four days.

The web application uses React, JavaScript, Vite, Tailwind CSS and Supabase, and is deployed on Vercel. A custom SVG map and small serverless API present data from OpenStreetMap, EM-DAT, UNDP/MOLISA and GADM. Python scripts were used offline to clean, combine and score the source data before the resulting map datasets were published for the JavaScript application.

This is a two-person prototype, not a finished donation service. It is our attempt to make the handoff between generosity and real need clearer, and to help every useful item find a more thoughtful next home.

## If you are donating

You begin with the item you already have. Browse what organisations currently need, list an item you can give, and contact an organisation when there is a potential match. When you list an item, you also say how it gets there: happy to deliver, or pickup only, so the organisation knows what to arrange. If one organisation does not need it, the item remains available for other organisations to discover and request. A small act of giving becomes easier when the next step is just a conversation.

<img width="1440" alt="Available donations with needs-matching labels" src="https://github.com/user-attachments/assets/9aea79e4-49d4-4101-9416-6347eb4709dd" />

<img width="1440" alt="Donation item detail" src="https://github.com/user-attachments/assets/6a5fe838-e22a-4b2e-937c-6788199250c5" />

## If you are an organisation

With an organisation account, you can publish what your community currently needs and mark the most urgent needs as priorities. When you browse items offered by donors, matching labels show which items may fit your organisation's needs. You can request an item, chat with the donor, and coordinate the handoff directly.

<img width="1440" alt="Organisations directory" src="https://github.com/user-attachments/assets/5b1e22ab-a4e1-4d84-a934-d6734e7136b1" />

[<img width="1440" alt="Goods offered by donors with organisation match labels" src="./organisation-matches.svg" />](./organisation-matches.svg)

## How the map works

Python data-processing scripts combine disaster exposure, regional poverty proxies, geographic boundaries and mapped facilities into deployment-ready datasets. The live map is rendered in the browser with JavaScript and reads those datasets through a small Vercel API, with a bundled fallback copy.

[<img width="1440" alt="3goods map showing a registered organisation in Thanh Hoa" src="./map-registered.svg" />](./map-registered.svg)

[<img width="1440" alt="3goods map showing the invite a facility flow in Thai Binh" src="./map-invite.svg" />](./map-invite.svg)

## Run locally

The app lives in `3goods/`. You need Node.js and npm, and a Supabase project of your own. The repo does not pin a Node version. This is a hackathon prototype, not a setup for real donor or organisation data: it has no user authentication, and the supplied database policies let the public key read and write the demo tables.

```bash
git clone https://github.com/fresfrida/data-4-life.git
cd data-4-life/3goods
npm ci
cp .env.example .env.local
```

In `.env.local`, fill in `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` from your Supabase project's API settings. `VITE_DEFAULT_LANGUAGE=en` sets the first-visit language unless the visitor has chosen one already. Use the public anon key only. Never put a service-role key in a `VITE_` variable, since Vite includes it in the browser build.

Run `supabase/schema.sql` in your own project's SQL Editor to create the demo tables and policies. Before adding demo records, insert the eight category names from `src/data/categories.js` into the `categories` table; `npm run db:seed` reads those rows by name and stops if they are missing. The seed script writes demo records to whichever project URL you put in `.env.local`, so check that it is your own project before running it.

```bash
npm run db:seed
npm run dev
```

Open the URL Vite prints. The donation flows need Supabase. The relief map can still use the bundled data in `public/data/`: the Vite development server does not serve this repo's Vercel `/api` functions, so if `VITE_MAP_API_BASE_URL` is left empty, the map falls back to that snapshot and labels it. To use the deployed map API during local development, set that variable to `https://3goods.vercel.app`.

`npm test` runs the Node tests. `npm run build` checks the translation keys and builds the site.

## Please enjoy using the site

The prototype is live at: [3goods.vercel.app](https://3goods.vercel.app/)

We hope it makes the distance between a generous person and a real need a little shorter.

- ⁠Video: [3goods.vercel.app/video](https://3goods.vercel.app/video)
- ⁠Pitch deck: [3goods.vercel.app/about-us](https://3goods.vercel.app/3goods-proposal.pdf)
  


