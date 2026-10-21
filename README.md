# Farm Machinery Repair Service: Model Description

## 1. Title and context
A service center in the Lowveld region of Mpumalanga repairs tractors and other agricultural machinery for local citrus farms. During the harvest season, it receives significantly more repair requests than in other months, and farmers report long waits between submitting a request and a mechanic starting work. These delays are costly: a broken tractor or forklift stops picked fruit from reaching the packhouse on time, which leads to quality losses and missed export deadlines. The center's management wants to reduce waiting times during peak season without permanently increasing staff costs.

## 2. Stakeholder
The service center manager, who plans staffing and dispatching. Secondary: the citrus farmers.

## 3. Decision
How to staff and organize dispatching during harvest season. Alternatives:
1. Hire 1–2 seasonal mechanics (6 or 7 instead of the current 5)
2. Introduce priority dispatching (urgent = machine stopped and needed for harvest; minor = everything else)
3. Both together

Future directions: travel time to farms, spare-parts delays, separate field and workshop crews.

## 4. Question
How will the share of urgent repair requests on which a mechanic starts work within 20 hours change during harvest season if the center operates with 5, 6 or 7 mechanics, under first-come-first-served versus priority dispatching?

## 5. Boundaries
**Start:** a repair request is registered by the dispatcher and classified as urgent or minor.
**End:** a mechanic is assigned and starts work on the request.

**Inside the model:**
- registration and urgency classification of requests
- the queue of waiting requests
- the dispatching rule (first-come-first-served or priority)
- assignment to a free mechanic
- the pool of mechanics (busy or free)

**External influences (affect the system but are not modeled):**
- harvest season and weather, which drive how often machines break down
- farmers' decisions to submit requests
- the mechanics' shift schedule

**Deliberately excluded:**
- repair duration and quality: the question is about waiting to start, not about fixing
- spare-parts availability: a separate problem needing separate data
- travel time to farms: simplified for the first version, although Lowveld farms are spread out, so this is a key future direction

**Consequence of the boundary:** because the model ends when a mechanic starts work, it can estimate the wait before service begins, but it cannot estimate total machine downtime, which also includes travel, diagnosis, waiting for parts and the repair itself. So the model cannot directly estimate how much fruit is lost.

## 6. Diagram
```mermaid
flowchart LR
    F(["Farmer: breakdown occurs"]) -->|"repair request"| R

    subgraph SYSTEM ["System boundary: service center"]
        R["Registration + urgency class"] --> Q[("Queue of waiting requests")]
        Q -->|"FCFS or priority rule"| A["Assignment to free mechanic"]
        M["Mechanics pool, c = 5 / 6 / 7"] -.-> A
    end

    A -->|"mechanic starts work"| OUT(["Repair begins (outside model)"])

    K1{{"KPI measured here: W = t_start - t_registered"}}
    A -.- K1
```
## 7. Quantities

## 8. KPIs

## 9. Assumptions
| Assumption | If it is wrong |
|---|---|
| Every mechanic can repair every type of machine | Waits for specialized machines (e.g. packhouse forklifts) are underestimated, because only some mechanics can take them |
| Urgency is classified correctly at registration | If farmers mark minor faults as urgent to get faster service, priority dispatching loses its effect and the KPI for urgent requests looks worse than it should |
| Requests stay in the queue until assigned (no cancellations) | If farmers give up and repair machines themselves or call another workshop, the real queue is shorter than the modeled one |
| All 5–7 mechanics are available for the whole shift | Sick days or long trips to distant farms reduce real capacity, so the model is too optimistic |

## 10. Models

## 11. Verification

## 12. Limitations
1. **No estimate of fruit losses or money.** The model measures waiting time before a mechanic starts, not total downtime or how much citrus is lost. It can show which scenario reduces waits, but not whether hiring a seasonal mechanic pays for itself.

2. **Results apply to harvest-season conditions only.** The model is set up for peak-season request rates. Its conclusions cannot be transferred to the off-season or to a harvest with very different breakdown rates (for example, after an unusually wet season).

3. **Travel time is ignored.** Lowveld farms are spread out, so a mechanic travelling to a distant farm is unavailable for longer. The model may overestimate how many requests each mechanic can take on, so the real benefit of 6 or 7 mechanics may differ.

4. **The model compares scenarios; it does not predict exact numbers.** Because of the simplifying assumptions (section 9), results should be read as "scenario A is better than scenario B by roughly this much", not as exact waiting times farmers will experience.

