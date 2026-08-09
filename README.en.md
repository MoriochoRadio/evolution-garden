# Garden of Evolution (진화의 정원)

🇰🇷 [한국어](README.md) · 🇬🇧 English

**An artificial life ecosystem simulator where neural networks nobody ever trained learn to hunt on their own.** Every creature is born with a small neural network (brain) of 137 weights, and from nothing but mutation and natural selection, food tracking, speciation (herbivore → predator), and predator-prey oscillations emerge. Pure vanilla JavaScript + Canvas 2D, in **a single HTML file**.

**[▶ Run it right in your browser](https://moriochoradio.github.io/evolution-garden/)** — no install, no build

## What You Can Watch

- **Early on**: most creatures with random brains wander around and starve, while the few that happen to have "turn toward food" brains survive and reproduce. Within a few generations, food tracking becomes unmistakable.
- **Midgame**: diet is a continuous 0–1 gene. **When the diet distribution chart splits in two, that's speciation** — a predator lineage has branched off from the herbivore population. A scavenger lineage that eats carcasses builds the evolutionary bridge from herbivory to carnivory.
- **Long term**: more predators means fewer herbivores, which then starves the predators too — **Lotka-Volterra oscillations** appear in the population chart.
- **Observation tools**: population / diet distribution / trait map charts, real-time brain activation visualization when you click a creature, and an event log.
- **The hand of god**: scatter food, ⚡ thin out the dominant species with lightning, tweak environment variables like mutation rate and plant growth rate, and save/load the world as JSON.

## Why It's Built This Way — Tech Choice Q&A

**Q. Why vanilla JS in a single HTML file?**
The attraction of this project is the simulation itself, not the UI. Stripping out frameworks, build tools, and external libraries down to one file (`index.html`, about 950 lines) means anyone can run it offline with a single double-click. Zero dependencies was both a demo-accessibility feature and a constraint: rendering (Canvas 2D), the neural network, and spatial partitioning are all written from scratch.

**Q. Why does the neural network have only 137 weights?**
The architecture is a fully connected network of 12 inputs (direction/distance/size/diet of the nearest food and neighbor, own state, etc.) → 9 hidden → 2 outputs (turn, thrust), which with biases is `9×(12+1) + 2×(9+1) = 137` weights. Hundreds of creatures have to run a forward pass every tick, so it must be small to stay real-time — and above all, the point was to show that "even a brain this small learns to hunt with no training, from selection pressure alone", so it was deliberately kept minimal.

**Q. Why is there no learning (backpropagation)?**
A creature lives its whole life with the brain it was born with. Creatures that move well reproduce, and their children's brain weights just get Gaussian mutations. The moment you design a reward function, you get "the behavior I defined" — so this is an experiment in what emerges from pure natural selection alone.

**Q. Why isn't speciation hardcoded?**
If herbivore/carnivore were implemented as discrete species, speciation would be a 'feature', not 'emergence'. Instead, diet is a continuous 0–1 spectrum, and carcasses (meat) create an intermediate food niche that keeps the omnivore stage viable. As a result, you can watch on the chart the moment the distribution splits on its own.

**Q. How was performance handled?**
Neighbor search is the bottleneck, so a spatial hash grid queries only nearby cells (avoiding O(n²) all-pairs comparison). In the hot loop, query result arrays are reused to reduce allocations.

## Structure

```
index.html
 ├─ Part 1. Simulation core  — no DOM dependency (genes, neural network, ecosystem logic)
 └─ Part 2. UI               — Canvas rendering, camera, charts, inspector, hand of god
```

The core and the UI are separated for the sake of 'Verification' below.

## Running

Just open `index.html` in a browser. (Or use the demo link above.)

## Verification

Because the simulation core is decoupled from the DOM, it can run headless in Node.js for long stretches.
A 60,000-tick test confirmed 90+ generations of evolution with no extinction, a 3x increase in average movement speed, and predator speciation.

## Related Project

**[Mindlings](https://github.com/MoriochoRadio/mindlings)** (Godot 4), by the same author, pushes the "neural networks + natural selection" theme explored here in a different direction — **NEAT-style topology evolution + within-lifetime learning** instead of a fixed-topology 137-weight brain, and a god-view sandbox where you care for a handful of creatures up close instead of a swarm of hundreds.

## License

MIT
