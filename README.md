# MeghDrishti — AI cloud-eye for cleaner solar power in Bangladesh

MeghDrishti helps solar-plant operators anticipate fast-moving clouds and decide when to charge batteries, shift flexible loads, or prepare a generator. The goal is to avoid running diesel gensets “just in case” when solar output is likely to remain stable.

> **Current status:** The included dashboard is an interactive prototype. Its sky, power, forecasts, alerts, and savings are simulated; it does not connect to a live camera or forecasting service.

## Demo

Open [`MeghDrishti_ AI cloud-eye for cleaner solar in Bangladesh.html`](./MeghDrishti_%20AI%20cloud-eye%20for%20cleaner%20solar%20in%20Bangladesh.html) in a modern browser. No build step or dependencies are required.

The demo lets you explore clear, partly cloudy, and overcast scenarios; switch between an illustrative Gazipur factory and the SKIPP'D benchmark site; replay and scrub through the day; and inspect forecast ranges, Bangla operator alerts, estimated diesel savings, and a panel-cleaning advisor. Savings and cleaning estimates use editable assumptions, not field measurements.

## How it is intended to work

1. A Raspberry Pi with a fisheye camera captures the sky once per minute. The proposed camera setup costs under BDT 15,000.
2. Site timestamps are checked against the sun's path, and `pvlib` estimates the clear-sky power limit.
3. HAMF combines the latest three sky images, 60 minutes of power history, and the clear-sky estimate to forecast 19 quantiles from 5 to 120 minutes ahead.
4. Decision rules translate the forecast and its uncertainty into practical actions: charge the battery, delay a flexible load, keep diesel off, or prepare the genset.
5. Operators receive the recommendation in Bangla through a dashboard and, in the planned deployment, SMS. The small model is intended to run locally and tolerate weak internet connectivity.

## Research evidence

The forecasting research uses SKIPP'D, a public dataset containing 517 days of sky images and PV power. On a 20-day test set, the reported results are:

| Model | RMSE (kW) | Skill vs. persistence | Probabilistic skill | 90% range width |
| --- | ---: | ---: | ---: | ---: |
| HAMF | 3.38 | 17.9% | 34.8% | 5.6 kW |
| TiDE-style dense model | 3.47 | 15.8% | 31.3% | 6.6 kW |
| Probabilistic persistence | 4.12 | 0.1% | 0.0% | 9.8 kW |
| Smart persistence | 4.12 | 0.0% | −16.6% | — |
| SUNSET CNN benchmark | 4.25 | −3.1% | −80.9% | — |

HAMF's reported improvement over persistence and the SUNSET benchmark is statistically significant (`p < 0.001`). Its 90% prediction range is 43% narrower than probabilistic persistence and covers 87.6% of observations, compared with 83.4%.

The research also identified daylight-saving timestamp errors in 328,309 dataset records. Leaving these uncorrected inflated reported skill by about seven percentage points, so timestamp validation against the sun is part of the proposed pipeline.

### Limitations

- The model was trained at one site in California; performance in Bangladesh has not yet been validated.
- HAMF's improvement over the TiDE-style dense baseline was not statistically significant (`p = 0.20`).
- Packaged weather forecasts did not add skill at the evaluated forecast horizons.
- The dashboard's diesel, CO₂, and cleaning estimates depend on assumptions and are not measured field results.
- The research paper is under double-blind review.

## Roadmap

The project plan in the prototype is for Hack for Humanity 2026:

- **30 Sep — Idea and evidence:** proposal, research results, and interactive prototype.
- **14 Oct — Working prototype:** serve HAMF through an API, connect the dashboard to live inference, and install a sky camera on a Dhaka rooftop.
- **24 Oct — Live demo at AIUB:** forecast from real Dhaka sky images and make an initial comparison against persistence.
- **After the event — Pilot:** fine-tune on local data, add an SMS gateway and satellite fallback, and trial with a factory and a mini-grid.

## Technology

Python, PyTorch, `pvlib`, FastAPI, Raspberry Pi, and Next.js are the technologies identified for the planned system. The current interactive demo is a self-contained HTML, CSS, and JavaScript file.

## Project

MeghDrishti is for **Hack for Humanity 2026, Track 1: Climate Resilience & Environmental Sustainability**.
