# Discussion & Poll Prompts — Session 4

For the 15-minute Q&A. Steer toward the participants' own release / problem / configuration data — the goal is to move from "I saw four algorithms" to "I know where I'd point one." Each prompt notes what a good answer surfaces.

## Live poll (quick, hands-up or A/B/C)

**Poll 1 — "You have 50,000 host configs and zero labels. Which do you reach for first?"**
A) K-means B) DBSCAN C) PCA then look
- *Surfaces:* there's no single right answer — the point is *why*. If you want the fleet grouped and roughly know how many profiles exist → K-means. If you want the odd hosts flagged → DBSCAN (noise = anomaly). If a config has 200 fields → PCA first to make distance meaningful. Reward reasoning, not the letter.

**Poll 2 — "This t-SNE plot shows two blobs far apart. Are those two groups very different?"**
A) Yes B) No C) Can't tell from the plot
- *Surfaces:* the answer is C (leaning B). Between-cluster distance on a t-SNE/UMAP plot is not trustworthy. Catches whether the t-SNE trap landed.

## Discussion prompts

1. **"What in your world has lots of 'normal' and rare, varied 'weird'?"**
   *Good answers name:* config drift, incident streams, build/telemetry fingerprints, access-pattern logs. The pattern — abundant normal, rare novel outliers — is exactly where unsupervised anomaly detection beats a supervised classifier, because you can't label anomalies you've never seen.

2. **"An anomaly detector flags 6 hosts overnight. What do you do with that list — and what would make it useless?"**
   *Good answers surface:* a flag is a hypothesis, not a verdict; you need a human to triage; too many false alarms (threshold too tight, or "normal" drifted) destroys trust and the team stops looking. Connects to Session 13/13: never let a pipeline act on the flag without a qualified gate.

3. **"K-means will always return exactly the K clusters you ask for — even in pure noise. How do you protect yourself from fooling yourself?"**
   *Good answers name:* elbow + silhouette as cross-checks, looking at actual cluster members, domain sanity ("do these groups mean anything?"). The meta-point: unsupervised methods always answer, so skepticism is the real skill.

4. **"Why does forgetting to scale your features quietly wreck every method today?"**
   *Good answers explain:* K-means, DBSCAN, and PCA are all distance/variance-based; a feature in bytes swamps one in 0–1, so it silently dominates the clustering or the first principal component. It fails silently — no error, just a wrong answer.

5. **"When would you *not* use unsupervised learning for anomalies, and reach for a labelled classifier instead?"**
   *Good answers surface:* when you have a rich history of labelled failures and care about known failure modes, a supervised model can be more precise. Unsupervised shines for *novel* anomalies and when labels don't exist. It's a trade, not a hierarchy.

6. **"'t-SNE to see, PCA to compute.' Give an example of each from your own work."**
   *Good answers:* PCA to shrink a 200-field config vector before clustering or before feeding another model (compute); t-SNE/UMAP to make a one-slide picture of whether incidents fall into natural groups (see). Reinforces that they're for different jobs, often used together.

7. **"'Normal' drifts. What's your operational plan to keep an anomaly detector honest over 12 months?"**
   *Good answers name:* scheduled re-fitting, monitoring the false-alarm rate, validating flags against known incidents, versioning the model. Turns a demo into an operable control — squarely this audience's discipline.
