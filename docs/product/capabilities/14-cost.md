# 5D cost

5D cost attaches money to the model and the schedule: cost items are priced from rates and
quantities, assigned to tasks and rolled up over time. Costs come from takeoff, from the schedule
source or from manual entry, and remain traceable to the scene items they price.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Cost breakdown | Name, currency, code tree (structure levels) | Contains cost items |
| Cost item | Code, description, cost category (material, labor, equipment, subcontract, other), fixed amount or unit rate times quantity, quantity source, currency | Belongs to a cost breakdown; may be derived from takeoff items; may be assigned to tasks |
| Rate | Resource or item, unit, unit cost, currency, effective date | Used by cost items and takeoff formulas |
| Cost assignment | Cost item, task, share or distribution (front loaded, even, back loaded, by progress) | Links cost items to tasks |
| Cost baseline | Name, date, frozen cost and schedule values | Compared with the current state |

## Cost data

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-COST-001 | Task cost fields | Tasks carry cost per category (material, labor, equipment, subcontract, other) as fixed amounts or as unit rate times a quantity; the task total and the roll-up to parent tasks are computed. | T1 | 3 |
| RV-COST-002 | Cost from schedule source | Cost fields are imported with the schedule through the field mapping and updated on synchronization unless locally overridden. | T2 | 3 |
| RV-COST-003 | Cost breakdown structure | A hierarchical cost breakdown with codes and descriptions can be created or imported from CSV or XLSX; totals roll up through the code tree. | T2 | 3 |
| RV-COST-004 | Rates | A rate table holds unit costs per resource and unit with an effective date; changing a rate recomputes every dependent cost item and task. | T2 | 3 |
| RV-COST-005 | Costs from takeoff | Cost items take their quantity from takeoff items, so a quantity change after a source update reprices the cost item and its tasks (takeoff in [15-takeoff.md](15-takeoff.md)). | T1 | 3 |
| RV-COST-006 | Currency and units | Costs carry an ISO 4217 currency per breakdown; mixed currencies are rejected in one roll-up unless a conversion rate table is defined. | T2 | 3 |

## Cost over time

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-COST-007 | Cost assignment to tasks | Cost items are assigned to tasks in whole or in shares, with a distribution over the task duration (even, front loaded, back loaded or by recorded progress). | T1 | 3 |
| RV-COST-008 | Cumulative cost curve | A chart shows planned and actual cumulative cost over time and cost per period, synchronized with the simulation date. | T1 | 3 |
| RV-COST-009 | Cost in simulation | The simulation overlay shows cost spent to date and cost of active tasks, and items can be colored by cost per unit or cost category. | T2 | 3 |
| RV-COST-010 | Cost baselines | The current cost and schedule state can be frozen as a named baseline; a comparison lists changes in totals per code and task and plots baseline against current curves. | T2 | 3 |
| RV-COST-011 | Earned value figures | With progress records, planned value, earned value and actual cost are computed per task and period, with schedule and cost variance. | T3 | 5 |

## Reporting

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-COST-012 | Cost summary | A summary rolls up costs by breakdown code, task, category, level or any item property and exports to CSV, XLSX and JSON. | T1 | 3 |
| RV-COST-013 | Item cost traceability | Selecting a cost item highlights the scene items that contribute its quantity; selecting an item lists the cost items it contributes to. | T2 | 3 |
| RV-COST-014 | IFC cost export | Cost items and assignments are exported as IFC cost schedules and cost items related to the referenced elements by GlobalId. | T3 | 3 |
