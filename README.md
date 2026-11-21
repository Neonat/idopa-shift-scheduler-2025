# iDopa

A shift-scheduling prototype built for the HacknRoll 2025 hackathon with two experiments: a React interface for selecting a weekday and a Python script for finding a valid assignment.

The browser and solver are independent. The repository captures the interface concept and constraint logic, with no API between them.

[Try the interface](#try-the-interface) · [Run the solver](#run-the-solver) · [Stack and structure](#stack-and-structure)

## Current behaviour

The browser displays a list of weekdays. You can select a day and press **Upload** to show a confirmation alert. The button does not upload a file, store availability, or request a schedule, despite the alert's wording.

The Python example defines four workers, four shifts, and each worker's availability in source. It enumerates candidate assignments and prints the first one that satisfies all three rules:

- Each shift has exactly one worker.
- Each worker receives at least one shift.
- Every assigned shift is within that worker's availability.

The script demonstrates feasibility checking. It has no fairness objective, input form, or file parser.

## Try the interface

Use Node.js with npm and a modern browser.

```sh
git clone https://github.com/Neonat/idopa-shift-scheduler-2025.git
cd idopa-shift-scheduler-2025
npm ci
npm run dev
```

Open the URL printed by Vite, normally [localhost:5173](http://localhost:5173). Select a weekday and click **Upload** to inspect the confirmation state.

## Run the solver

Python 3 and its standard library are sufficient. From the repository root:

```sh
python3 src/scheduler.py
```

A successful run prints `Valid schedule found:` followed by each worker's assigned shift. Change `workers`, `shifts`, and `availability` in [src/scheduler.py](src/scheduler.py) to explore the example. The exhaustive search materialises all binary assignment combinations, so its current design is for small inputs.

## Stack and structure

| Choice | Role in the prototype |
| --- | --- |
| React 18 and TSX components | Hold the selected-day and alert states and split the page into a list, header, button, and alert. |
| Bootstrap 5 and application CSS | Style the list, controls, and confirmation. |
| Vite 4 | Serve and bundle the interface. The build script does not run a separate TypeScript type check. |
| Python and `itertools.product` | Express assignment constraints directly without a solver dependency. |

| Location | Responsibility |
| --- | --- |
| [src/App.tsx](src/App.tsx) | Weekday list and alert visibility |
| [src/components/](src/components/) | Interface components |
| [src/App.css](src/App.css) | Application styling |
| [src/scheduler.py](src/scheduler.py) | Standalone assignment search |
| [vite.config.js](vite.config.js) | Frontend build configuration |

There are no API credentials or environment variables to configure.

## Build and development

```sh
npm run build
npm run preview
```

Vite writes the production files to `dist/` and prints the preview URL. The project has no automated test script. To assess a solver change, check that the printed assignment respects availability and both assignment rules.

Connecting the interface to the solver would require an availability data contract and a way to execute Python. Neither exists in the current app.

## License

No project license is included.
