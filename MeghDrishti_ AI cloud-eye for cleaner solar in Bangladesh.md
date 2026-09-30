[MeghDrishti মেঘদৃষ্টি](#top)

[Live demo](#demo)[How it works](#how)[Evidence](#evidence)[Roadmap](#roadmap)

# See the cloud before the power drops.

মেঘ আসার আগেই দেখুন, ডিজেল পোড়ানোর আগেই ভাবুন।

MeghDrishti watches the sky with a low-cost camera and tells solar operators in Bangladesh what the next two hours will bring, so they stop running diesel gensets just in case.

[Watch the live demo](#demo) [See how it works](#how)

- **Monsoon clouds move fast**A rooftop plant can lose half its output within minutes, then recover just as quickly.
- **Diesel runs just in case**With load shedding and costly fuel imports, factories keep gensets on standby because they cannot see a drop coming.
- **Hourly forecasts can't see a cloud**A sky camera can. In our research it gave the biggest gain on partly cloudy days.

Site Demo mode: simulated sky and power, shaped on our SKIPP'D results. Live model connects in Round 2.

CAM-01

Scanning the sky.

Rooftop fisheye camera, one image per minute. The dashed line marks a cloud on course for the sun.

09:00

### Solar output, next 2 hours

[ ] Show what actually happened

Measured Forecast median 50% range 90% range Persistence Clear-sky limit

### Diesel kept off today

Compared with keeping the genset on standby whenever the sky is not clear.

**0**litres of diesel saved

**0**BDT saved

**0**kg CO₂ avoided

**0**standby hours avoided

Edit assumptions

Diesel price (BDT per litre) Genset standby burn (litres per hour)

CO₂ uses 2.68 kg per litre of diesel. MeghDrishti puts the genset on standby only during red alerts.

### What the model looks at

Share of attention per source as the forecast looks further ahead.

Sky images Recent power Weather forecast

Pattern from our paper (Fig. 6): the camera matters more for longer leads and on cloudy days. Weights are descriptive, not causal.

### Operator alerts in Bangla

Sent by SMS or app when the advice changes.

মেঘদৃষ্টি সতর্কবার্তা

### Panel cleaning advisor

On clear periods, measured output should match the physics-based clear-sky limit. A growing gap means dust on the panels.

Days since last cleaning: 14

Dust loss per day (%) Electricity value (BDT per kWh) Cost of one cleaning (BDT)

Dust loss rate is an assumption until measured at the Dhaka pilot site.

## From one sky photo to one clear instruction

Every five minutes MeghDrishti runs the same loop on a small computer at the site. It keeps working on a weak internet connection because the model is small enough to run locally.

1. **Capture**A Raspberry Pi with a fisheye lens photographs the whole sky every minute, for under BDT 15,000.
2. **Correct**Timestamps are checked against the sun's path, and pvlib computes the clear-sky limit for the site.
3. **Forecast**HAMF fuses the last three sky images, 60 minutes of power and the clear-sky prior into 19 quantiles for 5 to 120 minutes ahead.
4. **Decide**Rules turn the forecast range into one action: charge the battery, delay a load, keep diesel off, or prepare the genset.
5. **Alert**The operator gets the action in Bangla on the dashboard and by SMS, with how confident the forecast is.

## Built on tested research, with honest limits

The forecasting model comes from our research on SKIPP'D, a public dataset of sky images and PV power (517 days). The paper is under double-blind review.

| Model (20 test days) | RMSE (kW) | Skill vs persistence | Probabilistic skill | 90% range width |
| --- | --- | --- | --- | --- |
| HAMF (ours) | 3.38 | 17.9% | 34.8% | 5.6 kW |
| TiDE-style dense model | 3.47 | 15.8% | 31.3% | 6.6 kW |
| Probabilistic persistence | 4.12 | 0.1% | 0.0% | 9.8 kW |
| Smart persistence | 4.12 | 0.0% | −16.6% | – |
| SUNSET CNN (benchmark) | 4.25 | −3.1% | −80.9% | – |

HAMF beats persistence and the SUNSET benchmark with p \< 0.001. Its 90% range is 43% narrower than probabilistic persistence while covering more observations (87.6% vs 83.4%).

**328,309** records in the public dataset carry a daylight-saving timestamp error that we found. Left uncorrected, it inflates skill by about 7 points. MeghDrishti checks every site's clock against the sun before training.

### What we still need to prove

- The model was trained at one site in California. Performance in Bangladesh is untested until our Dhaka camera pilot.
- The gain over a strong dense baseline (TiDE-style) is not statistically significant (p = 0.20).
- Packaged weather forecasts added no skill at these horizons. The camera does the heavy lifting.
- Diesel and CO₂ numbers in this demo come from the stated assumptions, not field measurements.

## Plan through the Grand Finale

Each round moves one step from research result to a working system on a Bangladeshi rooftop.

1. 30 Sep**Idea and evidence**Proposal, research results and this interactive prototype.
2. 14 Oct**Working prototype**HAMF served through an API, this dashboard on live inference, and a sky camera installed on a Dhaka rooftop.
3. 24 Oct**Live demo at AIUB**Forecasts from real Dhaka sky images and a first local comparison against persistence.
4. After**Pilot**Fine-tune on local data, SMS gateway, satellite fallback for sites without a camera, and trials with one factory and one mini-grid.

MeghDrishti, Hack for Humanity 2026, Track 1: Climate Resilience & Environmental Sustainability. Python, PyTorch, pvlib, FastAPI, Raspberry Pi, Next.js